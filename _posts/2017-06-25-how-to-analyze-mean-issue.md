---
layout: post
title: How to configure Nginx in production to serve an Angular app and reverse proxy NodeJS
description: "Install Nginx on Ubuntu Server, understand configuration files, configure SSL, serve static files, 
 reverse proxy Keycloak and NodeJS servers"
author: ama
permalink: /ama/how-
published: false
categories: [mean]
tags: [codingmarks, nginx, ssl, tls, cerbot, keycloak, nodejs]
---

First when you need to optimize, it is usually great news. And indeed it is we managed have now over 1 MB of really great programming resources on the 
[codingmarks.org](http://codingmarks.org) website. We've reached that by importing [free-programming-books](https://github.com/EbookFoundation/free-programming-books) directory into
[codingmarks.org](http://codingmarks.org) - see the [codingmarks-free-programming-books-importer](https://github.com/Codingpedia/codingmarks-free-programming-books-importer)
for that. Now 

The application is easy - it loads the public bookmarks, so you can search through some of the best programming related
resources out there, or you can search through your own bookmarks.

After importing the 

Mongo DB indexes the main culprit, so I thought.
First thing inspect the indexes
ADD there mongo examples 
https://docs.mongodb.com/manual/reference/explain-results/
```
> db.bookmarks.find({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"}).explain("executionStats");
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "codingpedia-bookmarks.bookmarks",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"userId" : {
				"$eq" : "2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"
			}
		},
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"userId" : 1
				},
				"indexName" : "userId_1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "forward",
				"indexBounds" : {
					"userId" : [
						"[\"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\", \"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\"]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 2714,
		"executionTimeMillis" : 4,
		"totalKeysExamined" : 2714,
		"totalDocsExamined" : 2714,
		"executionStages" : {
			"stage" : "FETCH",
			"nReturned" : 2714,
			"executionTimeMillisEstimate" : 10,
			"works" : 2715,
			"advanced" : 2714,
			"needTime" : 0,
			"needYield" : 0,
			"saveState" : 21,
			"restoreState" : 21,
			"isEOF" : 1,
			"invalidates" : 0,
			"docsExamined" : 2714,
			"alreadyHasObj" : 0,
			"inputStage" : {
				"stage" : "IXSCAN",
				"nReturned" : 2714,
				"executionTimeMillisEstimate" : 0,
				"works" : 2715,
				"advanced" : 2714,
				"needTime" : 0,
				"needYield" : 0,
				"saveState" : 21,
				"restoreState" : 21,
				"isEOF" : 1,
				"invalidates" : 0,
				"keyPattern" : {
					"userId" : 1
				},
				"indexName" : "userId_1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "forward",
				"indexBounds" : {
					"userId" : [
						"[\"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\", \"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\"]"
					]
				},
				"keysExamined" : 2714,
				"dupsTested" : 0,
				"dupsDropped" : 0,
				"seenInvalidated" : 0
			}
		}
	},
	"serverInfo" : {
		"host" : "adrians-mbp.home",
		"port" : 27017,
		"version" : "3.2.9",
		"gitVersion" : "22ec9e93b40c85fc7cae7d56e7d6a02fd811088c"
	},
	"ok" : 1
}
```

PUblic bookmarks:
```
> db.bookmarks.find().explain("executionStats");
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "codingpedia-bookmarks.bookmarks",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"$and" : [ ]
		},
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"$and" : [ ]
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 2734,
		"executionTimeMillis" : 1,
		"totalKeysExamined" : 0,
		"totalDocsExamined" : 2734,
		"executionStages" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"$and" : [ ]
			},
			"nReturned" : 2734,
			"executionTimeMillisEstimate" : 0,
			"works" : 2736,
			"advanced" : 2734,
			"needTime" : 1,
			"needYield" : 0,
			"saveState" : 21,
			"restoreState" : 21,
			"isEOF" : 1,
			"invalidates" : 0,
			"direction" : "forward",
			"docsExamined" : 2734
		}
	},
	"serverInfo" : {
		"host" : "adrians-mbp.home",
		"port" : 27017,
		"version" : "3.2.9",
		"gitVersion" : "22ec9e93b40c85fc7cae7d56e7d6a02fd811088c"
	},
	"ok" : 1
}
```


Then within node, mongoose itself:
Code before:
```
    Bookmark.find({'shared':true}, function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmarks);
    });
```
After:
```
    //Bookmark.find({'shared':true}, function(err, bookmarks){
    Bookmark.find({'shared':true}).lean().exec(function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      console.log('here');
      res.send(bookmarks);
```

Before:
```
bookmarks-api.codingpedia.org:server Listening on port 3000 +0ms
OPTIONS /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 200 5.045 ms - 13
here
GET /api/bookmarks/ 304 511.807 ms - -
GET /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 304 24.830 ms - -
```

After (with mongoose.lean) dramatic increase:
```
[nodemon] starting `node ./bin/www start`
  bookmarks-api.codingpedia.org:server Listening on port 3000 +0ms
here
GET /api/bookmarks/ 200 87.038 ms - 1040147
OPTIONS /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 200 3.082 ms - 13
GET /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 304 37.497 ms - -
```
