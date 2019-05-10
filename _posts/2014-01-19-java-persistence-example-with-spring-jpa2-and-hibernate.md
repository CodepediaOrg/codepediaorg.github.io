---
id: 1097
title: Java Persistence Example with Spring, JPA2 and Hibernate
date: 2014-01-19T22:51:41+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1097
permalink: /ama/java-persistence-example-with-spring-jpa2-and-hibernate/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 0
dsq_thread_id:
  - 2145789827
fsb_social_google:
  - 11
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:68:"https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate";s:11:"ribbon-type";i:4;}'
categories:
  - java
  - spring
tags:
  - demo
  - example
  - hibernate
  - jpa
  - jpa2
  - persistence
  - rest
---

<p style="text-align: justify;">
  This is sort of a follow up post for a previous post of mine &#8211; <a title="RESTful Web Services Example in Java with Jersey, Spring and MyBatis" href="http://www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" target="_blank">RESTful Web Services Example in Java with Jersey, Spring and MyBatis.</a> It is based on the same example application, which, via a REST API, can execute CRUD operations against a single Podcasts table. If in the first post the focus was on how to build the REST API with Jersey, this time <span style="font-size: medium;"><strong>it is on the data persistence layer</strong></span>. I will present how to implement a container-agnostic persistence layer with JPA2/Hibernate, being glued in the application via Spring.
</p>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Architecture_and_technologies">1. Architecture and technologies</a><ul>
        <li>
          <a href="#11_Architecture">1.1. Architecture</a><ul>
            <li>
              <a href="#12_Technologies_used">1.2. Technologies used</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Source_Code">2. Source Code</a>
    </li>
    <li>
      <a href="#3_The_coding">3. The coding</a><ul>
        <li>
          <a href="#31_Configuration">3.1. Configuration</a><ul>
            <li>
              <a href="#311_Project_dependencies">3.1.1. Project dependencies</a>
            </li>
            <li>
              <a href="#312_Spring_application_context_configuration">3.1.2. Spring application context configuration</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#32_The_Data_Persistence_layer">3.2. The Data Persistence layer</a><ul>
            <li>
              <a href="#321_CREATE">3.2.1. CREATE</a>
            </li>
            <li>
              <a href="#322_READ">3.2.2. READ</a>
            </li>
            <li>
              <a href="#323_UPDATE">3.2.3. UPDATE</a>
            </li>
            <li>
              <a href="#324_DELETE">3.2.4. DELETE</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#4_Resources">4. Resources</a><ul>
        <li>
          <a href="#41_Source_Code">4.1. Source Code</a>
        </li>
        <li>
          <a href="#42_JPA_resources">4.2. JPA resources</a>
        </li>
        <li>
          <a href="#43_Rest_resources">4.3. Rest resources</a>
        </li>
        <li>
          <a href="#44_Codingpedia_related_resources">4.4. Codingpedia related resources</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Appendix">Appendix</a><ul>
        <li>
          <a href="#A_8211_testing_the_demo_application_with_DEV_HTTP_Client">A &#8211; testing the demo application with DEV HTTP Client</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<h2 style="text-align: justify;">
  <span id="1_Architecture_and_technologies"><span id="Architecture_and_technologies">1. Architecture and technologies</span></span>
</h2>

### <span id="11_Architecture">1.1. Architecture</span>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/01/Example-architecture.png"><img class="alignnone size-medium wp-image-1098" src="{{site.url}}/wp-content/uploads/2014/01/Example-architecture-300x140.png" alt="Example architecture" width="300" height="140" srcset="{{site.url}}/wp-content/uploads/2014/01/Example-architecture-300x140.png 300w, {{site.url}}/wp-content/uploads/2014/01/Example-architecture.png 602w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

#### <span id="12_Technologies_used"><span id="Technologies_version">1.2. Technologies used</span></span>

  1. Jersey 2.4
  2. Spring 3.2
  3. JPA 2
  4. Maven 3
  5. Tomcat 7
  6. Jetty 9
  7. MySql 5.6
  8. Hibernate 4

## <span id="2_Source_Code"><span id="Follow_along">2. Source Code</span></span>

If you want to follow along, you find all you need on GitHub:

<li style="text-align: justify;">
  <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate</a>
</li>
<li style="text-align: justify;">
  <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/resources/input_data/DumpRESTdemoDB.sql" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/resources/input_data/DumpRESTdemoDB.sql" target="_blank">MySQL DB creation self-contained file</a> – everything is configured for the <code>username/password</code> : <code>rest_demo/rest_demo</code>
</li>

