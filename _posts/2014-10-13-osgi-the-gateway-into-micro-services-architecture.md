---
id: 1986
title: 'OSGi: the gateway into micro-services architecture'
date: 2014-10-13T20:54:05+00:00
author: Andriy Redko
layout: post
guid: http://www.codingpedia.org/?p=1986
permalink: /aredko/osgi-the-gateway-into-micro-services-architecture/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:37:"https://github.com/reta/osgi-services";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 3113202146
fsb_social_facebook:
  - 2
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - architecture
  - java
tags:
  - apache cxf
  - Apache Karaf
  - hibernate
  - jackson
  - jax-rs 2.0
  - jpa
  - json
  - micro-services
  - microservice
  - modularity
  - openjpa
  - OSGi
  - rest
  - SOA
---
<p style="text-align: justify;">
  The terms &#8220;modularity&#8221; and &#8220;microservices architecture&#8221; pop up quite often these days in context of building scalable, reliable distributed systems. Java platform itself is known to be weak with regards to modularity (<a style="color: #888855;" href="http://openjdk.java.net/projects/jdk9/">Java 9</a> is going to address that by delivering project <a style="color: #888855;" href="http://openjdk.java.net/projects/jigsaw/">Jigsaw</a>), giving a chance to frameworks like <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> and <a style="color: #888855;" href="https://docs.jboss.org/author/display/MODULES/Introduction">JBoss Modules</a> to emerge.
</p>

<p style="text-align: justify;">
  When I first heard about <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> back in 2007, I was truly excited about all those advantages Java applications might benefit of by being built on top of it. But very quickly the frustration took place instead of excitement: no tooling support, very limited set of compatible libraries and frameworks, quite unstable and hard to troubleshoot runtime. Clearly, it was not ready to be used by average Java developer and as such, I had to put it on the shelf. With years, <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> has matured a lot and gained a widespread community support.<!--more-->
</p>

<p style="color: #333333;">
  The curious reader may ask: what are the benefits of using modules and <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> in particular? To name just a few problems it helps to solve:
</p>

<ul style="color: #333333;">
  <li>
    explicit (and versioned) dependency management: modules declare what they need (and optionally the version ranges)
  </li>
  <li>
    small footprint: modules are not packaged with all their dependencies
  </li>
  <li>
    easy release: modules can be developed and released independently
  </li>
  <li>
    hot redeploy: individual modules may be redeployed without affecting others
  </li>
</ul>

In today&#8217;s post we are going to take a 10000 feet view on a state of the art in building modular Java applications using <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a>. Leaving aside discussions how good or bad <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> is, we are going to build an example application consisting of following modules:

<ul style="color: #333333;">
  <li>
    data access module
  </li>
  <li>
    business services module
  </li>
  <li>
    <a style="color: #888855;" href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a> services module
  </li>
</ul>

<p style="text-align: justify;">
  <a style="color: #888855;" href="http://openjpa.apache.org/">Apache OpenJPA 2.3.0</a><span style="color: #333333;"> / </span><a style="color: #888855;" href="https://jcp.org/aboutJava/communityprocess/final/jsr317/">JPA 2.0</a><span style="color: #333333;"> for data access (unfortunately, </span><a style="color: #888855;" href="https://jcp.org/en/jsr/detail?id=338">JPA 2.1</a><span style="color: #333333;"> is not yet supported by </span><a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> <span style="color: #333333;">implementation of our choice), </span><a style="color: #888855;" href="http://cxf.apache.org/">Apache CXF 3.0.1</a><span style="color: #333333;"> / </span><a style="color: #888855;" href="http://download.oracle.com/otndocs/jcp/jaxrs-2_0-fr-eval-spec/">JAX-RS 2.0</a><span style="color: #333333;"> for </span><a style="color: #888855;" href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a><span style="color: #333333;"> layer are two main building blocks of the application. I found </span><b style="color: #333333;">Christian Schneider</b><span style="color: #333333;">&#8216;s blog, </span><a style="color: #888855;" href="http://www.liquid-reality.de/display/liquid/Liquid+Reality+-+Christian+Schneider's+Blog">Liquid Reality</a><span style="color: #333333;">, to be invaluable source of information about </span><a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a><span style="color: #333333;"> (as well as many other topics).</span>
</p>

