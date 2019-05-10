---
layout: post
title: How to make parallel calls in Java with CompletableFuture example
description: "This blog post presents an example about how to make parallel calls in Java with CompletableFuture in an 
asynchronous fashion. It resembles somehow the parallel calls pattern one can achieve in JavaScript with async-await and Promise.all"
author: ama
permalink: /ama/how-to-make-parallel-calls-in-java-with-completablefuture-example
published: true
categories: [java]
tags: [java, javaee, async]
---

Some time ago I wrote how elegant and rapid is to [make parallel calls in NodeJS with async-await and Promise.all
capabilities](http://www.codingpedia.org/ama/parallel-calls-with-async-await-in-javascript-i-promise-you-all-performance-and-simplicity). 
Well, it turns out in Java is just as elegant and succinct with the help of [CompletableFuture](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
which was introduced in Java 8. To demonstrate that let's imagine that you need to retrieve a list of ToDos from a REST service, given their Ids. Of course
you could iterate through the list of Ids and sequentially call the web service, but it's much more performant to do it in parallel 
with asynchronous calls.

<!--more-->

Here is the piece of code that might do just that:

  
```java
import org.codingpedia.example;

import javax.inject.Inject;
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.function.Supplier;
import java.util.stream.Collectors;

public class ParallelCallsDemoService {

    @Inject
    RestApiClient restApiClient;

    public List<ToDo> getToDos(List<String> ids){

        List<CompletableFuture<ToDo>> futures =
                ids.stream()
                          .map(id -> getToDoAsync(id))
                          .collect(Collectors.toList());

        List<ToDo> result =
                futures.stream()
                        .map(CompletableFuture::join)
                        .collect(Collectors.toList());

        return result;
    }


    CompletableFuture<ToDo> getToDoAsync(String id){

        CompletableFuture<ToDo> future = CompletableFuture.supplyAsync(new Supplier<ToDo>() {
            @Override
            public ToDo get() {
                final ToDo toDo = restApiClient.getToDo(id);

                return toDo;
            }
        });

        return future;
    }

}
```

**Notes:**

* the `supplyAsync` method takes a [`Supplier`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)
 which contains the code you want to execute asynchronously - in this case this is where the REST call takes place...
* you fire all the rest calls and you collect all the `futures` in a list
* in the end you collect all the results with the help of the `join()` method - this returns the value of the CompletableFuture
when complete, or throws an (unchecked) exception if completed exceptionally. 


