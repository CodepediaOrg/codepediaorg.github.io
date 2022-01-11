---
layout: post
title: Read file from test resources in unit test
description: "Read file from test resources in unit test code snippet"
author: ama
permalink: /snippets/605d86a3cfab4d6247615acf/read-file-from-test-resources-in-unit-test
published: true
snippetId: 605d86a3cfab4d6247615acf
categories: [snippets]
tags: [java, testing, unit-testing, jaxb, codever-snippets]
---

Use the `getClass().getClassLoader().getResourceAsStream()`. In the following snippet we read the `bookmark-example.xml`
 file, which is placed directly in the  `src/test/resources` folder:

```java
    @Test
    void shouldUnmarshallXmlToJava() throws JAXBException {
        JAXBContext jaxbContext = JAXBContext.newInstance("dev.codepal.bookmark");
        final var jaxbUnmarshaller = jaxbContext.createUnmarshaller();

        InputStream inStream = getClass().getClassLoader().getResourceAsStream(
                "bookmark-example.xml");

        JAXBElement<Bookmark> o = (JAXBElement<Bookmark>)jaxbUnmarshaller.unmarshal(inStream);
        Bookmark bookmark = o.getValue();
        assertThat(bookmark.getTitle(), equalTo("CodepediaOrg"));
    }
```


<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="605d86a3cfab4d6247615acf" %}
