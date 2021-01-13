---
title: 'How to add or substract a time period to a date in Java Persistence Query Language &#8211; JPQL'
date: 2014-02-25T20:49:37+00:00
author: ama
layout: post
permalink: /ama/how-to-add-or-substract-a-time-period-to-a-date-in-java-persistence-query-language-jpql/
categories:
  - tutorial
tags:
  - java
  - date
  - datetime
  - jpa
  - jpql
---
<p style="text-align: justify;">
  There are quite often situations, when you&#8217;d need to add or substract a time period to a date
   when you are accessing the database via <a title="JPA" href="http://en.wikipedia.org/wiki/Java_Persistence_API" target="_blank">Java Persistence API(JPA)</a>. Now
</p>

<li style="text-align: justify;">
  <strong>the bad news</strong> is that <a title="JPQL" href="http://en.wikipedia.org/wiki/Java_Persistence_Query_Language" target="_blank">Java Persistence Query Language(JPQL)</a> does not support such operations on dates yet.
</li>
<li style="text-align: justify;">
  <strong>the good news</strong> is that it is possible by using a native query or doing the computation on the Java side. I prefer the second option as it provides database independence.<!--more-->
</li>

## How To

<p style="text-align: justify;">
  Let&#8217;s see how it can be done. Consider the following scenario: I need to get recent items(podcasts) from the database, let&#8217;s say that were inserted in the last 8 days. For that I&#8217;ll build a function that looks back into the past by the <code>numberOfDaysToLookBack</code> parameter:
</p>

```java
public List&lt;Podcast&gt; getRecentPodcasts(int numberOfDaysToLookBack) {

	Calendar calendar = new GregorianCalendar();
	calendar.setTimeZone(TimeZone.getTimeZone("UTC+1"));//Munich time
	calendar.setTime(new Date());
	calendar.add(Calendar.DATE, -numberOfDaysToLookBack);//substract the number of days to look back
	Date dateToLookBackAfter = calendar.getTime();

	String qlString = "SELECT p FROM Podcast p where p.insertionDate &gt; :dateToLookBackAfter";
	TypedQuery&lt;Podcast&gt; query = entityManager.createQuery(qlString, Podcast.class);
	query.setParameter("dateToLookBackAfter", dateToLookBackAfter, TemporalType.DATE);

	return query.getResultList();
}
```

and call it like `getRecentPodcast(8);`

<p class="note_normal">
  <strong>Note </strong>that the date calculation takes place in Java. One way to do it is by using the Java <code>Calendar</code>&#8216;s  <code>add</code> function with a negative value.  After that you can use the calculated date as parameter in the JPQL comparison.
</p>

If you don&#8217;t like the Java calendar approach, you can achieve the same results with <a title="Joda time" href="http://joda-time.sourceforge.net/quickstart.html" target="_blank">Joda-Time</a>:

<pre class="lang:java decode:true" title="Calculate days back with Joda-Time">DateTime dateToLookBackAfterJoda = new DateTime(new Date());
dateToLookBackAfterJoda = dateToLookBackAfterJoda.minusDays(numberOfDaysToLookBack);
dateToLookBackAfterJoda.toDate();</pre>

<p class="note_normal">
  <strong>Note:</strong> If you want to have an addition instead of substraction use <code>Calendar.add()</code> with a positive value or Joda-Time <code>addDays(int numberOfDays)</code> function.
</p>

Check out also my related posts on the topic

  * <a title="Java Persistence Example with Spring, JPA2 and Hibernate" href="https://www.codepedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate </a>&#8211; CRUD example with JPA, Spring and Hibernate incorporating the method from above
  * <a title="RESTful Web Services Example in Java with Jersey, Spring and MyBatis" href="https://www.codepedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" target="_blank">RESTful Web Services Example in Java with Jersey, Spring and MyBatis </a>&#8211; defines a REST API that among others, it requests recent podcasts employing the above method