## <span id="3_The_coding"><span id="The_coding">3. The coding</span></span>

### <span id="31_Configuration"><span id="Configuration">3.1. Configuration</span></span>

#### <span id="311_Project_dependencies"><span id="Project_dependencies">3.1.1. Project dependencies</span></span>

<p style="text-align: justify;">
  I use Maven to build the project. In addition to Spring Core and persistence dependencies, we also need to define Hibernate in the <code>pom.xml</code>:
</p>

<pre>
  <code class="xml">
    &lt;!-- ******* JPA/Hibernate ******** --&gt;
    &lt;dependency&gt;
    	&lt;groupId&gt;org.hibernate&lt;/groupId&gt;
    	&lt;artifactId&gt;hibernate-core&lt;/artifactId&gt;
    	&lt;version&gt;${hibernate.version}&lt;/version&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;
    	&lt;groupId&gt;org.hibernate.javax.persistence&lt;/groupId&gt;
    	&lt;artifactId&gt;hibernate-jpa-2.0-api&lt;/artifactId&gt;
    	&lt;version&gt;1.0.1.Final&lt;/version&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;
    	&lt;groupId&gt;org.hibernate&lt;/groupId&gt;
    	&lt;artifactId&gt;hibernate-entitymanager&lt;/artifactId&gt;
    	&lt;version&gt;${hibernate.version}&lt;/version&gt;
    &lt;/dependency&gt;
  </code>
</pre>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> If you want to see what other dependencies (Jersey, Spring, Jetty, testing etc.) are used in the project or how the <code>jetty-maven-plugin</code> is configured so that you can start the project in Jetty directly from Eclipse, you can download the complete <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/pom.xml" target="_blank">pom.xml</a> file from GitHub – <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/pom.xml" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/pom.xml</a>
</p>

#### <span id="312_Spring_application_context_configuration"><span id="Spring_application_context_configuration">3.1.2. Spring application context configuration</span></span>

The Spring application context configuration is located at `classpath:spring/applicationContext.xml`:

<pre>
  <code class="xml">
    &lt;beans xmlns="http://www.springframework.org/schema/beans"
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
        &lt;bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"&gt;
            &lt;property name="persistenceXmlLocation" value="classpath:config/persistence-demo.xml" /&gt;
            &lt;property name="dataSource" ref="restDemoDS" /&gt;
            &lt;property name="packagesToScan" value="org.codingpedia.demo.*" /&gt;
            &lt;property name="jpaVendorAdapter"&gt;
                &lt;bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"&gt;
                    &lt;property name="showSql" value="true" /&gt;
                    &lt;property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" /&gt;
                &lt;/bean&gt;
            &lt;/property&gt;
        &lt;/bean&gt;

    	&lt;bean id="podcastDao" class="org.codingpedia.demo.rest.dao.impl.PodcastDaoJPA2Impl"/&gt;
        &lt;bean id="podcastRestService" class="org.codingpedia.demo.rest.service.PodcastRestService" &gt;
        	&lt;property name="podcastDao" ref="podcastDao"/&gt;
        &lt;/bean&gt;

    	&lt;bean id="restDemoDS" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
    	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoDB" /&gt;
    	    &lt;property name="resourceRef" value="true" /&gt;
    	&lt;/bean&gt;
    &lt;/beans&gt;
  </code>
</pre>

Relevant JPA beans:

<li style="text-align: justify;">
  <code>transactionManager</code> &#8211; note that the transaction manager used is <code>JpaTransactionManager</code>, instead of the <code>DataSourceTransactionManager</code>, which was the case when building the persistence layer with MyBatis.
</li>
<li style="text-align: justify;">
  <code>entityManagerFactory</code> &#8211; the <code>LocalContainerEntityManagerFactoryBean</code> gives full control over <code>EntityManagerFactory</code> configuration and is appropriate for environments where fine-grained customization is required. It is a <a title="interface in org.springframework.beans.factory" href="http://docs.spring.io/spring/docs/3.1.x/javadoc-api/org/springframework/beans/factory/FactoryBean.html"><code>FactoryBean</code></a> that creates a JPA <a title="class or interface in javax.persistence" href="http://download.oracle.com/javaee/6/api/javax/persistence/EntityManagerFactory.html?is-external=true"><code>EntityManagerFactory</code></a> according to JPA&#8217;s standard <i>container</i> bootstrap contract. This is the most powerful way to set up a shared JPA EntityManagerFactory in a Spring application context; the EntityManagerFactory can then be passed to JPA-based DAOs via dependency injection. <ul>
    <li>
      <code>persistenceXmlLocation</code> &#8211; Set the location of the <code>persistence.xml</code> file we want to use. This is a Spring resource location. Default is <code>classpath:META-INF/persistence.xml</code>.
    </li>
    <li>
      <code>jpaVendorAdapter</code> &#8211; the <code>HibernateJpaVendorAdapter</code> setup to recognize the MySQL dialect
    </li>
  </ul>

  <p class="note_normal">
    Note that switching to a JNDI lookup or to a <a title="class in org.springframework.orm.jpa" href="http://docs.spring.io/spring/docs/3.1.x/javadoc-api/org/springframework/orm/jpa/LocalEntityManagerFactoryBean.html"><code>LocalEntityManagerFactoryBean</code></a> definition, which are the other two options to setup JPA in a Spring environment, is just a matter of configuration!
  </p>
