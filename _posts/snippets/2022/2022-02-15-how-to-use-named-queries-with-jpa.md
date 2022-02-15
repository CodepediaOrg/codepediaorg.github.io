---
layout: post
title: How to use named queries with JPA
description: "How to use named queries with JPA code snippet"
author: ama
permalink: /snippets/620b6123e102f148ceb9f5e6/how-to-use-named-queries-with-jpa
published: true
snippetId: 620b6123e102f148ceb9f5e6
categories: [snippets]
tags: [java, jpa, jpql, javaee, codever-snippets]
---

You can define several named queries on an entity by using the `@NamedQueries` and `@NamedQuery` annotations.
For each named query you need to define a `name` and the jpql `query` itself:

```java
import javax.persistence.*;

@Entity
@Access(AccessType.FIELD)
@Table(name = PartnerInitialLoad.TABLE_NAME)
@NamedQueries({
  @NamedQuery(
      name = FIND_MAX_PARTNERNUMMER,
      query = "select max(p.partnernummer) from PartnerInitialLoad p"),
  @NamedQuery(
      name = FIND_PARTNER_BY_STATUS,
      query =
          "select p from PartnerInitialLoad p where p.status = :status order by p.partnernummer asc")
})
public class PartnerInitialLoad {
  public static final String TABLE_NAME = "T_PARTNER_INITIAL_LOAD";

  public static final String FIND_MAX_PARTNERNUMMER = "findMaxPartnernummer";
  public static final String FIND_PARTNER_BY_STATUS = "findPartnerByStatus";

  public static final String PARAM_STATUS = "status"; //the value here has to match the one in jpql, here "status"

 // further entity details emitted for brevity
}
```

Then use the named queries in your repository services. For that use the `createNamedQuery` method of the `EntityManager`,
which expects the **name** of the query you defined in the metadata, plus the **type of the query result**:

```java
@Stateless
public class PartnerInitialLoadRepository {

  @Inject private EntityManager em;

  public List<PartnerInitialLoad> getPartnersByStatus(Integer chunkSize, String status) {
    var query =
        em.createNamedQuery(PartnerInitialLoad.FIND_UNPROCESSED_PARTNER, PartnerInitialLoad.class);
    query.setParameter(PartnerInitialLoad.PARAM_STATUS, status);
    query.setMaxResults(chunkSize);

    return query.getResultList();
  }

  public int getMaxPartnernummer() {
    var query = em.createNamedQuery(PartnerInitialLoad.FIND_MAX_PARTNERNUMMER, Integer.class);
    var singleResult = query.getSingleResult();

    return singleResult == null ? 0 : singleResult;
  }
}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620b6123e102f148ceb9f5e6" %}
