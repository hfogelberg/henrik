---
title: "Introducing TooGo"
date: 2017-09-20T08:42:03Z
draft: false
---
Ever so often I end up copy-pasting small code functions. This is OK, but it is a bit tedious and in pretty much every web project it's the same handfull of functions I use. So I just dumped them in a package called [TooGo](https://github.com/hfogelberg/toogo)  (Tools for Go )and they are ready to plug in. Currently there isn't very much in it, but the idea is to keep it growing and add more features as I come up with more useful snippets.

## Getenv
Read enviroment variables and use a default value if it's missing. 
`port := Getenv("PORT", ":3000")`

## FaviconHandler
Use in routing to serve the favicon. 
`router.HandleFunc("/favicon.ico", faviconHandler)`

## RoundToTwo
Takes a float64 and returns a string, rounded to two decimal points.

## TimeToDay
Takes a unix time in int format and returns the day, abbrevated to three letters.

## TimeToHour
Takes a unix timestamp in int format and returns the time in 24 hour format.
