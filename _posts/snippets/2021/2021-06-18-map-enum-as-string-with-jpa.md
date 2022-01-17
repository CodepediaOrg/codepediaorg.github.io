---
layout: post
title: Map enum as string with jpa
description: "Code snippet showing how to map an enum as string with jpa"
author: ama
permalink: /snippets/60cc29135eeee76d91640344/map-enum-as-string-with-jpa
published: true
snippetId: 60cc29135eeee76d91640344
categories: [snippets]
tags: [java, string, jpa, codever-snippets]
---

Use the `@Enumerated(EnumType.STRING)` annotation:

```java
import javax.persistence.Entity;
import javax.persistence.EnumType;
import javax.persistence.Enumerated;

@Entity
public class ComparisonReport implements Serializable {

  public static final String TABLE_NAME = "T_COMPARISON_REPORT";
  public static final String COLUMN_TRIGGER_TYPE = "TRIGGER_TYPE";

  @Column(name = COLUMN_TRIGGER_TYPE)
  @Enumerated(EnumType.STRING)
  private ReportTriggerType triggerType;

 //others ignored for brevity
}
```

The `enum` itself looks something the following

```java
public enum ReportTriggerType {
    MANUAL("MANUAL"),
    AUTOMATIC("AUTOMATIC");

    private String value;

    ReportTriggerType(String source) {
      this.value = source;
    }

    public String getValue() {
      return value;
    }
  }
```



<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60cc29135eeee76d91640344" %}
