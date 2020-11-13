---
layout: post
title: GraphQL Resources to help you get started
description: "List of GraphQL resources to get you started with GraphQL, from beginner to advanced - the order is important."
author: ama
permalink: /ama/graphql-resources-to-help-you-get-started
published: true
categories: [graphql]
tags: [resources, bookmarks]
---

I have recently started to look more thoroughly into GraphQL.
 I must say I felt a bit overwhelmed at the beginning with the question - **"Where do I start?"**,
  I mean take a look at the [spec](https://spec.graphql.org/draft)... or don't! Not yet.

I had already [bookmarked dozens of GraphQL resources](https://www.bookmarks.dev/search?q=%5Bgraphql%5D%20user:33d22b0e-9474-46b3-9da4-b1fb5d273abc&sd=public-bookmarks),
 I had read or watched before. I sat down and analyzed them again.  So, I came up with a list of GraphQL resources I found most useful to begin.
  Thus, you can get a good grasp of GraphQL in about two or three days.

<p class="note_normal">
    <strong>Note: The order is also important</strong>
</p>

> After you are done with the theory I suggest you get your hands dirty and checkout  my second blog post on GraphQL,
> [Complete example of a CRUD API with Express GraphQL]({% post_url 2020-11-10-complete-example-crud-api-graqphql-express %})

* TOC
{:toc}

<!--more-->

## 1. HowToGraphQL Videos
I would start with the these Youtube videos from HowToGraphQL to get a grasp what GraphQL is all about from a flight altitude.

### HowToGraphQL (Fundamentals) - Introduction (1/4)
Learn what GraphQL is, how it compares to REST and about the historic context in which it was created.

<iframe width="560" height="315" src="https://www.youtube.com/embed/oCT4HOJsUZQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### HowToGraphQL (Fundamentals) - GraphQL is the better REST (2/4)
Learn about the technical differences between GraphQL and REST and how many common issues with REST APIs can be solved by using GraphQL.

<iframe width="560" height="315" src="https://www.youtube.com/embed/T571423fC68" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### HowToGraphQL (Fundamentals) - Core Concepts (3/4)
Learn about the basic GraphQL language concepts, such as Queries, Mutations, Subscriptions and the GraphQL Schema & SDL.

<iframe width="560" height="315" src="https://www.youtube.com/embed/NeQfq0U5LnI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### HowToGraphQL (Fundamentals) - Big Picture (Architecture) (4/4)
Learn about different architectural use cases of GraphQL and the major components on the backend and the frontend, like resolver functions and client libraries.

<iframe width="560" height="315" src="https://www.youtube.com/embed/b7tMHnxzK34" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> You can also find these videos in the home page from [HowToGraphQL.com](https://www.howtographql.com/)

## 2. README file of the spec project on Github
This [README](https://github.com/graphql/graphql-spec/blob/master/README.md) walks you through the GraphQL.js reference
 implementation's type system, query execution, validation, and introspection systems.
  There's more in both [GraphQL.js](https://github.com/graphql/graphql-js/) and specification,
   including a description and implementation for executing queries, how to format a response,
    explaining how a type system maps to an underlying implementation, and how to format a GraphQL response, as well as the grammar for GraphQL.

## 3. [https://graphql.org/learn](https://graphql.org/learn/)

Read **all**, and I mean all the articles from **Introduction** to **Best practices**.

## 4. Advanced stuff
Once you are through with the above resources, and you feel more comfortable with GraphQL, you can start with more advanced stuff like

### 4.1 Pagination
#### [Understanding pagination: REST, GraphQL, and Relay](https://www.apollographql.com/blog/understanding-pagination-rest-graphql-and-relay-b10f835549e7/)
 where some different approaches to pagination in REST and GraphQL are covered:

* Pagination: what is it for?
* What are different types of pagination, and when are they useful?
* What is it like to implement these different types?
* How does all of this lead to Relay’s pagination spec for GraphQL?
* Is there a single best solution? (Spoiler alert: no, as always)

Plus extra you get to understand terms like **connections**, **edges** from the relay/graphQL world

### 4.2 Security aspects

Like
* Route change
* Introspection
* Authentication
* Depth / Complexity
* Schema generation

presented very nice in
[GraphQL Security OWASP YVR 2020](https://owasp.org/www-chapter-vancouver/assets/presentations/2020-06_GraphQL_Security.pdf)

OR

### 4.3 Error handling

#### [Handling GraphQL errors like a champ with unions and interfaces](https://blog.logrocket.com/handling-graphql-errors-like-a-champ-with-unions-and-interfaces/)
An interesting approach to handling errors in GraphQL with the possibilities that the spec provides and still have type safety.

### 4.4 Caching
#### [GraphQL & Caching: The Elephant in the Room](https://www.apollographql.com/blog/graphql-caching-the-elephant-in-the-room-11a3df0c23ad/)
The author addresses the misconception that "GraphQL breaks caching" and discusses thoroughly the topic.


## Bonus - [GraphQL Public Bookmarks](https://github.com/BookmarksDev/bookmarks/blob/master/tagged/graphql.md)
You can follow the  [**graphql** tag on Bookmarks.dev](https://www.bookmarks.dev/t/graphql)
to get the latest public entries about GraphQL or search to find resources in combination with other terms
(e.g. [graphql and java](https://www.bookmarks.dev/search?q=%5Bgraphql%5D%20java&sd=public-bookmarks&page=1&include=all))

> I think the best way to learn something is with hands-on exercise, so in the next article we will do just that.


