---
layout: post
title: How to get current time in java enterprise and be easily testable
description: "How to get current time in java enterprise and be easily testable code snippet"
author: ama
permalink: /snippets/620e5468390f044e75ba6eb5/how-to-get-current-time-in-java-enterprise-and-be-easily-testable
published: true
snippetId: 620e5468390f044e75ba6eb5
categories: [snippets]
tags: [java, javaee, timestamp, date, datetime, cdi, mocking, codever-snippets]
---

With Java version 8 it's very easy to get the current timestamp with `LocalDateTime.now()`.
One problem that might arise is it's not easily **testable/mockable**.
Imagine some testing data and conditions depend on the date you set through this call, or you want to do "time travel".
The solution is quite simple - **define a service class that produces the current timestamp**, which you can then easily mock,
in this case we'll name it `ProductionCalendar`:

```java
@ApplicationScoped
public class ProductionCalendar {
  public LocalDateTime currentTimestamp() {
    return LocalDateTime.now();
  }

  public LocalDate currentDate() {
    return LocalDate.now();
  }

  public String currentTimestamp(Partner partner) {
    return DateTimeFormatter.ofPattern("yyyy-MM-dd-HH.mm.ss.SSSSSS")
        .format(LocalDateTime.now());
  }
}
```

Then when were you need to call `LocalDateTime.now()` inject this service and use its methods instead:


```java
@Stateless
public class PartnerRepository {

  @Inject ProductionCalendar productionCalendar;

  @Inject EntityManager entityManager;

  public void savePartner(String partnerNumber) {
    Partner partner = new Partner();
    partner.setPartnerNumber(partnerNumber);
    partner.setCreatedAt(productionCalendar.currentTimestamp());

    entityManager.persist(partner);
  }

  public List<Partner> getPartnerCreatedAfterDate(LocalDateTime timestamp) {
    String queryJpql = "select p from P p where createdAt > :timestamp order by createdAt desc";
    TypedQuery<Partner> query = entityManager.createQuery(queryJpql, Partner.class);
    query.setParameter("timestamp", timestamp);

    return query.getResultList();
  }
}
```

Then, in the test class you can easily mock `ProductionCalendar` and calls to its methods and set timestamps as we need them:

```java
@ExtendWith(MockitoExtension.class)
@RunWith(JUnitPlatform.class)
public class UserServiceUnitTest {

  @Inject PartnerRepository partnerRepository;

  @Mock ProductionCalendar productionCalendar;

  @Test
  void testGetLatestPartners() {
     when(productionCalendar.currentTimestamp()).thenReturn(LocalDatetime.now().minusDays(1));
     partnerRepository.save("12345");
     var recentPartners = partnerRepository.getPartnerCreatedAfterDate(LocalDatetime.now().minusDays(2));
     assertThat(recentPartners, hasSize(1));
  }

}
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620e5468390f044e75ba6eb5" %}
