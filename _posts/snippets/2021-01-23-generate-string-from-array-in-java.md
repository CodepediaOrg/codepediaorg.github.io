---
layout: post
title: Generate string from array in Java
description: "Code snippet showing how to generate string from array in Java  code snippet"
author: ama
permalink: /snippets/5fff33cfe6eb82224c98e0f6/generate-string-from-array-in-java
published: true
snippetId: 5fff33cfe6eb82224c98e0f6
categories: [snippets]
tags: [java, java-stream, lambda]
---

Convert the array into String using `Collectors.joining()` method of the `stream` package:

```java
import java.util.stream.Collectors;
import java.util.stream.Stream;

class GenerateStringFromArray {
  public static void main(String[] args) {

    String[] values = {"Bookmarks.dev -", "Bookmarks", "and", "code snippets", "manager"};

    String text = Stream.of(values)
        .map(Object::toString) // in this case you can remove the map, but useful for complexer objects
        .collect(Collectors.joining(" "));

    System.out.println(text);
  }
}
```

Result

```java
"Bookmarks.dev - Bookmarks and code snippets manager"
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html" target="_blank" style="font-weight: lighter">
     https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fff33cfe6eb82224c98e0f6" %}
