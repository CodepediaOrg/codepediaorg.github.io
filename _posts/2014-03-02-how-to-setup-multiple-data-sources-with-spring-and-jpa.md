---
id: 1218
title: How to setup multiple data sources with Spring and JPA
date: 2014-03-02T12:42:31+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1218
permalink: /ama/how-to-setup-multiple-data-sources-with-spring-and-jpa/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 2
fsb_social_google:
  - 8
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2345397342
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
categories:
  - java
  - spring
tags:
  - configuration
  - data source
  - example
  - how to
  - jpa
  - persistence
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Showcase">1. Showcase</a>
    </li>
    <li>
      <a href="#2_Configuration">2. Configuration</a><ul>
        <li>
          <a href="#21_Persistencexml">2.1. Persistence.xml</a>
        </li>
        <li>
          <a href="#22_Spring_Appplication_Context">2.2. Spring Appplication Context</a>
        </li>
        <li>
          <a href="#23_In_code">2.3. In code</a><ul>
            <li>
              <a href="#231_Using_Transactional">2.3.1. Using @Transactional</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Testing_it_8211_optional">3. Testing it &#8211; optional</a>
    </li>
    <li>
      <a href="#4_Resources">4. Resources</a><ul>
        <li>
          <a href="#41_Codingpedia">4.1. Codingpedia</a>
        </li>
        <li>
          <a href="#42_GitHub">4.2. GitHub</a>
        </li>
        <li>
          <a href="#43_Web">4.3. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  In this post I will show you how to setup two or more data sources in a Spring application where the access to the database is done via JPA. It will be a XML-based Spring configuration. To highlight the setup I will use a showcase that builds on an existing demo example I have committed on GitHub, that covers other two posts of mine
</p>

  * <a title="http://www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" href="http://www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" target="_blank">RESTful Web Services Example in Java with Jersey, Spring and MyBatis</a>

    and
  * <a title="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate</a>

Don’t worry! You don’t have to understand what&#8217;s going on in those if you just want to see how the setup for multiple data sources looks like &#8211; I&#8217;ll do a quick introduction in the first part of the post.<!--more-->

<h2 dir="ltr">
  <span id="1_Showcase">1. Showcase</span>
</h2>

<p dir="ltr" style="text-align: justify;">
  <span style="line-height: 1.5;">The demo application used in the posts mentioned presents how to use a REST API to execute CRUD operations against a </span>back-end<span style="line-height: 1.5;"> db, delivering podcasts. For the demo’s sake I&#8217;ll say a client of the REST API also needs resources(podcasts) from a “legacy” system &#8211; she needs to combine them with the ones from the actual system and present them to their users.</span>
</p>

<p style="text-align: justify;">
  <span style="line-height: 1.5;">For that I will implement two new read operations that will GET the podcast(s) from the “legacy” system. The new REST facade layer &#8211; <code>PodcastLegacyRestService</code> &#8211; that delivers “legacy” resources will use the same data access layer to highlight the use of multiple data sources. </span>
</p>

<span style="line-height: 1.5;">Now let’s see how to configure and code multiple data sources with Spring and JPA:</span>

## <span id="2_Configuration">2. Configuration</span>

### <span id="21_Persistencexml">2.1. Persistence.xml</span>

The first thing I did was to modify the `persistence.xml` file by adding a new `persistence unit` that will correspond to the &#8220;legacy&#8221; `entityManager`, managing the new &#8220;legacy&#8221; data source:

<pre class="lang:default mark:6-8 decode:true" title="Persistence.xml">&lt;persistence version="2.0" xmlns="http://java.sun.com/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"&gt;
   &lt;persistence-unit name="demoRestPersistence"&gt;
      &lt;provider&gt;org.hibernate.ejb.HibernatePersistence&lt;/provider&gt;
   &lt;/persistence-unit&gt;

   &lt;persistence-unit name="demoRestPersistenceLegacy"&gt;
      &lt;provider&gt;org.hibernate.ejb.HibernatePersistence&lt;/provider&gt;
   &lt;/persistence-unit&gt;   
&lt;/persistence&gt;</pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The persistence unit defines a set of all entity classes that are managed by <code>EntityManager</code> instances in an application. This set of entity classes represents the data contained within a single data store.
</p>

