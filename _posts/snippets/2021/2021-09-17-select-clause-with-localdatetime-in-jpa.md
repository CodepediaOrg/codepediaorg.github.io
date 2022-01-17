---
layout: post
title: Select clause with LocalDateTime comparison in JPA
description: "Select clause with LocalDateTime comparison in JPA code snippet"
author: ama
permalink: /snippets/61446015fb0221040e46d431/select-with-in-clause-from-list-with-jpa
published: true
snippetId: 614479defb0221040e46d62a
categories: [snippets]
tags: [java, sql, jpa, jpql, hibernate, datetime, codever-snippets]
---

The native SQL query we want to map in JPA is similar to the following:

```
select *
from PARTNER
where EVENT_TIMESTAMP >= timestamp '2021-09-17 10:00:00'
  and EVENT_TIMESTAMP < timestamp '2021-09-17 11:00:00'
```

where the two timestamps should come as query parameters.

With JPA you can use a `TypedQuery` for example and set the `LocalDateTime` values to query parameters via the `setParameter` method:

```java
@Stateless
public class PartnerDataRepository {

    @Inject private EntityManager em;

    public List<PartnerData> findPartnerDataWithinInterval(
      LocalDateTime fromDatetime, LocalDateTime toDatetime) {
        TypedQuery<PartnerData> query =
            em.createNamedQuery(
                PartnerData.FIND_PARTNER_DATA_IN_TIME_INTERVAL, PartnerData.class);

        query.setParameter(PartnerData.FROM_DATETIME, fromDatetime);
        query.setParameter(PartnerData.TO_DATETIME, toDatetime);

        return query.getResultList();
    }
}
```

In the named query itself you can directly pass the parameters before `:` as usual, and the implementation (in my case Hibernate)
 takes care of the rest :

```java
@Entity
@Access(AccessType.FIELD)
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@Table(name = PartnerData.TABLE_NAME)
@NamedQueries({
 @NamedQuery(
      name = PartnerKerndaten.FIND_PARTNER_DATA_IN_TIME_INTERVAL,
      query = "select m from PartnerData m where eventTimestamp >= :fromDatetime and eventTimestamp < :toDatetime")
})
public class PartnerData {
  public static final String TABLE_NAME = "PARTNER";

  public static final String FROM_DATETIME = "fromDatetime";
  public static final String TO_DATETIME = "toDatetime";

  public static final String FIND_PARTNER_DATA_IN_TIME_INTERVAL =
      "findPartnerDataWithinInterval";

  //... rest ignored for brevity
}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="614479defb0221040e46d62a" %}