<p style="text-align: justify;">
  In <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> world, the modules are called <b>bundles</b>. Bundles manifest their dependencies (import packages) and the packages they expose (export packages) so other bundles are able to use them. <a style="color: #888855;" href="http://maven.apache.org/">Apache Maven</a> supports this packaging model as well. The bundles are managed by <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> runtime, or container, which in our case is going to be <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> (actually, it is the single thing we need to <a style="color: #888855;" href="http://karaf.apache.org/index/community/download.html">download</a> and unpack).
</p>

<p style="color: #333333;">
  Let me stop talking and better show some code. We are going to start from the top (<a style="color: #888855;" href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a>) and go all the way to the bottom (data access) as it would be easier to follow. Our <b>PeopleRestService</b> is a typical example of <a style="color: #888855;" href="http://download.oracle.com/otndocs/jcp/jaxrs-2_0-fr-eval-spec/">JAX-RS 2.0</a> service implementation:
</p>

<pre class="lang:java decode:true">package com.example.jaxrs;

import java.util.Collection;

import javax.ws.rs.DELETE;
import javax.ws.rs.DefaultValue;
import javax.ws.rs.FormParam;
import javax.ws.rs.GET;
import javax.ws.rs.POST;
import javax.ws.rs.PUT;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.QueryParam;
import javax.ws.rs.core.Context;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;
import javax.ws.rs.core.UriInfo;

import com.example.data.model.Person;
import com.example.services.PeopleService;

@Path( "/people" )
public class PeopleRestService {
    private PeopleService peopleService;
        
    @Produces( { MediaType.APPLICATION_JSON } )
    @GET
    public Collection&lt; Person &gt; getPeople( 
            @QueryParam( "page") @DefaultValue( "1" ) final int page ) {
        return peopleService.getPeople( page, 5 );
    }

    @Produces( { MediaType.APPLICATION_JSON } )
    @Path( "/{email}" )
    @GET
    public Person getPerson( @PathParam( "email" ) final String email ) {
        return peopleService.getByEmail( email );
    }

    @Produces( { MediaType.APPLICATION_JSON  } )
    @POST
    public Response addPerson( @Context final UriInfo uriInfo,
            @FormParam( "email" ) final String email, 
            @FormParam( "firstName" ) final String firstName, 
            @FormParam( "lastName" ) final String lastName ) {
        
        peopleService.addPerson( email, firstName, lastName );
        return Response.created( uriInfo
            .getRequestUriBuilder()
            .path( email )
            .build() ).build();
    }
    
    @Produces( { MediaType.APPLICATION_JSON  } )
    @Path( "/{email}" )
    @PUT
    public Person updatePerson( @PathParam( "email" ) final String email, 
            @FormParam( "firstName" ) final String firstName, 
            @FormParam( "lastName" )  final String lastName ) {
        
        final Person person = peopleService.getByEmail( email );
        
        if( firstName != null ) {
            person.setFirstName( firstName );
        }
        
        if( lastName != null ) {
            person.setLastName( lastName );
        }

        return person;              
    }
    
    @Path( "/{email}" )
    @DELETE
    public Response deletePerson( @PathParam( "email" ) final String email ) {
        peopleService.removePerson( email );
        return Response.ok().build();
    }
    
    public void setPeopleService( final PeopleService peopleService ) {
        this.peopleService = peopleService;
    }
}</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">As we can see, there is nothing here telling us about </span><a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a><span style="color: #333333;">. The only dependency is the </span><b style="color: #333333;">PeopleService</b><span style="color: #333333;"> which somehow should be injected into the </span><b style="color: #333333;">PeopleRestService</b><span style="color: #333333;">. How? Typically, </span><a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a><span style="color: #333333;"> applications use </span><a style="color: #888855;" href="http://aries.apache.org/modules/blueprint.html">blueprint</a><span style="color: #333333;"> as the dependency injection framework, very similar to old buddy, XML based </span><a style="color: #888855;" href="http://spring.io/">Spring</a><span style="color: #333333;"> configuration. It should be packaged along with application inside </span><b style="color: #333333;">OSGI-INF/blueprint</b><span style="color: #333333;"> folder. Here is a </span><a style="color: #888855;" href="http://aries.apache.org/modules/blueprint.html">blueprint</a><span style="color: #333333;"> example for our </span><a style="color: #888855;" href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a><span style="color: #333333;">module, built on top of </span><a style="color: #888855;" href="http://cxf.apache.org/">Apache CXF 3.0.1</a><span style="color: #333333;">:</span>
</p>

