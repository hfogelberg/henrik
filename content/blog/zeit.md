---
title: "Zeit"
date: 2017-10-04T16:16:12Z
draft: false
---
I've mainly been hosting my personal projects on Heroku the last few years. The good news is that it's reliable and easy to use. The bad news is the price. The free plan has always had the limitation of the deployment going to sleep after some time without any traffic. The time before going to sleep has now dropped to 30 minutes and with a wake-up time of up to 10 seconds it isn't really usable. The Hobby plan is OK for light weight production apps, but comes at a price tag of 7$ per site.

## Digital Ocean
I tried Digital Ocean for a while and did enjoy their service. It feels very solid with good documentation. However there is a bit of an IKEA feel to it. It's cheap (starting at 5$ per month), but I have to do it all myself. I need to handle SSL certificates, think of securty, know when to patches etc. If the site takes of I'll need a load balancer too! It's a lot of work! I want to write code and be done with it.

I have the highest respect for the people who work with hosting. The job is not trivial. It isn't anything to be done as fast as possible with the left hand. Not if you want to stay unhacked and have a site that runs smoothly.

## Zeit
Looking around for alternatives i ran in to [Zeit](https://zeit.co/) and their hosting service Now. They aren't completely new, but started some time in the spring of 2016. What drew my attention to them is the founder and lead developer Guillermo Rauch. He is a well known name in the JS/Node world and has been involved in for instance Mongoose and Socket.io. The stuff he works on is usually of incredibly high quality. 

## Deploying
To use Now you need to install the CLI. Pushing the code couldn't be easier. Just cd in to the directory and type `$ now`. Done. If you need to set environment variables, just pass them as `$ now -e API_TOKEN=dnjkvndskjhds`. For a static blog written in Hugo (like this) I needed to do a little more:
````
$ hugo
$ cd public
$ now
````

It must have taken five seconds

I've also deployed several dockerized apps written in Golang which is just as easy. The only gotcha was that the port had to be exposed in the Dockerfile, like so: 
````
FROM golang:1.8-onbuild
EXPOSE 80
````

Zeit has a free plan to the service with the main limitation that custom domain names are not available. Since I didn't want my site name along the lines of https://public-gkcchbhmyh.now.sh I soon upgraded to the premium plan at 15$ per month. This includes five custom domain names and ten concurrent instances running. For low traffic sites one site can run on one instance.

## Custom domains
So how do I point my custom domains to Now? Again, easy! What I had to do was to set the DNS server to Zeits, like so: 
````
london.zeit.world
california.zeit.world
````

They have around ten DNS servers around the world, but I settled for these two for now. 

Secondly, I just had to alias my deployment: `now alias public-gkcchbhmyh.now.sh henrikfogelberg.com`.