<p style="text-align: justify;">
  Because I am using Spring you can see the configuration of the persistence unit in the <code>persistence.xml</code> is very <strong>lean</strong>. The actual configuration of the entity managers takes places in the Spring&#8217;s application context of the application. See the next section for the details.
</p>

### <span id="22_Spring_Appplication_Context">2.2. Spring Appplication Context</span>

In the Spring application context I just added new beans for the entity manager, transaction manger and datasource:

<pre class="lang:default mark:22-24,37-49,58-61 decode:true">&lt;beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd

		http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx.xsd

		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd"&gt;

	&lt;context:component-scan base-package="org.codingpedia.demo.rest.*" /&gt;

	&lt;!-- ************ JPA configuration *********** --&gt;
	&lt;tx:annotation-driven transaction-manager="transactionManager" /&gt;  
    &lt;bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager"&gt;
        &lt;property name="entityManagerFactory" ref="entityManagerFactory" /&gt;
    &lt;/bean&gt;
    &lt;bean id="transactionManagerLegacy" class="org.springframework.orm.jpa.JpaTransactionManager"&gt;
        &lt;property name="entityManagerFactory" ref="entityManagerFactoryLegacy" /&gt;
    &lt;/bean&gt;    
    &lt;bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"&gt;
        &lt;property name="persistenceXmlLocation" value="classpath:config/persistence-demo.xml" /&gt;
        &lt;property name="persistenceUnitName" value="demoRestPersistence" /&gt;        
        &lt;property name="dataSource" ref="restDemoDS" /&gt;
        &lt;property name="packagesToScan" value="org.codingpedia.demo.*" /&gt;
        &lt;property name="jpaVendorAdapter"&gt;
            &lt;bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"&gt;
                &lt;property name="showSql" value="true" /&gt;
                &lt;property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" /&gt;
            &lt;/bean&gt;
        &lt;/property&gt;
    &lt;/bean&gt;     
    &lt;bean id="entityManagerFactoryLegacy" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"&gt;
        &lt;property name="persistenceXmlLocation" value="classpath:config/persistence-demo.xml" /&gt;
        &lt;property name="persistenceUnitName" value="demoRestPersistenceLegacy" /&gt;
        &lt;property name="dataSource" ref="restDemoLegacyDS" /&gt;
        &lt;property name="packagesToScan" value="org.codingpedia.demo.*" /&gt;
        &lt;property name="jpaVendorAdapter"&gt;
            &lt;bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"&gt;
                &lt;property name="showSql" value="true" /&gt;
                &lt;property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" /&gt;
            &lt;/bean&gt;
        &lt;/property&gt;
    &lt;/bean&gt;        

	&lt;bean id="podcastDao" class="org.codingpedia.demo.rest.dao.impl.PodcastDaoJPA2Impl"/&gt;
    &lt;bean id="podcastRestService" class="org.codingpedia.demo.rest.service.PodcastRestService" /&gt;
    &lt;bean id="podcastLegacyRestService" class="org.codingpedia.demo.rest.service.PodcastLegacyRestService" /&gt;

	&lt;bean id="restDemoDS" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoDB" /&gt;
	    &lt;property name="resourceRef" value="true" /&gt;        
	&lt;/bean&gt;
	&lt;bean id="restDemoLegacyDS" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoLegacyDB" /&gt;
	    &lt;property name="resourceRef" value="true" /&gt;        
	&lt;/bean&gt;
&lt;/beans&gt;</pre>

Note the persistenceUnitName property of the entityManagerFactory is pointing to the corresponding persistence unit.

Here&#8217;s a quick recap of the main configured beans:

