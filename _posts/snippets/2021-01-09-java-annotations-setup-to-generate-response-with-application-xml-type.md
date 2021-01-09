---
layout: post
title: Java annotations configuration to generate a rest response with application/xml type
description: "How to use java annotations on classes and fields to generate response with application/xml type in a rest call for example."
author: ama
permalink: /snippets/5fb64a11bbfafc4ee5087805/java-annotations-setup-to-generate-response-with-application-xml-type
published: true
snippetId: 5fb64a11bbfafc4ee5087805
categories: [snippets]
tags: [java, xml, mime-types, resteasy, api, rest]
---

The `@XmlAccessorType(XmlAccessType.FIELD)`  is required if you set `@XmlElement`  annotations on the fields instead of the getters methods

```java
package dev.bookmarks.example;

import java.util.List;
import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

@XmlAccessorType(XmlAccessType.FIELD)
@XmlRootElement(name = "bookmarks")
public class BookmarksResponse {

  @XmlElement(name = "bookmark", type = BookmarkDto.class)
  private List<BookmarkDto> bookmarks;

  public BookmarksResponse () {
  }

  public BookmarksResponse (List bookmarks) {
    this.bookmarks= bookmarks;
  }

  public List<BookmarkDto> getAllBookmarks() {
    return bookmarks;
  }

  public void setBookmarks(
      List<Bookmark> bookmarks) {
    this.bookmarks= bookmarks;
  }
}
```

<hr/>
The `BookmarkDto` class (getters and setters not shown for brevity):

```java
package dev.bookmarks.example;

import com.fasterxml.jackson.annotation.JsonProperty;
import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlAttribute;
import javax.xml.bind.annotation.XmlRootElement;
import lombok.EqualsAndHashCode;
import lombok.ToString;

@EqualsAndHashCode
@ToString
@XmlRootElement(name = "bookmark")
@XmlAccessorType(XmlAccessType.FIELD)
public class BookmarkDto {

  @JsonProperty("id")
  @XmlAttribute(name = "id")
  Integer id;

  @JsonProperty("name")
  @XmlAttribute(name = "name")
  String name;

  @JsonProperty("url")
  @XmlAttribute(name = "url")
  String url;

  public BookmarkDto() {
  }

 //getters and setters hidden for brevity
}

```

<hr/>
Would result in an xml response if you request with the `Accept` header as `Accept: application/xml`  as the following

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<bookmarks>
    <boomark id="1">
      <name>How to filter results list in Angular with input text</name>
      <url>https://www.codepedia.org/ama/how-to-filter-results-in-angular-example-with-input-text</url>
   </bookmark>
    <boomark id="2">
      <name>Complete example of a CRUD API with Express GraphQL</name>
      <url>https://www.codepedia.org/ama/complete-example-crud-api-express-graphql</url>
   </bookmark>
</bookmarks
```

<hr/>
The `Accept: application/json` response should look like the following

```json
{
    "bookmarks": [
        {
            "id": 1,
            "name": "How to filter results list in Angular with input text",
            "url": "https://www.codepedia.org/ama/how-to-filter-results-in-angular-example-with-input-text"
        },
        {
            "id": 2,
            "name": "Complete example of a CRUD API with Express GraphQL",
            "url": "https://www.codepedia.org/ama/complete-example-crud-api-express-graphql"
        }
   ]
}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="5fb64a11bbfafc4ee5087805" %}
