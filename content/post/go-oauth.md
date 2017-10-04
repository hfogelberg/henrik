+++
date = "2017-08-19T15:54:56+01:00"
title = "Oauth in Golang"
menu="main"
+++
The sample code for the tutorial is available on [Github](https://github.com/hfogelberg/go-auth-demo).

Years ago when I first came in contact with Oauth it seemed like magic. I just plugged it in and didn’t think too much about what was going on and why. The truth is that there is no magic involved and it really is quite simple.

There are several packages around for Oauth in Golang, for instance Goth. However if you want to reinvent the wheel and build a version of your own from the ground up it really is quite straight forward. And even if you use a ready made component it’s good to know how it actually works.

The basic flow is:
1. Call an authentication provider who will redirect the user to a login page.
2. When the user has logged in with his/her credentials an access token is returned.
3. Call the authentication provider again with the access token and the email address, name, profile picture or whatever information they provide and you are entitled to have is returned.

## Setting up the Go project
First of all just scaffle the project and set up som basic routing.
````
$ mkdir go-auth-sample
$ cd go-auth sample
$ touch main.go
````

Open main.go in whatever text editor you use and just build up the basic structure. Nothing strange here really:
````
package main
import (
  "fmt"
  "net/http"
  "github.com/gorilla/mux"
)
func main() {
  router := mux.NewRouter().StrictSlash(true)
  router.HandleFunc("/", indexHandler)
  router.HandleFunc("/login", loginHandler)
  router.HandleFunc("/callback", callbackHandler)
  http.ListenAndServe(":3000", router)
}
func indexHandler(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintln(w, "Index")
}
func loginHandler(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintln(w, "Login")
}
func callbackHandler(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintln(w, "Callback")
}
````

Just run go get if you don’t have Gorilla mux on the machine, start it up and give it a quick try. If you’re fed up with killing and restarting Go whenever you change the code you can use Fresh for the job. It works much the same as Nodemon if you come from the world of Node like I do.

## Setting up Google Auth
For the example we’ll use Google as the authentication provider. I assume most Go developers have a Google account.

I have a love-hate relationship to the Google Cloud Platform. You can do more or less anything and there’s a ton of amazing tools that are free or at least reasonably priced. It is however a nightmare to navigate through and enabling Ouath is no exception. Nobody on the team seems to have heard of usability.

Anyway, head over to the Google Cloud Console. In the blue menu bar click on Select a Project. Either reuse some old test project you have lying around or create a new one for the tutorial. There is a limit to the number of free projects, so by all means reuse one if it’s possible.
In the dashboard select Enable APIs and Get Credentials, under Getting started.

In the new window click on Credentials, which is in the menu bar to the left. Then click Create Credentials. In the drop down select Oauth Client Id.

Select Web application, give it a name and fill in `http://localhost:3000` and `http://localhost:3000/callback` as origins and and callback URI respectively. Click on create. Be sure to copy and save the client ID and client secret.

If you’re on a Mac or *nix open the .bash_profile and add two new lines:
![Bash](/bash.png)

Back to the code
Time to update the code with a couple of imports
´´´´
import (
  "fmt"
  "net/http"
  "github.com/gorilla/mux"
  "golang.org/x/oauth2"
  "golang.org/x/oauth2/google"
)
´´´´

We’re using two of the standard libraries in Google for authentication that you may need to go get.
Next configure Google auth by adding this variable right after the import:
´´´´
var googleOauthConfig = &oauth2.Config{
  RedirectURL: "http://localhost:3000/callback",
  ClientID: os.Getenv("GOOGLE_CLIENT_ID"),
  ClientSecret: os.Getenv("GOOGLE_CLIENT_SECRET"),
  Scopes: []string{
    "https://www.googleapis.com/auth/userinfo.profile",
    "https://www.googleapis.com/auth/userinfo.email"},
  Endpoint: google.Endpoint,
}
´´´´

This might look a bit of a mess, but we’ll take it step by step. First of all we specify the callback url when the authentication has succeeded. This has to match whatever you specified in the Google Cloud Platform. Second, read the client id and client secret from .bash_profile. Next, we specify that we want access to the logged in user’s email and profile. You can add more permission requests here. Personally I however think twice about clicking on Accept if they want all my contacts, shoe size and bank account number. Finally, the end point is a constant defined in the libraries we imported.
Just update the index handler and login handler:

´´´´
func indexHandler(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintln(w, "<a href='/login'>Log in with Google</a>")
}

func loginHandler(w http.ResponseWriter, r *http.Request) {
  oauthStateString := uniuri.New()
  url := googleOauthConfig.AuthCodeURL(oauthStateString)
  http.Redirect(w, r, url, http.StatusTemporaryRedirect)
}
´´´´
Give it a try. Click on Login and you should see a page similar to this:
![BasSign inh](/sign-in.png)

Click on the user you want to log in with and you should be redirected to the callback page. Obviously nothing much will happen there, but that’s the next thing on the todo-list.

## Handling the callback
At the start of the tutorial I wrote that the authentication provider will return an access token when the user has been successfully logged in. So, let’s see if it does. Modify the callback function like below, rerun the application and try to log in again. You should see the access token in the web browser.

´´´´
func callbackHandler(w http.ResponseWriter, r *http.Request) {
  code := r.FormValue(“code”)
  token, _ := googleOauthConfig.Exchange(oauth2.NoContext, code)
  fmt.Fprintf(w, token.AccessToken)
}
´´´´

I leave out the error handling to keep the code a bit shorter. You should also call the method token.Valid() to check the token. A more complete code example is available on Github.
Getting user information
Just having the token doesn’t do much good. We also want to know who is logged in. So, for step three we need to make a second call, passing the token as a parameter. The response will contain a json string with data we want. First of all we’ll add a struct with user info:

´´´´
type GoogleUser struct {
  ID string `json:"id"`
  Email string `json:"email"`
  VerifiedEmail bool `json:"verified_email"`
  Name string `json:"name"`
  GivenName string `json:"given_name"`
  FamilyName string `json:"family_name"`
  Link string `json:"link"`
  Picture string `json:"picture"`
  Gender string `json:"gender"`
  Locale string `json:"locale"`
}
´´´´

Next, call the Google Oauth API to fetch the user data and unmarshal.
´´´´
response, _ := http.Get(“https://www.googleapis.com/oauth2/v2/userinfo?access_token=" + token.AccessToken)
defer response.Body.Close()
contents, _ := ioutil.ReadAll(response.Body)
var user *GoogleUser
_ = json.Unmarshal(contents, &user)
And finally print it all out on the screen
fmt.Fprintf(w, “Email: %s\nName: %s\nImage link: %s\n”, user.Email, user.Name, user.Picture)
´´´´

And finally print it all out on the screen
fmt.Fprintf(w, “Email: %s\nName: %s\nImage link: %s\n”, user.Email, user.Name, user.Picture)

## Summary
OK, it's quite a bit of code to get it working, although it is quite straight forward. If you need more providers - which you will- the code will obviously be proportionally larger. It makes a lot of sense to use a ready made component.

There isn't really one around that suits my needs completely, so I'm working on one of my own at the moment. Soon I'll let you know more.
