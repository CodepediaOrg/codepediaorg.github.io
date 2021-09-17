---
layout: post
title: Select with in clause from list with JPA
description: "Select in clause from list with JPA code snippet"
author: TODO-change-me
permalink: /snippets/61446015fb0221040e46d431/select-with-in-clause-from-list-with-jpa
published: true
snippetId: 61446015fb0221040e46d431
categories: [snippets]
tags: [java, sql, jpa, jpql, hibernate, codever-snippets]
---

The native SQL query we want to map in JPA is similar to the following:

```
SELECT * FROM PARTNER where PARTNER_NUMBER IN ('id1', 'idn').
```

With JPA you can use a `TypedQuery` for example and set the expected list of the `IN` clause directly as query parameter

```java
@Stateless
public class PartnerDataRepository {

	@Inject private EntityManager em;

	public List<PartnerData> findPartnerDataFromList(
		List<String> partnerNumbers) {
	  TypedQuery<PartnerData> query =
		  em.createNamedQuery(
			  PartnerData.FIND_PARTNER_DATA_IN_LIST, PartnerData.class);
	  query.setParameter(PartnerData.PARTNER_NUMBERS, partnerNumbers);

	  return query.getResultList();
	}
}
```

In the named query itself you can pass the parameter with `:` as you would when setting a "normal" parameter:

```java
@Entity
@Access(AccessType.FIELD)
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@Table(name = PartnerData.TABLE_NAME)
@NamedQueries({
  @NamedQuery(
      name = PartnerData.FIND_PARTNER_DATA_IN_LIST,
      query = "select m from PartnerData m where partnerNumber in :partnerNumbers")
})
public class PartnerData {
  public static final String TABLE_NAME = "PARTNER";

  public static final String PARTNER_NUMBERS = "partnerNumbers";
  public static final String FIND_PARTNER_DATA_IN_LIST =
      "findPartnerDataWithPartnerNumbers";

  //... rest ignored for brevity
}

```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="61446015fb0221040e46d431" %}
