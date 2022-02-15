---
layout: post
title: How to use named parameters in JPA queries
description: "How to use named parameters in JPA queries code snippet"
author: ama
permalink: /snippets/620b73afe102f148ceb9f6ea/how-to-use-named-parameters-in-jpa-queries
published: true
snippetId: 620b73afe102f148ceb9f6ea
categories: [snippets]
tags: [jpa, jpql, java, sql, codever-snippets]
---

Define a named parameter (`:status`) in a [named query](https://www.codever.land/snippets/620b6123e102f148ceb9f5e6/details):

```java
import javax.persistence.*;

@Entity
@Access(AccessType.FIELD)
@Table(name = PartnerInitialLoad.TABLE_NAME)
@NamedQueries({
  @NamedQuery(
      name = FIND_PARTNER_BY_STATUS ,
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

Then, set the named parameter in the created `TypedQuery` (`em.createNamedQuery`) with the `setParameter` method,
which expects the name of the parameter (should match the one defined in the `@NamedQuery`) and its value:

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
}
```

Same principle applies, if we create a **collection-valued named parameters**,
to generate for example a `SELECT IN` clause sql as in the following snippet:

```java
  public List<PartnerInitialLoad> getPartnersWithStatusInList(Integer chunkSize, List<String> statusList) {
   String sql="select p from PartnerInitialLoad p where p.status IN (:statusList) order by p.partnernummer asc"
    var query =
        em.createQuery(sql, PartnerInitialLoad.class);
    query.setParameter("statusList", status);
    query.setMaxResults(chunkSize);

    return query.getResultList();
  }
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620b73afe102f148ceb9f6ea" %}
