---
title: "Deploy Meteor on Heroku"
date: 2015-10-16T09:42:46Z
draft: false
---
I’ve used Heroku to host quite a few applications by now. I developed in Rails previously and first heard of Heroku through the Rails community, where it’s a popular hosting option.  

Heroku doesn’t natively support Meteor. They do however support Node.js. Since the server side part of Meteor of course is based on Node it will run flawlessly on Heroku if you just follow some simple steps:

1. Sign up to [Heroku] if you haven’t done so already and create a new project. There are good instructions on Heroku’s site so I won’t go in on any details
2. Install the Heroku toolbelt. Again, there are good instructions on Heroku’s site
3. Commit your Meteor project to Git.
4. Add Heroku to your git-config as so
''heroku git:remote -a YOUR_PROJECT_NAME
5. Now you need to add a build pack to Heroku.  As I mentioned Heroku doesn’t natively support Meteor so you need a buildpack to allow your Meteor app to run. There are a few of the around, but I’ve used Horse successfully.
````
heroku buildpacks:set https://github.com/AdmitHub/meteor-buildpack-horse.git
````
6. You of course need a Mongo database for your Meter app. The easiest option id to use the Mongolab addon. By default you’ll be on the free sandbox plan.
````
heroku addons:create mongolab
````
7. Configure the root url
````
heroku config:set ROOT_URL=https://YOUR_PROJECT_NAME_.herokuapp.com
````
8. And now you can push you app to Heroku. It takes a couple of minutes to build, so some patiences is needed.
````
git push heroku master
````
9. Finally, open your site in the browser:
````
heroku open 
````
10. Bonus step. If you have custom settings, for instance store images in aws or something these should be stored in a settings.json file. This is the way to set it up on Heroku:
````
heroku config:add METEOR_SETTINGS="$(cat settings.json)"
````

