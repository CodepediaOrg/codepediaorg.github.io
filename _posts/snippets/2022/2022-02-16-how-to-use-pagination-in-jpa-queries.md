---
layout: post
title: How to use pagination in JPA queries
description: "How to use pagination in JPA queries code snippet"
author: ama
permalink: /snippets/620bde4ee102f148ceb9f6eb/how-to-use-pagination-in-jpa-queries
published: true
snippetId: 620bde4ee102f148ceb9f6eb
categories: [snippets]
tags: [java, jpa, javaee, jpql, sql, codever-snippets]
---

Use the `firstResult(int startPosition)` and `setMaxResults(int maxResult)` of the JPA `Query` interface.
The `firstResult()` method sets the **offset** value in SQL lexicon, meaning the position of the first result to retrieve.
The `setMaxResults()` method sets the **limit** value in SQL lexicon, meaning the maximum number of results to retrieve:

```java
@Stateless
public class PartnerInitialLoadRepository {

  @Inject private EntityManager em;

  public List<PartnerInitialLoad> getNextUnprocessed(Integer pageSize, Integer offset) {
    var jpqlQuery="select p from PartnerInitialLoad p where p.status = 'UNPROCESSED' order by p.partnernumber asc";
    var query =
        em.createNamedQuery(jpqlQuery, PartnerInitialLoad.class);
    query.setFirstResult(offset);
    query.setMaxResults(pageSize);

    return query.getResultList();
  }
}
```

JPA with Hibernate translates this in the following SQL statement in Oracle dialect which uses **offset** and **limit**:

```sql
select partnerini0_.PARTNER_NUMBER as PARTNER_1_13_,
       partnerini0_.COMPLETED_AT as COMPLETE2_13_,
       partnerini0_.STATUS as STATUS3_13_
from T_PARTNER_INITIAL_LOAD partnerini0_
where partnerini0_.STATUS is null
   or partnerini0_.STATUS = 'UNPROCESSED'
order by partnerini0_.PARTNER_NUMBER asc
offset ? limit ?;
```

An alternative would be to use **native queries**, the major drawback is that you make your code dependent on the underlying RDBMS:

```java
  public List<PartnerInitialLoad> getNextUnprocessed(Integer pageSize, Integer offset) {
    var sqlNativeString =
        String.format(
            "select * from T_PARTNER_INITIAL_LOAD where STATUS = 'UNPROCESSED' order by PARTNER_NUMBER asc OFFSET %s LIMIT %s",
            offset, pageSize);
    var query = em.createNativeQuery(sqlNativeString, PartnerInitialLoad.class);

    return query.getResultList();
  }
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620bde4ee102f148ceb9f6eb" %}
