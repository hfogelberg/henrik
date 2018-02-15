---
title: "Using Layouts in Golang"
date: 2017-05-18T18:12:41Z
draft: false
---
The templates in Golang are really smooth to work with and pretty intuitive. One thing took me a bit longer to figure out though: How do I use a base layout? You know, a Master page (as it's called in .NET), with the menu, header and footer etc?  It turns ut that it can be done and is quite easy. So, for a quick example:

layout.html
````
{{define "layout"}}
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>My beautiful site</title>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="/public/styles/master.css">
    </head>
  <body>
    <div class="page">
      <header>
        My header
      </header>

      <div class="container">
        {{template "content" .}}
      </div>

      <footer>
       My footer
      </footer>
    </div>
  </body>
</html>
{{end}}
````

index.html
````
{{define "content"}}
  <div class="index">
    <h1>Hello World</h1>
  </div>
{{end}}
````

about.html
````
{{define "content"}}
  <div class="index">
    <h1>This Page is a Demo of Hello World</h1>
  </div>
{{end}}
````

And now for the rendering
````
func indexHandler(w http.ResponseWriter, r *http.Request) {
	tpl, err := template.New("").ParseFiles("templates/index.html", "templates/layout.html")
	if err = tpl.ExecuteTemplate(w, "layout", nil); err != nil {
		log.Printf("Error serving index template %s\n", err.Error())
		return
	}
}
````