---
layout: post
title: Find entity in jpa by primary key
description: "Find entity in jpa by primary key code snippet"
author: ama
permalink: /snippets/619524c9fb0221040e48bbb5/find-entity-in-jpa-by-primary-key
published: true
snippetId: 619524c9fb0221040e48bbb5
categories: [snippets]
tags: [java, jpa, javaee, codever-snippets]
---

Use the `find` method of the `EntityManager`, where the primary key is provided as the second parameter:

```java
@Stateless
public class PartnerRepository {

  @Inject private EntityManager em;

  public Optional<Partner> find(String partnerNumber) {
    return Optional.ofNullable(em.find(Partner.class, partnerNumber));
  }
}
```

The entity looks something like the following, with `partnerNumber` as primary key:

```java
@Entity
@Access(AccessType.FIELD)
@Table(name = "T_PARTNER")
public class Partner {

  @Id
  @NotNull
  @Column(name = "PARTNER_NUMBER", nullable = false)
  private Integer partnerNumer;

  //other fields ignored for readability
}
```

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="619524c9fb0221040e48bbb5" %}