<li style="text-align: justify;">
  <strong>entityManagerFactoryLegacy </strong><code>(org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean)</code> is a <code>org.springframework.beans.factory.FactoryBean</code> that creates a JPA <code>javax.persistence.EntityManagerFactory</code> according to JPA&#8217;s standard container bootstrap contract. This is the most powerful way to set up a shared JPA EntityManagerFactory in a Spring application context; the <code>EntityManagerFactory</code> can then be passed to JPA-based DAOs via dependency injection. Note that switching to a JNDI lookup or to a <code>LocalEntityManagerFactoryBean</code> definition is just a matter of<br /> configuration!<br /> As with <code>LocalEntityManagerFactoryBean</code>, configuration settings are usually read in from a <code>META-INF/persistence.xml</code> config file, residing in the class path, according to the general JPA configuration contract. However, this <code>FactoryBean</code> is more flexible in that you can override the location of the persistence.xml file (as here the case), specify the JDBC DataSources to link to, etc. Furthermore, it allows for pluggable class instrumentation through Spring&#8217;s <code>org.springframework.instrument.classloading.LoadTimeWeaver</code> abstraction, instead of being tied to a special VM agent specified on JVM startup.
</li>
<li style="text-align: justify;">
  <strong>transactionManagerLegacy</strong> <code>(org.springframework.orm.jpa.JpaTransactionManager)</code> is a <code>org.springframework.transaction.PlatformTransactionManager</code> implementation for a single JPA <code>javax.persistence.EntityManagerFactory</code>. Binds a JPA <code>EntityManager</code> from the specified factory to the thread, potentially allowing for one thread-bound EntityManager per factory. <code>SharedEntityManagerCreator</code> and <code>JpaTemplate</code> are aware of thread-bound entity managers and participate in such transactions automatically. Using either is required for JPA access code supporting this transaction management mechanism.<br /> <em>This transaction manager is appropriate for applications that use a single JPA EntityManagerFactory for transactional data access. JTA (usually through <code>org.springframework.transaction.jta.JtaTransactionManager</code>) is necessary for accessing multiple transactional resources within the same transaction. Note that you need to configure your JPA provider accordingly in order to make it participate in JTA transactions.</em>
</li>

### <span id="23_In_code">2.3. In code</span>

As mentioned before to highlight the multiple data source configuration in code I extended the DAO layer class with methods to access the &#8220;legacy&#8221; system:

<pre class="lang:java mark:24-25,38 decode:true" title="Multiple data source configuration in code">package org.codingpedia.demo.rest.dao.impl;

import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;
import java.util.List;
import java.util.TimeZone;

import javax.persistence.EntityManager;
import javax.persistence.NoResultException;
import javax.persistence.PersistenceContext;
import javax.persistence.Query;
import javax.persistence.TemporalType;
import javax.persistence.TypedQuery;

import org.codingpedia.demo.rest.dao.PodcastDao;
import org.codingpedia.demo.rest.entities.Podcast;

public class PodcastDaoJPA2Impl implements PodcastDao {

	@PersistenceContext(unitName="demoRestPersistence")
	private EntityManager entityManager;

	@PersistenceContext(unitName="demoRestPersistenceLegacy")
	private EntityManager entityManagerLegacy;

	public List&lt;Podcast&gt; getPodcasts() {

		String qlString = "SELECT p FROM Podcast p";
		TypedQuery&lt;Podcast&gt; query = entityManager.createQuery(qlString, Podcast.class);		

		return query.getResultList();
	}
	/* .............................*/
	public List&lt;Podcast&gt; getLegacyPodcasts() {

		String qlString = "SELECT p FROM Podcast p";
		TypedQuery&lt;Podcast&gt; query = entityManagerLegacy.createQuery(qlString, Podcast.class);		

		return query.getResultList();
	}

}</pre>

You see here how you can use multiple entity managers in the same class. And we that we conclude the configuration example of accessing multiple data sources.

<p class="note_code" style="text-align: justify;">
  <strong>Code Tip</strong> &#8211; you can best see the differences to the code with one data source configuration by having a look at my GitHub commit for this post at <strong><a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/commit/f37d52d88348eb125097a561e9105d61fffa08c3" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/commit/f37d52d88348eb125097a561e9105d61fffa08c3" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/commit/f37d52d88348eb125097a561e9105d61fffa08c3</a></strong>
</p>

<h4 style="text-align: justify;">
  <span id="231_Using_Transactional">2.3.1. Using @Transactional</span>
</h4>

If you are using transactions via the @Transactional annotation you need to specify the transaction manager you are using:

