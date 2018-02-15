---
title: "Middleware for Selected Routes in Golang"
date: 2017-10-07T10:00:53Z
draft: false
---
You know the situation: you're building a  web site where the user needs to log in to reach some (most?) of the pages. So, how do check if they are logged in? I a smooth, easily manageable way of course. The answer in Go is to use middleware. There are a few different ways to do it in, but after a bit of googling and some trial and error this is the way I tend to do it. Since an example says more than word let's get to it.

Although you can do a lot with Go's standard packages you need to pull gorilla/mux and urfave/negroni for this one.  Gorilla has a few well built and widely used packages and mux is their widely used used routing solution. Negroni is a package to chain and handle middleware, which comes with a few built in functions such as route logging and recovery. Without further adoo:

````
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/mux"
	"github.com/urfave/negroni"
)

func main() {
	r := mux.NewRouter().StrictSlash(false)
	r.HandleFunc("/", indexHandler)
	r.HandleFunc("/about", aboutHandler)

	http.ListenAndServe(":3000", r)
}

func indexHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "<h1>Index</h1>")
}

func aboutHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "<h1>About</h1>")
}
````

Just run it with `go run main.go` to give it a sanity check. There shouldn't be any surprises.

Now we'll add the admin section of the site, which we'll later monitor with the middleware. Of course this tutorial won't go in to that, so we'll stick to "pretend-login". The admin pages are added as a subroute, which is one nice feature of gorilla/mux. Add these lines just below the routing to index and about in the main function:

````
adm := r.PathPrefix("/admin/").Subrouter()
adm.HandleFunc("/index", adminHandler)
adm.HandleFunc("/create", createHandler)
````

Obviously we need to handle the routing for the new routes:

````
func adminHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "<h1>Admin</h1>")
}

func createHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "<h1>Create</h1>")
}
````
## So, When Will You Get to the Point?
Ok, with that in place, time for the interesting stuff; setting up the middleware.
````
mux := http.NewServeMux()
mux.Handle("/", r)
mux.Handle("/admin/", negroni.New(
	negroni.HandlerFunc(adminMiddleware),
	negroni.Wrap(r),
))
````
Set up a mux server to handle the routing for all routes. However all subroutes to admin will be handled by the middleware. For this we use Negroni, which is an excellent package for managing and chaining middleware. You could of course handle a whole set of different subroutes; regular users, vip users, admins, etc ...

So now for the middleware. To keep things simple, just log the request. In a real-world application this would of course be a good place to authenticate the user.
````
func adminMiddleware(w http.ResponseWriter, r *http.Request, next http.HandlerFunc) {
	fmt.Println("Middleware has checked that everything is OK!")
	next(w, r)
}
````
And finally, just kick of the sever and try it out. Negroni classic ads some useful tools, such as logging and recovery handling. We'll just throw that in too.
````
n := negroni.Classic()
n.UseHandler(mux)
http.ListenAndServe(":3000", n)
````

The complete sample code is available on [Github](https://github.com/hfogelberg/middleware-demo)

