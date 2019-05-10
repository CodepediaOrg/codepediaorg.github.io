---
layout: post
title: A Java cache example with Guava
description: "This blog post presents an elegant solution to implement a Java cache with Guava. We imagine a scenario
 where we call a REST service, whose response values don't change very often"
author: ama
permalink: /ama/java-cache-example-with-guava
published: true
categories: [java]
tags: [java, javaee, caching]
---

Last week I showed you [how to increase the performance of multiple java calls by making them asynchronous and parallel](http://www.codingpedia.org/ama/how-to-make-parallel-calls-in-java-with-completablefuture-example).
This week we take a look at another potential performance booster, you should always keep in mind - <span class="highlight-yellow">caching</span>. 

Let's imagine a scenario where you call a REST service and you know the returned values don't change very often. In this case
you really need to consider caching the response. We will use Guava's provided caching capabilities to implement such a cache
for this example. 

<!--more-->

Here is the piece of code that might do just that:
  
```java
import org.codingpedia.example;

import com.google.common.cache.CacheBuilder;
import com.google.common.cache.CacheLoader;
import com.google.common.cache.LoadingCache;

import javax.inject.Inject;
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.core.MediaType;
import java.util.List;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.TimeUnit;
import java.util.stream.Collectors;


public class GuavaCacheDemoService {

    static LoadingCache<String, ToDo> toDosCache = CacheBuilder.newBuilder()
            .maximumSize(1000)
            .expireAfterAccess(60, TimeUnit.SECONDS)
            .build(
                    new CacheLoader<String, ToDo>() {
                        public ToDo load(String id) {
                               final ToDo toDo = restApiClient.getToDo(id);    
                               return toDo;
                        }
                    }
            );
    
    @Inject
    RestApiClient restApiClient;
    
    ToDo getToDo(String toDoId) throws ExecutionException {
        final ToDO toDo = toDosCache.get(toDoId);
        return toDo; 
    }
    
}
```

**Notes:**

* you obtain a `Cache` by using the `CacheBuilder` builder pattern
* our cache should not grow beyound 1000 entries - `CacheBuilder.maximumSize(1000)`
*  `CacheBuilder.expireAfterAccess(60, TimeUnit.SECONDS)` only expire entries after the 60 seconds have passed since the entry was last accessed by a read or a write
* we use for caching a  `LoadingCache`, which is a Cache built with an attached `CacheLoader`
  * to create a `CacheLoader` you just need to implementing the method `V load(K key) throws Exception`
  * inside the method's body you would put the **expensive** method - here the REST call
* to return the value associated with the key in the cache you need to use the `V get(K key) throws ExecutionException` method;
in our case `toDosCache.get(toDoId)`
* your ToDos might change every night - then you want to evict all entries from
the cache by using `Cache.invalidateAll()`
* Guava caches are local to a single run of your application. They do not store data in files, or on outside servers.
 If this does not fit your needs, consider a tool like [Memcached](http://memcached.org/).


I encourage you to read the [Guava Caches Explained Guide](https://github.com/google/guava/wiki/CachesExplained) to learn
 about other caching capabilities like writing to cache, using other eviction methods, explicit removals and so on. 
