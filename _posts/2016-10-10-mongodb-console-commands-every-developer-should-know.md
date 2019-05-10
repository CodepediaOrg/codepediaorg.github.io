---
layout: post
title: Practical MongoDB shell commands every developer should know
description: "Practical MongoDB shell commands every developer should know, or I at least should know..."
author: ama
permalink: /ama/practical-mongodb-shell-commands-every-developer-should-know
published: true
categories: [mongodb]
tags: [codingmarks, mongodb]
---

The [codingmarks](https://www.codingmarks.org/) are persisted in a [MongoDB Server](https://docs.mongodb.com/manual/).
 Very often I find myself in the situation, where I need to modify or look for something in the mongo database.
 Experience has taught me that interacting with a system via shell commands helps me understand it better,
 and sort of brings me closer to it. Ok, so how to find the right mongo shell command? Well, I google for it of course, and most likely I am pointed to the right entry in the [Mongo manual](https://docs.mongodb.com/manual/). In this post I try to consolidate the commands I usually use,
  so that I have only one [codingmark](https://www.codingmarks.org/search?q=codingpedia mongo shell commands) to look for...

  {% include source-code-codingmarks.html %}

<!--more-->

* TOC
{:toc}

## Start the mongo shell

To start the mongo shell and connect to the MongoDB instance running on **localhost** with **default port**(which is 27017), change to the MongoDB installation directory:

``` bash
> cd <mongodb installation dir>
```

and then type `./bin/mongo` to start mongo

> If you are like me, and [hooked on aliases](http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/), you might use something like `alias mongo-start-client='~/dev/mongodb/bin/mongo'`.
 You can also add `<mongodb installation dir>/bin` to the PATH environment variable and then you can just type `mongo`

MongoDB does not enable access control by default. This might be fine for development, but for a production environment it is highly recommended
to employ [authorization](https://docs.mongodb.com/manual/core/authorization/). Please see the **Create database and users** section of
the [MongoDB Setup For Production](https://github.com/Codingpedia/codingmarks-api/wiki/MongoDB-Setup-for-Production) wiki page for commands related to that.

## Quit the mongo shell

You can type the following command to exit the mongo shell:

``` bash
> quit()
```

## Display database and collections

You can print a list of all available database on the server via `show dbs` command:

``` bash
> show dbs
admin                  0.000GB
codingpedia-bookmarks  0.000GB
local                  0.000GB
```

To switch to the _codingpedia-bookmarks_ database, use the `use` command:

``` bash
> use codingpedia-bookmarks
switched to db codingpedia-bookmarks
```

Print the list of all collections of the current database:

``` bash
> show collections
bookmarks
```

## Find documents

### Find all documents
To find all documents from collection, type the following:

``` bash
> db.bookmarks.find()
```

Response

```bash
{ "_id" : ObjectId("57f3f0ba5fac7f3264354144"), "name" : "Test bookmark", "url" : "some titi url", "description" : "some description", "category" : "test", "tags" : [ "testing", "setup" ], "__v" : 0, "updatedAt" : ISODate("2016-10-10T08:18:32.563Z") }
{ "_id" : ObjectId("57f3f1235fac7f3264354146"), "name" : "Second bookmark", "url" : "some other url", "description" : "some description", "category" : "test", "tags" : [ "testing", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f484ac1f991e33ab5e50bb"), "name" : "java bookmark", "url" : "java url", "description" : "some description", "category" : "java", "tags" : [ "testing", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5cd5975bf4e35310426f7"), "name" : "git bookmark", "url" : "git url", "description" : "some git description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5cd7c453b2a353ba33f73"), "name" : "js bookmark", "url" : "js url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5db18eab35f36151691f4"), "name" : "wildfly bookmark", "url" : "wildfly url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5dbd43910e93642b25d6d"), "name" : "jboss bookmark", "url" : "jboss url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5dcf6517aa1366b0526fa"), "name" : "linux bookmark", "url" : "linux url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5dd573fed7e3677b20ca5"), "name" : "windows bookmark", "url" : "windows url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }

```

> use the `pretty` cursor to display the results in an easy to read format like `db.bookmarks.find().pretty()`
 
### Find document by id

Explicit
```
> db.test.find({"_id" : ObjectId("5b10cce6b011af410e515e21")}) 
```
or shortcut

```
> db.bookmarks.find(ObjectId("5b10cce6b011af410e515e21")).pretty();
```


### Find documents with filter

Filter by one attribute (here filter by **location**):
```
> db.bookmarks.find({location: "http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/"});
{ "_id" : ObjectId("5948ab65ce8e01b7e330b330"), "name" : "Git magic", "location" : "http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/", "tags" : [ "free-programming-books-zh", "版本控制" ], "description" : "", "descriptionHtml" : "<p></p>", "createdAt" : ISODate("2017-06-20T04:58:13.192Z"), "shared" : true, "userId" : "2d6f6cb5-44f3-441d-a0c5-ec9afea98d39", "language" : "zh" }
```

Check out the [find documention](https://docs.mongodb.com/manual/reference/method/db.collection.find/) for more details.

### [Query an array](https://docs.mongodb.com/manual/tutorial/query-arrays/)

#### Match an array

Query for all documents where the field `tags` value is an array with exactly three elements, "angular", "angular-cli" and "dev-tools", in the specified order:   
```
> db.bookmarks.find( { tags: ["angular", "angular-cli", "dev-tools"] } )
```

If, instead, we wish to find an array that contains all the elements "angular", "angular-cli" and "dev-tools", without regard to order or other elements in the array,
 we use the `$all` operator:
```
> db.bookmarks.find( { tags: { $all: ["angular", "angular-cli", "dev-tools"] } } )
```

#### Query an Array for an Element

To query if the array field contains at least one element with the specified value, use the filter `{ <field>: <value> }` where `<value>` is the element value.

The following example queries for all documents where `tags` is an array that contains the string "angular" as one of its elements:
```
> db.bookmarks.find({tags:"angular-cli"})
```

### Sort results

You can apply `sort()` to the cursor before retrieving any documents from the database.

For example let's sort documents by `updatedAt` Date (`1` for ascending and `-1` for descending):
docs

``` bash
> db.bookmarks.find().sort({updatedAt:1});
> db.bookmarks.find().sort({updatedAt:-1});
```

See [doku](https://docs.mongodb.com/manual/reference/method/cursor.sort/) for more details.

### Find all documents with missing field

For example I wanted to find all the documents that don't contain the field `starredBy` yet:
```bash
> db.bookmarks.find({ "starredBy" : { "$exists" : false } })
```
or 

```bash
> db.bookmarks.find({starredBy: null});
```

### Find number of elements matching search criteria

For that we need the [db.collection.count()](https://docs.mongodb.com/manual/reference/method/db.collection.count/) operator.

For example to count all the documents in the collection:
```bash
> db.bookmarks.count()
```

To count all the documents missing the ``starredBy`` field as above:
```bash
> db.bookmarks.count({ "starredBy" : { "$exists" : false } })
```

## Delete/Remove documents

Remove all documents from collection:
```bash
> db.bookmarks.remove()
```

Removes all bookmarks for user, identified by userId
```
> db.bookmarks.remove({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"});
WriteResult({ "nRemoved" : 4 })
```

Remove by id, use `justOne` flag
```bash
db.bookmarks.remove({ "_id" : ObjectId("5c4aa6f65b27d00447dd66bd") }, {justOne: true})
WriteResult({ "nRemoved" : 1 })
```

Remove with attributes (all but where `shared` is not set to true):
```bash
> db.bookmarks.remove({shared:{$ne:true}})
WriteResult({ "nRemoved" : 9 })
```

For more details see the [db.collection.remove() documentation](https://docs.mongodb.com/manual/reference/method/db.collection.remove/)

## Update documents

### Update field value
Update `githubURL` for document with the given `location` (we know that is unique):

```
> db.bookmarks.update({ location : "http://www.codingpedia.org/" }, { githubURL : "https://github.com/Codingpedia/codingpedia.github.io"} );
```

### Add new field

Add new `language` field and set it to `en` for all documents:
```
> db.bookmarks.update({}, {$set: {language: "en"}}, {multi: true});
```

Add a new field `starredBy` for all the documents where it is missing:
```
> db.bookmarks.update(
    { "starredBy": { "$exists": false } },
    { "$set": { "starredBy": [] } },
    { "multi": true }
)
```
For more `update` options, like `upsert`, `$unset` etc, please see the [db.collection.update() manual](https://docs.mongodb.com/manual/reference/method/db.collection.update/).

### Rename a field
To rename a field, call the `$rename` operator with the current name of the field and the new name:
``` bash
> db.bookmarks.updateMany({}, { $rename: {"url":"location"} })
```
This operation renames the field `url` to `location` for all documents in the collection.
To see how to rename embedded fields and more see the [documentation for $rename](https://docs.mongodb.com/manual/reference/operator/update/rename/)

### Update Many

Update the userId all documents in the collection of a specific user:
```
try {
   db.bookmarks.updateMany(
      { userId: { $eq: "4204bc10-3dea-4ce7-baf1-44bbbc2eaedf" } },
      { $set: { userId : "33d22b0e-9474-46b3-9da4-b1fb5d273abc" } }
   );
} catch (e) {
   print(e);
}
```

## Indexing
[Indexes](https://docs.mongodb.com/manual/indexes/) support the efficient execution of queries in MongoDB.
 Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection,
  to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

### Show indexes

Before creating an index we should see the existing indexes on a collection,
by typing the following command:

```
> db.bookmarks.getIndexes()

[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "codingpedia-bookmarks.bookmarks"
        }
]
```

> By default, all collections have an index on the `_id `field

### Create index
MongoDB provides complete support for indexes on any field in a collection of documents. In addition to the index on the `_id`field applications and users may add additional indexes to support important queries and operations.

> Why you might want to create indexes - well, because indexes improve the efficiency of read operations by reducing the amount of data that query operations need to process. Please see the [Query Optimization](https://docs.mongodb.com/manual/core/query-optimization/) documentation entry for more details

Some exaples:

Create [**single** index](https://docs.mongodb.com/manual/core/index-single/) for the `userId` field
```
> db.bookmarks.createIndex( { userId: 1 } )
```

> Value `1` means the index is sort ascending on the field

Create [**unique** index](https://docs.mongodb.com/manual/core/index-unique/) for the `location` field:
```
> db.bookmarks.createIndex( { location: 1 }, { unique: true } );
```

Show the newly created indexes after:
```
> db.bookmarks.getIndexes()

[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "codingpedia-bookmarks.bookmarks"
        },
        {
                "v" : 1,
                "key" : {
                        "userId" : 1
                },
                "name" : "userId_1",
                "ns" : "codingpedia-bookmarks.bookmarks"
        },
        {
                "v" : 1,
                "unique" : true,
                "key" : {
                        "location" : 1
                },
                "name" : "location_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        }
]
```

Verify if index is used for a query with the [explain](https://docs.mongodb.com/manual/reference/explain-results/) method:
```
> db.bookmarks.find({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"}).explain("queryPlanner")
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
	"serverInfo" : {
		"host" : "adrians-mbp.home",
		"port" : 27017,
		"version" : "3.2.9",
		"gitVersion" : "22ec9e93b40c85fc7cae7d56e7d6a02fd811088c"
	},
	"ok" : 1
}
```

### Drop index

We use `db.collection.dropIndex(index)` to drop or remove an index from a collection. Let's drop the unique "name" index from index list shown above.
To drop the index 'name', we can either use the index name:
```
> db.bookmarks.dropIndex("name_1");
```
Or we can use the index specification document (key) `{ "name" : 1 } `:
```
> db.pets.dropIndex( { "name" : 1 } );
```


### Create compound index

Drop the <span class="highlight-yellow">unique location index</span> and add compound index on <span class="highlight-yellow">location and user id</span>:

```
// drop index
codingpedia> db.bookmarks.dropIndex("location_1");
{ "nIndexesWas" : 2, "ok" : 1 }

// create compound unique index
codingpedia> db.bookmarks.createIndex( { location: 1, userId:1 }, { unique: true } );
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
```
## Advanced

### Remove trailing spaces from array entries

Trim the spaces for all the **tags** in the bookmark collection:
```
db.bookmarks.find({}).forEach(
  function(doc){
    if(doc.tags){
      print(doc.tags);
      var trimmedTags = [];
      doc.tags.forEach(function(tag){
        trimmedTags.push(tag.trim());
      });

      db.bookmarks.update({"_id": doc._id}, {"$set":{ "tags":trimmedTags } });
    }
  }
);
```

The more I use MongoDB, probably the bigger this blog post will get.

  {% include source-code-codingmarks.html %}