</li>

### <span id="32_The_Data_Persistence_layer">3.2. The Data Persistence layer</span>

<p style="text-align: justify;">
  As mentioned before the data persistence layer permits CRUD operations against the database and is triggered via REST requests.
</p>

<p style="text-align: justify;">
  There have been lots of discussions whether using DAOs is still relevant when using JPA &#8211; you can find some resources on this topic at the end of the post. I, for one, still employ DAOs, as I don&#8217;t like my service classes cluttered with EntityManager/JPA specific code. What if I want to use MyBatis or other persistence technology?
</p>

So the contract between the service layer and the data persistence layer is done via the `PodcastDao` interface:

<pre>
  <code class="java">
    public interface PodcastDao {

    	public List&lt;Podcast&gt; getPodcasts();

    	/**
    	 * Returns a podcast given its id
    	 *
    	 * @param id
    	 * @return
    	 */
    	public Podcast getPodcastById(Long id);

    	public Long deletePodcastById(Long id);

    	public Long createPodcast(Podcast podcast);

    	public int updatePodcast(Podcast podcast);

    	/** removes all podcasts */
    	public void deletePodcasts();

    }
  </code>
</pre>

For this interface I provide a `PodcastDaoJPA2Impl` JPA-specific implementation class:

<pre>
  <code class="java">
    public class PodcastDaoJPA2Impl implements PodcastDao {

    	@PersistenceContext
    	private EntityManager em;

    	.......................

    }
  </code>
</pre>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> You can find the <a title="PodcastDaoJPA2Impl" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/src/main/java/org/codingpedia/demo/rest/dao/impl/PodcastDaoJPA2Impl.java" target="_blank">complete implementation of PodcastDaoJPA2Impl on GitHub </a> &#8211; I will present the code split and give some JPA related explanations bellow.
</p>

**EntityManager**

<p style="padding-left: 30px; text-align: justify;">
  The EntityManager API is used to access a database in a particular unit of work. It is used to create and remove persistent entity instances, to find entities by their primary key identity, and to query over all entities. This interface is similar to the Session in Hibernate.
</p>

<p style="text-align: justify;">
  <strong>Persistence context</strong>
</p>

<p style="text-align: justify; padding-left: 30px;">
  A persistence context is a set of entity instances in which for any persistent entity identity there is a unique entity instance. Within the persistence context, the entity instances and their lifecycle is managed by a particular entity manager. The scope of this context can either be the transaction, or an extended unit of work.
</p>

<p class="note_normal" style="text-align: justify;">
  Note: When the persistence layer was implemented with MyBatis, there was no need to have an implementation class of the interface, as one was provided at runtime via the MapperFactoryBean. See my post <a title="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" href="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" target="_blank">Spring MyBatis integration example</a>, for complete details how to integrate MyBatis in Spring.
</p>

<p style="text-align: justify;">
  Now let&#8217;s see how the CRUD operations are implemented with JPA:
</p>

<h4 style="text-align: justify;">
  <span id="321_CREATE">3.2.1. CREATE</span>
</h4>

<pre>
  <code class="java">
    public Long createPodcast(Podcast podcast) {

    	entityManager.persist(podcast);
    	entityManager.flush();//force insert to receive the id of the podcast

    	return podcast.getId();
    }
  </code>
</pre>

<p style="text-align: justify;">
  To insert an entity in the database, you can use either <code>persist</code> or <code>merge</code>, whereas if <code>persist</code> is sufficient you should use it. There&#8217;s a nice article, <a title="http://spitballer.blogspot.de/2010/04/jpa-persisting-vs-merging-entites.html" href="http://spitballer.blogspot.de/2010/04/jpa-persisting-vs-merging-entites.html" target="_blank">JPA: persisting vs. merging entites</a>, that explains the difference between the two. In my case <code>persist</code> was enough.
