---
title: "Using Semantic With Meteor"
date: 2015-10-01T14:02:08Z
draft: false
---
I’ve been using Bootstrap for quite a few years in pretty much all of my web applications. Bootstrap is great in many ways. The grid layout makes life easy and you get a responsive site for free.

Bootstrap is a bit boring though. Everybody including your grandma is using it and you see so many Bootstrap sites out there. To a trained eye it doesn’t even take a second to see that Bootstrap was involved. The top nav bar, the colors on the buttons, the well class. Got you!

For my latest project I decided to take a look at Semantic instead and I like it. Semantic isn’t as widely spread (yet) as Bootstrap and the design is just a bit nicer I think. Some of the components are more ambitious than Bootstrap’s. Also the theaming is better, so it isn’t as obvious that Semantic is used as it often is with Bootstrap.

Getting it to work with Meteor was a bit of a hassle though and the installation is a bit hacky, unfortunately:

1. meteor add semantic:ui
2. Touch client/libs/semantic/custom.semantic.json.
3. start meteor
4. stop meteor
5. remove hidden file
6. start again

The entire Semantic download is huge and I wouldn’t use all of it for production. For development it’s fine though. For production you have two options. Either adding the specific components you need individually, eg `meteor add semantic:buttons`.

Or (a better option imho) to edit custom.semantic.json. The file is a regular json file, listing the components and themes in Semantic. Just set true or false to the parts you want an rerun the installation.



