---
title: "Why Go?"
date: 2017-06-01T13:05:45Z
draft: false
---
Go is an open source programming language, created by Google. The plan was launched in 2007 and the project announced in 2009. Version 1.0 was released in 2012. So the language is quite new, but has already achieved quite a large following.


There are many similarities between Go and C. In many ways the goal was to make a more modern version of C. This really is no coincidense since the brains behind GO, Rob Pike and Ken Thompson, were heavily involved in creating the C programming language. Ken Thompson also contributed to Unix, UTF-8 and regular expressions!


However Go is not just a clone of C. It borrows heavily from other languages and more modern programming concepts.

Go isn't just a programming language, but a whole range of tools: the compiler (of course), package handling, a formatting tool, a testing framework, etc. All of it packaged in a nice, accessible, zero configuring environmnet.

Coming most recently from Node and the world of Javascript this is something I very much appreciate. JS is fun in many ways, but there's a lot of messing about and tinkering before any code can be written. Go is more in the line of the late Steve Jobs: "It just works".

## Hello World
And of course we just have to start with Hello World. 

````
package main

import "fmt"

func main() {
  fmt.Println("Hello World)
}
````

Some points to note:

* In Go you must specify which package the code belongs to. By default the program always starts in the main package and there must be a main package in the project.
* Import, obviously, describes which additional packages are used. Fmt is a package that handles string formatting. You must import exactly the packages you need otherwise the code won't build.
* Function main is the starting point of the program. There must always be a func main to kick of the program.
* "fmt.Println" prints a line of text with a carriage return.

### Run and build
* `$ go run main.go` will run the code in the file main.go.
* To build a project use `$ go build`, which will build the solution in the current directory. The build file will have the same name as the directory. So if the sample code is i the directory hello-world the build file will be named hello-world. Run the application by typing `.\hello-world` at the command lin.


