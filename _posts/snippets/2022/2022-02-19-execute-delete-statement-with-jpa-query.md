---
layout: post
title: Execute delete statement with jpa query
description: "Execute delete statement with jpa query code snippet"
author: ama
permalink: /snippets/620f73f0390f044e75ba6ff9/execute-delete-statement-with-jpa-query
published: true
snippetId: 620f73f0390f044e75ba6ff9
categories: [snippets]
tags: [java, jpa, jpql, codever-snippets]
---

Use the `executeUpdate` method of the `Query` interface. It will return the number of deleted entries:

```java
public int deleteOldMessages(int daysBack) {
  var query = em.createQuery("delete from Message m where createdAt < :givenTimestamp");
  query.setParameter(Message.GIVEN_TIMESTAMP, LocalDateTime.now().minusDays(daysBack));

  return query.executeUpdate();
}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620f73f0390f044e75ba6ff9" %}