<pre class="lang:default decode:true ">&lt;blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jaxrs="http://cxf.apache.org/blueprint/jaxrs"
    xmlns:cxf="http://cxf.apache.org/blueprint/core"
    xsi:schemaLocation="
        http://www.osgi.org/xmlns/blueprint/v1.0.0 
        http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd
        http://cxf.apache.org/blueprint/jaxws 
        http://cxf.apache.org/schemas/blueprint/jaxws.xsd
        http://cxf.apache.org/blueprint/jaxrs 
        http://cxf.apache.org/schemas/blueprint/jaxrs.xsd
        http://cxf.apache.org/blueprint/core 
        http://cxf.apache.org/schemas/blueprint/core.xsd"&gt;

    &lt;cxf:bus id="bus"&gt;
        &lt;cxf:features&gt;
            &lt;cxf:logging/&gt;
        &lt;/cxf:features&gt;       
    &lt;/cxf:bus&gt;

    &lt;jaxrs:server address="/api" id="api"&gt;
        &lt;jaxrs:serviceBeans&gt;
            &lt;ref component-id="peopleRestService"/&gt;
        &lt;/jaxrs:serviceBeans&gt;
        &lt;jaxrs:providers&gt;
            &lt;bean class="com.fasterxml.jackson.jaxrs.json.JacksonJsonProvider" /&gt;
        &lt;/jaxrs:providers&gt;
    &lt;/jaxrs:server&gt;
    
    &lt;!-- Implementation of the rest service --&gt;
    &lt;bean id="peopleRestService" class="com.example.jaxrs.PeopleRestService"&gt;
        &lt;property name="peopleService" ref="peopleService"/&gt;
    &lt;/bean&gt;         
    
    &lt;reference id="peopleService" interface="com.example.services.PeopleService" /&gt;
&lt;/blueprint&gt;</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">Very small and simple: basically the configuration just states that in order for the module to work, the reference to the </span><b style="color: #333333;">com.example.services.PeopleService</b><span style="color: #333333;"> should be provided (effectively, by </span><a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a><span style="color: #333333;"> container). To see how it is going to happen, let us take a look on another module which exposes services. It contains only one interface </span><b style="color: #333333;">PeopleService</b><span style="color: #333333;">:</span>
</p>

<pre class="lang:java decode:true ">package com.example.services;

import java.util.Collection;

import com.example.data.model.Person;

public interface PeopleService {
    Collection&lt; Person &gt; getPeople( int page, int pageSize );
    Person getByEmail( final String email );
    Person addPerson( final String email, final String firstName, final String lastName );
    void removePerson( final String email );
}</pre>

<span style="color: #333333;">And also provides its implementation as </span><b style="color: #333333;">PeopleServiceImpl</b><span style="color: #333333;"> class:</span>

<pre class="lang:java decode:true ">package com.example.services.impl;

import java.util.Collection;

import org.osgi.service.log.LogService;

import com.example.data.PeopleDao;
import com.example.data.model.Person;
import com.example.services.PeopleService;

public class PeopleServiceImpl implements PeopleService {
    private PeopleDao peopleDao;
    private LogService logService;
 
    @Override
    public Collection&lt; Person &gt; getPeople( final int page, final int pageSize ) {        
        logService.log( LogService.LOG_INFO, "Getting all people" );
        return peopleDao.findAll( page, pageSize );
    }

    @Override
    public Person getByEmail( final String email ) {
        logService.log( LogService.LOG_INFO, 
            "Looking for a person with e-mail: " + email );
        return peopleDao.find( email );        
    }

    @Override
    public Person addPerson( final String email, final String firstName, 
            final String lastName ) {
        logService.log( LogService.LOG_INFO, 
            "Adding new person with e-mail: " + email );
        return peopleDao.save( new Person( email, firstName, lastName ) );
    }

    @Override
    public void removePerson( final String email ) {
        logService.log( LogService.LOG_INFO, 
            "Removing a person with e-mail: " + email );
        peopleDao.delete( email );
    }
    
    public void setPeopleDao( final PeopleDao peopleDao ) {
        this.peopleDao = peopleDao;
    }
    
    public void setLogService( final LogService logService ) {
        this.logService = logService;
    }
}
</pre>

<span style="color: #333333;">And this time again, very small and clean implementation with two injectable dependencies,</span><b style="color: #333333;">org.osgi.service.log.LogService</b><span style="color: #333333;"> and </span><b style="color: #333333;">com.example.data.PeopleDao</b><span style="color: #333333;">. Its </span><a style="color: #888855;" href="http://aries.apache.org/modules/blueprint.html">blueprint</a><span style="color: #333333;"> configuration, located inside </span><b style="color: #333333;">OSGI-INF/blueprint</b><span style="color: #333333;"> folder, looks quite compact as well:</span>