</p>

<p style="text-align: justify;">
  I use the <code>flush()</code> method of the <code>EntityManager</code> to force the insertion of the entity and synchronize the persistence context with the underlying database, so that I can return the new id of podcast being inserted.
</p>

<p class="note_normal" style="padding-left: 30px;">
  <strong>Note:</strong> The transaction context is set via the <code>@Transcational</code> Spring annotation at the caller of the DAO class &#8211; <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java" target="_blank">PodcastRestService</a>:
</p>

<pre>
  <code class="java">
    @POST
    @Consumes({MediaType.APPLICATION_JSON})
    @Produces({MediaType.TEXT_HTML})
    @Transactional
    public Response createPodcast(Podcast podcast) {
    		podcastDao.createPodcast(podcast);

    		return Response.status(201).entity("A new podcast/resource has been created").build();
    }
  </code>
</pre>

<p style="text-align: justify;">
  Please see these articles: <a title="http://www.kumaranuj.com/2013/06/jpa-2-entitymanagers-transactions-and.html" href="http://www.kumaranuj.com/2013/06/jpa-2-entitymanagers-transactions-and.html" target="_blank">JPA 2 | EntityManagers, Transactions and everything around it </a>and <a title="http://en.wikibooks.org/wiki/Java_Persistence/Transactions" href="http://en.wikibooks.org/wiki/Java_Persistence/Transactions" target="_blank">Java Persistence/Transactions</a> for a better understanding of the JPA mechanismus for transactions.
</p>

<h4 style="text-align: justify;">
  <span id="322_READ">3.2.2. READ</span>
</h4>

<pre>
  <code class="java">
    public List&lt;Podcast&gt; getPodcasts() {

    	String qlString = "SELECT p FROM Podcast p";
    	TypedQuery&lt;Podcast&gt; query = entityManager.createQuery(qlString, Podcast.class);

    	return query.getResultList();
    }

    public Podcast getPodcastById(Long id) {

    	try {
    		String qlString = "SELECT p FROM Podcast p WHERE p.id = ?1";
    		TypedQuery&lt;Podcast&gt; query = entityManager.createQuery(qlString, Podcast.class);
    		query.setParameter(1, id);

    		return query.getSingleResult();
    	} catch (NoResultException e) {
    		return null;
    	}
    }
  </code>
</pre>

For the reading operations I use `TypedQuery`, which is an extension of the `java.persistence.Query`, and, as the name suggets, knows the type it returns as a result of its execution.

<h4 style="text-align: justify;">
  <span id="323_UPDATE">3.2.3. UPDATE</span>
</h4>

<pre>
  <code class="java">
     int updatePodcast(Podcast podcast) {

    	entityManager.merge(podcast);

    	return 1;
    }
  </code>
</pre>

To update the podcast I simply use the `merge` method of the `EntityManager`.

<h4 style="text-align: justify;">
  <span id="324_DELETE">3.2.4. DELETE</span>
</h4>

<pre>
  <code class="java">
    public Long deletePodcastById(Long id) {

    	Podcast podcast = entityManager.find(Podcast.class, id);
    	entityManager.remove(podcast);

    	return 1L;
    }
  </code>
</pre>

The deletion of an entity is executed with the `remove` method of the `EntityManager`, after the entity has been loaded into the `PersistenceContext` with the help of the `find` method.

<pre>
  <code class="java">
    public void deletePodcasts() {
    	Query query = entityManager.createNativeQuery("TRUNCATE TABLE podcasts");
    	query.executeUpdate();
    }
  </code>
</pre>

For the deletion of all podcasts, I used a `TRUNCATE` command against the MySQL database. Native queries are made possible via the `createNativeQuery` method of the `EntityManager`.

Well, that&#8217;s it. You&#8217;ve seen how to configure Spring with JPA/Hibernate to build the data persistence layer of an application, and how to use simple methods of the EntityManager to execute CRUD operations against a database.

<p class="note_normal">
  If you&#8217;ve found some usefulness in this post this, I&#8217;d be very grateful if you&#8217;d help it spread by sharing it on Twitter, Google+ or Facebook. Thank you! Don&#8217;t forget also to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you&#8217;ll find for sure interesting podcasts and episodes. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

## <span id="4_Resources"><span id="Resources-2">4. Resources</span></span>

### <span id="41_Source_Code"><span id="Source_Code">4.1. Source Code</span></span>

  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis</a>
  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/tree/master/src/main/resources/input_data" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-jpa2-hibernate/tree/master/src/main/resources/input_data" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/tree/master/src/main/resources/input_data </a>(create db schema and restore data .sql files)

