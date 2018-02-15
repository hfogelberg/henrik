---
title: "Trumpscrape"
date: 2017-09-16T17:38:04Z
draft: false
---
As a bit of a hobby I am learning the basics of machine learning. Quite a common way of getting data to work with is web scraping. So obviously it was time to figure out how to do it in Go.

It turned out to be really easy thanks to the excellent package [goQuery](https://github.com/PuerkitoBio/goquery).

The whole idea behind goQuery is to mimic jQuery, but -obviously- in Golang. I haven't used much jQuery the last few years, but felt at home quickly and thanks to the good docs it didn't take long to build a simple app.

[Omni]("http://omni.se") is a major Swedish on line new site and I usually read it on my mobile in the mornings. I though it would be a good source for my little app. Since Trump is in the new pretty much every day why not check how often he was mentioned? Whatever data I find I'll save to MongoDb. Perhaps I'll do something with it some day?

## The code
I'll just give a quick outline of how to do web scraping here. The code is available on [Github](https://github.com/hfogelberg/trumpscrape) if you want to check it out. It really is quite simple!

First of all you need to instantiate goQuery and point it to the url that should be scraped `doc, err := goquery.NewDocument(url)`. Second, 

Secondly, scrape! 

````
	doc.Find("article a.article-link h1").Each(func(index int, item *goquery.Selection) {
		linkTag := item
		link, _ := linkTag.Attr("href")
		linkText := linkTag.Text()

		if strings.Contains(linkText, Substr) {
			numTrumps++
		}
	})
  ````

It can of course be a bit tricky to target the data in the Find clause. In my case it was quite straight forward. 


And that's it.

By the way, Trump was mentioned twice on the first page this morning.