<pre class="lang:default decode:true">&lt;blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"         
    xsi:schemaLocation="
        http://www.osgi.org/xmlns/blueprint/v1.0.0 
        http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd"&gt;
        
    &lt;service ref="peopleService" interface="com.example.services.PeopleService" /&gt;        
    &lt;bean id="peopleService" class="com.example.services.impl.PeopleServiceImpl"&gt;
        &lt;property name="peopleDao" ref="peopleDao" /&gt;    
        &lt;property name="logService" ref="logService" /&gt;
    &lt;/bean&gt;
    
    &lt;reference id="peopleDao" interface="com.example.data.PeopleDao" /&gt;
    &lt;reference id="logService" interface="org.osgi.service.log.LogService" /&gt;
&lt;/blueprint&gt;</pre>

<p style="text-align: justify;">
  The references to <b>PeopleDao</b> and <b>LogService</b> are expected to be provided by <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> container at runtime. Hovewer, <b>PeopleService</b> implementation is exposed as service and <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> container will be able to inject it into <b>PeopleRestService</b> once its bundle is being activated.
</p>

<p style="color: #333333; text-align: justify;">
  The last piece of the puzzle, data access module, is a bit more complicated: it contains persistence configuration (<b>META-INF/persistence.xml</b>) and basically depends on <a style="color: #888855;" href="https://jcp.org/aboutJava/communityprocess/final/jsr317/">JPA 2.0</a> capabilities of the <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> container. The <b>persistence.xml</b> is quite basic:
</p>

<pre class="lang:default decode:true ">&lt;persistence xmlns="http://java.sun.com/xml/ns/persistence"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    version="2.0"&gt;
 
    &lt;persistence-unit name="peopleDb" transaction-type="JTA"&gt;
        &lt;jta-data-source&gt;
        osgi:service/javax.sql.DataSource/(osgi.jndi.service.name=peopleDb)
        &lt;/jta-data-source&gt;       
        &lt;class&gt;com.example.data.model.Person&lt;/class&gt;
        
        &lt;properties&gt;
            &lt;property name="openjpa.jdbc.SynchronizeMappings" 
                value="buildSchema"/&gt;         
        &lt;/properties&gt;        
    &lt;/persistence-unit&gt;
&lt;/persistence&gt;</pre>

<span style="color: #333333;">Similarly to the service module, there is an interface </span><b style="color: #333333;">PeopleDao</b><span style="color: #333333;"> exposed:</span>

<pre class="lang:java decode:true ">package com.example.data;

import java.util.Collection;

import com.example.data.model.Person;

public interface PeopleDao {
    Person save( final Person person );
    Person find( final String email );
    Collection&lt; Person &gt; findAll( final int page, final int pageSize );
    void delete( final String email ); 
}</pre>

<span style="color: #333333;">With its implementation </span><b style="color: #333333;">PeopleDaoImpl</b><span style="color: #333333;">:</span>

<pre class="lang:java decode:true">package com.example.data.impl;

import java.util.Collection;

import javax.persistence.EntityManager;
import javax.persistence.criteria.CriteriaBuilder;
import javax.persistence.criteria.CriteriaQuery;

import com.example.data.PeopleDao;
import com.example.data.model.Person;

public class PeopleDaoImpl implements PeopleDao {
    private EntityManager entityManager;
 
    @Override
    public Person save( final Person person ) {
        entityManager.persist( person );
        return person;
    }
 
    @Override
    public Person find( final String email ) {
        return entityManager.find( Person.class, email );
    }
 
    public void setEntityManager( final EntityManager entityManager ) {
        this.entityManager = entityManager;
    }
 
    @Override
    public Collection&lt; Person &gt; findAll( final int page, final int pageSize ) {
        final CriteriaBuilder cb = entityManager.getCriteriaBuilder();
     
        final CriteriaQuery&lt; Person &gt; query = cb.createQuery( Person.class );
        query.from( Person.class );
     
        return entityManager
            .createQuery( query )
            .setFirstResult(( page - 1 ) * pageSize )
            .setMaxResults( pageSize ) 
            .getResultList();
    }
 
