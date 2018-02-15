---
title: "Iterate and Update MongoDb Docs"
date: 2015-10-13T18:08:23Z
draft: false
---
I was working on a project with an existing Mongo database where I was going to add a search function. There were pretty much three different options I could think of:

1. Writing some more or less complicated code to allow wild card search in a couple of different fields.
2. Adding a few different packages to enable full text search.
3. Faking it!

Option 1 seemed like it could turn in to a maintenance night mare, option 2 felt like overkill in this case. Faking it seemed like a good idea :-).

````
db.artworks.find().forEach(function(data){ 
	var p = data.title + " " + data.description + " " + data.tags; 

	p = p.toLowerCase();
	db.artworks.update({_id: data._id}, {$set:{searchString: p}})
});
````