<pre class="lang:java mark:2 decode:true" title="Specify transaction manager name">/********************* Create related methods implementation ***********************/
@Transactional("transactionManager")
public Long createPodcast(Podcast podcast) throws AppException {

	validateInputForCreation(podcast);

	//verify existence of resource in the db (feed must be unique)
	PodcastEntity podcastByFeed = podcastDao.getPodcastByFeed(podcast.getFeed());
	if(podcastByFeed != null){
		throw new AppException(Response.Status.CONFLICT.getStatusCode(), 409, "Podcast with feed already existing in the database with the id " + podcastByFeed.getId(),
				"Please verify that the feed and title are properly generated", AppConstants.BLOG_POST_URL);
	}

	return podcastDao.createPodcast(new PodcastEntity(podcast));
}</pre>

## <span id="3_Testing_it_8211_optional">3. Testing it &#8211; optional</span>

To test the new &#8220;legacy&#8221; functionality you can start the jetty server configured in the `pom.xml`, file by issuing the following maven command in the root directory of the project:

<pre class="lang:default decode:true">mvn jetty:run -Djetty.port=8888</pre>

Once Jetty is started execute GET operations on the following URIs:

  * <a title="http://localhost:8888/demo-rest-spring-jersey-jpa2-hibernate-0.0.1-SNAPSHOT/legacy/podcasts" href="http://localhost:8888/demo-rest-spring-jersey-jpa2-hibernate-0.0.1-SNAPSHOT/legacy/podcasts" target="_blank">http://localhost:8888/demo-rest-spring-jersey-jpa2-hibernate-0.0.1-SNAPSHOT/legacy/podcasts</a>
  * <a title="http://localhost:8888/demo-rest-spring-jersey-jpa2-hibernate-0.0.1-SNAPSHOT/legacy/podcasts/2" href="http://localhost:8888/demo-rest-spring-jersey-jpa2-hibernate-0.0.1-SNAPSHOT/legacy/podcasts/2" target="_blank">http://localhost:8888/demo-rest-spring-jersey-jpa2-hibernate-0.0.1-SNAPSHOT/legacy/podcasts/2</a>

to get all podcasts, or respectively a specific podcast from the &#8220;legacy&#8221; system.

You can also have look at my video on how to test a REST API with the <a title="https://plus.google.com/104025798250320128549/posts" href="https://plus.google.com/104025798250320128549/posts" target="_blank">DEV HTTP Client</a>



Well, that&#8217;s it. You&#8217;ve learned how to configure multiple a Spring application to access multiple data sources via JPA.

<p class="note_normal">
  If you&#8217;ve found it useful, please help it spread by sharing it on Twitter, Google+ or Facebook. Thank you! Don&#8217;t forget also to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you&#8217;ll find for sure interesting podcasts and episodes. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

## <span id="4_Resources">4. Resources</span>

### <span id="41_Codingpedia">4.1. Codingpedia</span>

  * <a title="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate</a>
  * <a title="http://www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" href="http://www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" target="_blank">RESTful Web Services Example in Java with Jersey, Spring and MyBatis</a>
  * <a title="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" href="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" target="_blank">Tomcat JDBC Connection Pool configuration for production and development</a>

### <span id="42_GitHub">4.2. GitHub</span>

  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate" target="_blank">Source code for <strong>Java Persistence Example with Spring, JPA2 and Hibernate </strong></a>( encapsulates code for this post)
  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/commit/f37d52d88348eb125097a561e9105d61fffa08c3" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/commit/f37d52d88348eb125097a561e9105d61fffa08c3" target="_blank">Differences to one datasource version</a>

### <span id="43_Web">4.3. Web</span>

  * <a title="http://stackoverflow.com/questions/3111992/difference-between-configuring-data-source-in-persistence-xml-and-in-spring-conf" href="http://stackoverflow.com/questions/3111992/difference-between-configuring-data-source-in-persistence-xml-and-in-spring-conf" target="_blank">StackOverflow &#8211; Difference between configuring data source in persistence.xml and in spring configuration files</a>
  * <a title="http://stackoverflow.com/questions/1961371/spring-multiple-transactional-datasource" href="http://stackoverflow.com/questions/1961371/spring-multiple-transactional-datasource" target="_blank">http://stackoverflow.com/questions/1961371/spring-multiple-transactional-datasource</a>

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
    </div>

    <div class="clear">
    </div>
  </div>
</div>