    @Override
    public void delete( final String email ) {
        entityManager.remove( find( email ) );
    }
}</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">Please notice, although we are performing data manipulations, there is no mention of transactions as well as there are no explicit calls to entity manager&#8217;s transactions API. We are going to use the declarative approach to transactions as </span><a style="color: #888855;" href="http://aries.apache.org/modules/blueprint.html">blueprint</a><span style="color: #333333;"> configuration supports that (the location is unchanged, </span><b style="color: #333333;">OSGI-INF/blueprint</b><span style="color: #333333;"> folder):</span>
</p>

<pre class="lang:default decode:true ">&lt;blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"  
    xmlns:jpa="http://aries.apache.org/xmlns/jpa/v1.1.0"
    xmlns:tx="http://aries.apache.org/xmlns/transactions/v1.0.0" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"         
    xsi:schemaLocation="
        http://www.osgi.org/xmlns/blueprint/v1.0.0 
        http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd"&gt;
    
    &lt;service ref="peopleDao" interface="com.example.data.PeopleDao" /&gt;
    &lt;bean id="peopleDao" class="com.example.data.impl.PeopleDaoImpl"&gt;
     &lt;jpa:context unitname="peopleDb" property="entityManager" /&gt;
     &lt;tx:transaction method="*" value="Required"/&gt;
    &lt;/bean&gt;
    
    &lt;bean id="dataSource" class="org.hsqldb.jdbc.JDBCDataSource"&gt;
       &lt;property name="url" value="jdbc:hsqldb:mem:peopleDb"/&gt;
    &lt;/bean&gt;
    
    &lt;service ref="dataSource" interface="javax.sql.DataSource"&gt; 
        &lt;service-properties&gt; 
            &lt;entry key="osgi.jndi.service.name" value="peopleDb" /&gt; 
        &lt;/service-properties&gt; 
    &lt;/service&gt;     
&lt;/blueprint&gt;</pre>

<p style="text-align: justify;">
  One thing to keep in mind: the application doesn&#8217;t need to create <a style="color: #888855;" href="https://jcp.org/en/jsr/detail?id=338">JPA 2.1</a>&#8216;s entity manager: the <a style="color: #888855;" href="http://www.osgi.org/Main/HomePage">OSGi</a> runtime is able do that and inject it everywhere it is required, driven by <b>jpa:context</b> declarations. Consequently, <b>tx:transaction</b> instructs the runtime to wrap the selected service methods inside transaction.
</p>

Now, when the last service **PeopleDao** is exposed, we are ready to deploy our modules with <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a>. It is quite easy to do in three steps:

  * run the <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> container 
    <pre>bin/karaf (or bin\karaf.bat on Windows)
</pre>

  * execute following commands from the <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> shell: 
    <pre>feature:repo-add cxf 3.0.1
feature:install http cxf jpa openjpa transaction jndi jdbc
install -s mvn:org.hsqldb/hsqldb/2.3.2
install -s mvn:com.fasterxml.jackson.core/jackson-core/2.4.0
install -s mvn:com.fasterxml.jackson.core/jackson-annotations/2.4.0
install -s mvn:com.fasterxml.jackson.core/jackson-databind/2.4.0
install -s mvn:com.fasterxml.jackson.jaxrs/jackson-jaxrs-base/2.4.0
install -s mvn:com.fasterxml.jackson.jaxrs/jackson-jaxrs-json-provider/2.4.0
</pre>

  * build our modules and copy them into <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a>&#8216;s deploy folder (while container is still running): 
    <pre>mvn clean package
