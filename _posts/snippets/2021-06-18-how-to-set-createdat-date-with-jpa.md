---
layout: post
title: How to set createdAt  date with jpa
description: "How to set createdAt  date with jpa code snippet"
author: ama
permalink: /snippets/60cc29d95eeee76d91640345/how-to-set-createdat-date-with-jpa
published: true
snippetId: 60cc29d95eeee76d91640345
categories: [snippets]
tags: [java, jpa, timestamp, date, codever-snippets]
---

I usually set the date in the `@PrePersist` hook:

```java
import javax.persistence.Entity;
import javax.persistence.PrePersist;
// others ignored for brevity


@Entity
public class ComparisonReport implements Serializable {

  public static final String TABLE_NAME = "T_COMPARISON_REPORT";
  public static final String COLUMN_CREATED_AT = "CREATED_AT";

  @Column(name = COLUMN_CREATED_AT)
  private LocalDateTime createdAt;

@PrePersist
  protected void prePersist() {
    if (this.createdAt == null) createdAt = LocalDateTime.now();
  }
 //others ignored for brevity
}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60cc29d95eeee76d91640345" %}
