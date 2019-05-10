---
layout: post
title: How to insert a document in mongodb in java - example
description: "Simple example showing how to insert a document in mongodb with the help of Java language"
author: ama
permalink: /ama/how-to-insert-a-document-in-mongodb-in-java-example
published: true
categories: [java, mongodb]
tags: [codingmarks, java, mongodb, example, maven]
---

This blog post presents a simple example showing how to insert a document in mongodb, in the Java language. 

 <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" />
     The code samples are taken from the Free-Programming-Books-Importer project, available on <a href="https://github.com/Codingpedia/codingmarks-free-programming-books-importer" target="_blank">Github</a>
 </p>  
 
## Use MongoDB Java Driver
Add the java mongo driver to your class path, or if you use maven to the `dependencies` in your _pom_ file:

```
<dependencies>
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb-driver</artifactId>
        <version>3.2.2</version>
    </dependency>
    ......
</dependencies>
```

## Prepare the Mongo Client
```java
MongoClientURI connectionString = new MongoClientURI("mongodb://codingpedia:codingpedia@localhost:27017/codingpedia-bookmarks");
MongoClient mongoClient = new MongoClient(connectionString);
```
<!--more--> 

## Select Mongo Database and Collection
Get the Mongo database:
```java
MongoDatabase database = mongoClient.getDatabase("codingpedia-bookmarks");
```
> A database is a physical container for collections. Each database gets its own set of files on the file system.
 A single MongoDB server typically has multiple databases.

Now we need to select the collection, where we add our newly created document:

```java
MongoCollection<Document> bookmarksCollection = database.getCollection("bookmarks");
```

> A collection is a grouping of MongoDB documents. A collection is the equivalent of an RDBMS table.
 A collection exists within a single database. Collections do not enforce a schema. Documents within a collection can have different fields.
  Typically, all documents in a collection have a similar or related purpose. 

## Prepare the document to be inserted

```java
...
Document document = new Document();
String title = line.substring(line.indexOf("[")+1,line.indexOf("]"));
document.append("name", title);

Set<String> tags = new HashSet<>();
tags.add("free-programming-books");//all free programming books get this tag
tags.add(getFileNameWithoutExtension(fileName));
tags.add(category);//all links have at least one category (fall under an ### element)
if(subCategory != null) {
    tags.add(subCategory);
}
System.out.println("tags : " + tags);
document.append("tags", tags);

```

## Insert the document
Check if the collection contains already the codingmark document based on location. Else add the `document` to the collection
with the help of `insertOne` method:
```java
Document bookmark = bookmarksCollection.find(eq("location", location)).first();
if(bookmark!=null){
    System.out.println("*********************** Bookmark already present *********************** ");
} else {
    bookmarksCollection.insertOne(document);
    numberOfInsertedBookmarks++;
}

```

> A document is a record in a MongoDB collection and the basic unit of data in MongoDB. 
Documents are analogous to JSON objects but exist in the database in a more type-rich format known as BSON.
 See [Documents](https://docs.mongodb.com/manual/reference/glossary/) for more information.

 <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" />
     Source code for this example is available on <a href="https://github.com/Codingpedia/codingmarks-free-programming-books-importer" target="_blank">Github</a>
 </p>  