cp module*/target/*jar apache-karaf-3.0.1/deploy/
</pre>

When you run the **list** command in the <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> shell, you should see the list of all activated bundles (modules), similar to this one:

<div class="separator">
  <a style="color: #888855;" href="http://3.bp.blogspot.com/-t3N__LFQGsI/U-Y7SdfzE8I/AAAAAAAAALA/uxzVwhtq_fs/s1600/apache-karaf-list.PNG"><img src="http://3.bp.blogspot.com/-t3N__LFQGsI/U-Y7SdfzE8I/AAAAAAAAALA/uxzVwhtq_fs/s400/apache-karaf-list.PNG" alt="" border="0" /></a>
</div>

&nbsp;

<p style="text-align: justify;">
  Where <b>module-service</b>, <b>module-jax-rs</b> and <b>module-data</b> correspond to the ones we are being developed. By default, all our <a style="color: #888855;" href="http://cxf.apache.org/">Apache CXF 3.0.1</a> services will be available at base URL <b>http://:8181/cxf/api/</b>. It is easy to check by executing <b>cxf:list-endpoints -f</b> command in the <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> shell.
</p>

<div class="separator">
  <a style="color: #888855;" href="http://3.bp.blogspot.com/-ylReVFqcKCQ/U-Y8toUPOEI/AAAAAAAAALI/ebrK3k8yQ1g/s1600/apache-karaf-cxf-list-endpoints.PNG"><img src="http://3.bp.blogspot.com/-ylReVFqcKCQ/U-Y8toUPOEI/AAAAAAAAALI/ebrK3k8yQ1g/s400/apache-karaf-cxf-list-endpoints.PNG" alt="" border="0" /></a>
</div>

&nbsp;

Let us make sure our <a style="color: #888855;" href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a> layer works as expected by sending couple of <a style="color: #888855;" href="http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol">HTTP</a> requests. Let us create new person:

<pre>curl http://localhost:8181/cxf/api/people -iX POST -d "firstName=Tom&lastName=Knocker&email=a@b.com"

HTTP/1.1 201 Created
Content-Length: 0
Date: Sat, 09 Aug 2014 15:26:17 GMT
Location: http://localhost:8181/cxf/api/people/a@b.com
Server: Jetty(8.1.14.v20131031)
</pre>

And verify that person has been created successfully:

<pre>curl -i http://localhost:8181/cxf/api/people

HTTP/1.1 200 OK
Content-Type: application/json
Date: Sat, 09 Aug 2014 15:28:20 GMT
Transfer-Encoding: chunked
Server: Jetty(8.1.14.v20131031)

[{"email":"a@b.com","firstName":"Tom","lastName":"Knocker"}]
</pre>

Would be nice to check if database has the person populated as well. With <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> shell it is very simple to do by executing just two commands: **jdbc:datasources** and **jdbc:query peopleDb &#8220;select * from people&#8221;**.

<div class="separator">
  <a style="color: #888855;" href="http://1.bp.blogspot.com/-eflcSSwKdMY/U-Y_K1i6poI/AAAAAAAAALU/d2WVtAWXSH4/s1600/apache-karaf-list-datasources.PNG"><img src="http://1.bp.blogspot.com/-eflcSSwKdMY/U-Y_K1i6poI/AAAAAAAAALU/d2WVtAWXSH4/s400/apache-karaf-list-datasources.PNG" alt="" border="0" /></a>
</div>

&nbsp;

<p style="text-align: justify;">
  Awesome! I hope this quite introductory blog post opens yet another piece of interesting technology you may use for developing robust, scalable, modular and manageable software. We have not touched many, many things but these are here for you to discover. The complete source code is available on <a style="color: #888855;" href="https://github.com/reta/osgi-services">GitHub</a>.
</p>

<p style="text-align: justify;">
  Note to <b>Hibernate 4.2.x / 4.3.x</b> users: unfortunately, in the current release of <a style="color: #888855;" href="http://karaf.apache.org/">Apache Karaf 3.0.1</a> the<b>Hibernate 4.3.x</b> does work properly at all (as <a style="color: #888855;" href="https://jcp.org/en/jsr/detail?id=338">JPA 2.1</a> is not yet supported) and, however I have managed to run with <b>Hibernate 4.2.x</b>, the container often refused to resolve the JPA-related dependencies.
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with the permission of Andriy RedkoAndriy Redko</a> – source <a title="http://aredko.blogspot.ro/2014/08/osgi-gateway-into-micro-services.html" href="http://aredko.blogspot.ro/2014/08/osgi-gateway-into-micro-services.html" target="_blank">OSGi: the gateway into micro-services architecture</a> from <a title="http://aredko.blogspot.com" href="http://aredko.blogspot.com" target="_blank">http://aredko.blogspot.com</a>
</p>

<p style="text-align: justify;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="http://1.bp.blogspot.com/_WNHv4iYKMe0/S2Rnco10R2I/AAAAAAAAAAc/eTh_Rkk8V_w/S220/photo.jpg" alt="Andriy Redko" /> 
    
    <p id="about_author_header">
      Andriy Redko {devmind}
    </p>
    
    <div id="author_details" style="text-align: justify;">
      15+ years of software development experience as Programmer/Software Developer/Senior Software Developer/Team Lead/Consultant. I am extensively working with Java EE, Microsoft .NET and Adobe Flex platforms using Java, C#, C++, Groovy, Scala, Ruby, Action Script, Grails, MySQL, PostreSQL, MongoDB, Redis, ...
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://aredko.blogspot.com" target="_blank"> </a> <a class="icon-github" href="https://github.com/reta" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>