### <span id="42_JPA_resources">4.2. JPA resources</span>

  * <a title="http://en.wikibooks.org/wiki/Java_Persistence/" href="http://en.wikibooks.org/wiki/Java_Persistence" target="_blank">Java Persistence &#8211; open book </a>
  * <a title="http://docs.spring.io/spring/docs/3.2.6.RELEASE/spring-framework-reference/htmlsingle/#orm-jpa" href="http://docs.spring.io/spring/docs/3.2.6.RELEASE/spring-framework-reference/htmlsingle/#orm-jpa" target="_blank">Spring ORM-JPA </a>
  * <a title="http://www.infoq.com/news/2007/09/jpa-dao" href="http://www.infoq.com/news/2007/09/jpa-dao" target="_blank">Has JPA Killed the DAO?</a>
  * <a title="http://en.wikipedia.org/wiki/Java_Persistence_API" href="http://en.wikipedia.org/wiki/Java_Persistence_API" target="_blank">Java Persistance API &#8211; Wikipedia.org</a>
  * <a title="http://en.wikipedia.org/wiki/Data_access_object" href="http://en.wikipedia.org/wiki/Data_access_object" target="_blank">Data Access Object (DAO)</a>
  * <a title="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD &#8211; Wikipedia</a>
  * <a title="http://docs.jboss.org/hibernate/entitymanager/3.5/reference/en/html/architecture.html" href="http://docs.jboss.org/hibernate/entitymanager/3.5/reference/en/html/architecture.html" target="_blank">http://docs.jboss.org/hibernate/entitymanager/3.5/reference/en/html/architecture.html</a>
  * <a title="http://spitballer.blogspot.de/2010/04/jpa-persisting-vs-merging-entites.html" href="http://spitballer.blogspot.de/2010/04/jpa-persisting-vs-merging-entites.html" target="_blank">JPA: persisting vs. merging entites </a>
  * <a title="http://www.kumaranuj.com/2013/06/jpa-2-entitymanagers-transactions-and.html" href="http://www.kumaranuj.com/2013/06/jpa-2-entitymanagers-transactions-and.html" target="_blank">JPA 2 | EntityManagers, Transactions and everything around it </a>
  * <a title="http://en.wikibooks.org/wiki/Java_Persistence/Transactions" href="http://en.wikibooks.org/wiki/Java_Persistence/Transactions" target="_blank">Java Persistence/Transactions</a>
  * <a title="http://java.dzone.com/articles/transaction-configuration-jpa" href="http://java.dzone.com/articles/transaction-configuration-jpa" target="_blank">Transaction configuration with JPA and Spring 3.1</a>
  * <a title="http://www.baeldung.com/2011/12/13/the-persistence-layer-with-spring-3-1-and-jpa/" href="http://www.baeldung.com/2011/12/13/the-persistence-layer-with-spring-3-1-and-jpa/" target="_blank">Spring 3 and JPA with Hibernate</a>

### <span id="43_Rest_resources"><span id="Rest_resources">4.3. Rest resources</span></span>

  * <a href="http://en.wikipedia.org/wiki/Representational_State_Transfer" target="_blank">http://en.wikipedia.org/wiki/Representational_State_Transfer</a>
  * <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">http://en.wikipedia.org/wiki/Create,_read,_update_and_delete</a>
  * <a title="Java API for RESTful Services (JAX-RS)" href="https://jax-rs-spec.java.net/" target="_blank">Java API for RESTful Services (JAX-RS)</a>
  * <a title="Jersey REST" href="https://jersey.java.net/" target="_blank">Jersey – RESTful Web Services in Java </a>
  * <a title="Status Code Definitions" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP Status Code Definitions</a>

### <span id="44_Codingpedia_related_resources"><span id="Codingpedia_resources">4.4. Codingpedia related resources</span></span>

  * <a title="www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" href="www.codingpedia.org/ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/" target="_blank">RESTful Web Services Example in Java with Jersey, Spring and MyBatis</a>
  * <a title="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" href="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" target="_blank">Spring MyBatis integration example</a>

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

## <span id="Appendix">Appendix</span>

### <span id="A_8211_testing_the_demo_application_with_DEV_HTTP_Client">A &#8211; testing the demo application with <a title="https://plus.google.com/104025798250320128549/posts" href="https://plus.google.com/104025798250320128549/posts" target="_blank">DEV HTTP Client</a></span>
<iframe width="420" height="315" src="https://www.youtube.com/embed/_71JXeP5FNc" frameborder="0" allowfullscreen></iframe>
