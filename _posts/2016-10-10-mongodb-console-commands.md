---
layout: post
title: First REST operation with Express
description: "In this one I will try to implement a simple GET method that will deliver mocked bookmarks."
author: ama
permalink: /ama/first-rest-operation-with-express
published: false
categories: [nodejs]
tags: [codingpedia bookmarks, beginner, nodejs, expressjs, tutorial]
---

MongoDB INstallation
Go To Download Website

Find all documents from collection

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

Update Many

``` bash
> db.bookmarks.updateMany({}, { $rename: {"url":"location"} })
2016-10-11T06:31:45.390+0200 E QUERY    [thread1] uncaught exception: WriteError({
        "index" : 0,
        "code" : 11000,
        "errmsg" : "E11000 duplicate key error collection: codingpedia-bookmarks.bookmarks index: url_1 dup key: { : null }",
        "op" : {
                "q" : {

                },
                "u" : {
                        "$rename" : {
                                "url" : "location"
                        }
                },
                "multi" : true,
                "upsert" : false
        }
}) :
undefined
```

After dropping index
```
> db.bookmarks.updateMany({}, { $rename: {"url":"location"} })
{ "acknowledged" : true, "matchedCount" : 9, "modifiedCount" : 8 }
```

https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/

List all indexes
https://docs.mongodb.com/v3.2/tutorial/manage-indexes/

```bash
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
                "unique" : true,
                "key" : {
                        "name" : 1
                },
                "name" : "name_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        },
        {
                "v" : 1,
                "unique" : true,
                "key" : {
                        "url" : 1
                },
                "name" : "url_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        }
]
```

Drop Index

```bash
> db.bookmarks.dropIndex({"url" : 1})
{ "nIndexesWas" : 3, "ok" : 1 }
```




## References
