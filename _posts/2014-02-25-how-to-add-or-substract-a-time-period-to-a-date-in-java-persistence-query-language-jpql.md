---
id: 1202
title: 'How to add or substract a time period to a date in Java Persistence Query Language &#8211; JPQL'
date: 2014-02-25T20:49:37+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1202
permalink: /ama/how-to-add-or-substract-a-time-period-to-a-date-in-java-persistence-query-language-jpql/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2320635964
categories:
  - java
tags:
  - date
  - datetime
  - example
  - jpa
  - jpql
---
<p style="text-align: justify;">
  There are quite often situations, when you&#8217;d need to add or substract a time period to a date when you are accessing the database via <a title="JPA" href="http://en.wikipedia.org/wiki/Java_Persistence_API" target="_blank">Java Persistence API(JPA)</a>. Now
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

<pre class="lang:java decode:true" title="Substract time period with JPA/JPQL">public List&lt;Podcast&gt; getRecentPodcasts(int numberOfDaysToLookBack) {

	Calendar calendar = new GregorianCalendar();
	calendar.setTimeZone(TimeZone.getTimeZone("UTC+1"));//Munich time
	calendar.setTime(new Date());
	calendar.add(Calendar.DATE, -numberOfDaysToLookBack);//substract the number of days to look back
	Date dateToLookBackAfter = calendar.getTime();

	String qlString = "SELECT p FROM Podcast p where p.insertionDate &gt; :dateToLookBackAfter";
	TypedQuery&lt;Podcast&gt; query = entityManager.createQuery(qlString, Podcast.class);		
	query.setParameter("dateToLookBackAfter", dateToLookBackAfter, TemporalType.DATE);

	return query.getResultList();
}</pre>

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

  * <a title="Java Persistence Example with Spring, JPA2 and Hibernate" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate </a>&#8211; CRUD example with JPA, Spring and Hibernate incorporating the method from above
  * <a title="RESTful Web Services Example in Java with Jersey, Spring and MyBatis" href="http://www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" target="_blank">RESTful Web Services Example in Java with Jersey, Spring and MyBatis </a>&#8211; defines a REST API that among others, it requests recent podcasts employing the above method

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/amacoder.png" alt="Podcastpedia image" />

  <p id="about_author_header">
    <strong>Adrian Matei</strong>
  </p>

  <div id="author_details" style="text-align: justify;">
    Creator of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> and <a title="Codingpedia, sharing coding knowledge" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>, computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy - but these are just outside labels and not that important, deep inside we are all just consciousness, right?
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
       <a class="icon-twitter" href="https://twitter.com/codingpedia" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/codingpedia" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codingpediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>F

    <div class="clear">
    </div>
  </div>
</div>
