---
id: 1079
title: RESTful Web Services Example in Java with Jersey, Spring and MyBatis
date: 2014-01-11T16:48:00+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1079
permalink: /ama/restful-web-services-example-in-java-with-jersey-spring-and-mybatis/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 28
dsq_thread_id:
  - 2108766174
fsb_social_google:
  - 6
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:68:"https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis";s:11:"ribbon-type";i:10;}'
categories:
  - java
  - spring
tags:
  - demo
  - eclipse
  - example
  - failsafe-plugin
  - integration tests
  - jersey
  - jersey2
  - jetty
  - jetty-maven-plugin
  - maven
  - maven-failsafe-plugin
  - MyBatis
  - rest
  - spring3
  - web services
---
<p class="note_alert" style="text-align: justify;">
  <strong><span style="font-size: 1.2em;">Note:</span> At the time of writing this post I was just starting with REST and Jersey so I  suggest you have a look at  <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank"><span style="font-size: 1.2em;">Tutorial – REST API design and implementation in Java with Jersey and Spring </span> </a>instead. My gained REST knowledge will be from now on reflected in this post, which is already an &#8220;(r)evolution&#8221; regarding REST API design, REST best practices used and backend architecture/implementation supporting the REST API presented in the tutorial.</strong>
</p>

<p style="text-align: justify;">
  Looking to REST? In Java? There&#8217;s never time for that :), but if you are looking to use an <em>&#8220;architectural style consisting of a coordinated set of constraints applied to components, connectors, and data elements, within a distributed <a title="Hypermedia" href="http://en.wikipedia.org/wiki/Hypermedia">hypermedia</a> system&#8221;</em> in Java<em>,</em> then you have come to the right place, because in this post I will present a simple RESTful API that maps REST calls to backend services offering <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD</a> functionality.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> I will not focus too much on <a title="Wikipedia - REST" href="http://en.wikipedia.org/wiki/Representational_State_Transfer" target="_blank"><b>Representational state transfer</b></a> (<b>REST</b>) itself, because there are plenty of resources on the topic in the internet, some of which I listed under Resources at the end of the post.
</p>
<!--more-->
<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#1_The_example">1. The example</a><ul>
          <li>
            <a href="#11_Why">1.1. Why?</a>
          </li>
          <li>
            <a href="#12_What_does_it_do">1.2. What does it do?</a>
          </li>
          <li>
            <a href="#13_Architecture_and_technologies">1.3. Architecture and technologies</a><ul>
              <li>
                <a href="#131_Jersey">1.3.1. Jersey</a>
              </li>
              <li>
                <a href="#132_Spring">1.3.2. Spring</a>
              </li>
              <li>
                <a href="#133_MyBatis">1.3.3. MyBatis</a>
              </li>
              <li>
                <a href="#134_Web_Container">1.3.4. Web Container</a>
              </li>
              <li>
                <a href="#135_MySQL">1.3.5. MySQL</a>
              </li>
              <li>
                <a href="#136_Technologies">1.3.6. Technologies</a>
              </li>
              <li>
                <a href="#137_Follow_along">1.3.7. Follow along</a>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#2_The_coding">2. The coding</a><ul>
          <li>
            <a href="#21_Configuration">2.1. Configuration</a><ul>
              <li>
                <a href="#211_Project_dependencies">2.1.1. Project dependencies</a>
              </li>
              <li>
                <a href="#212_Web_Application_Deployment_Descriptor_8211_webxml">2.1.2. Web Application Deployment Descriptor &#8211; web.xml</a><ul>
                  <li>
                    <a href="#2121_Jersey-servlet">2.1.2.1. Jersey-servlet</a>
                  </li>
                  <li>
                    <a href="#2122_Spring_application_context_configuration">2.1.2.2. Spring application context configuration</a>
                  </li>
                </ul>
              </li>
            </ul>
          </li>

          <li>
            <a href="#22_The_RESTful_API">2.2. The RESTful API</a><ul>
              <li>
                <a href="#221_Resources">2.2.1. Resources</a>
              </li>
              <li>
                <a href="#222_Methods">2.2.2. Methods</a><ul>
                  <li>
                    <a href="#2221_CREATE">2.2.2.1. CREATE</a><ul>
                      <li>
                        <a href="#22211_Create_a_single_resource_8220podcast8221_from_JSON_input">2.2.2.1.1. Create a single resource (&#8220;podcast&#8221;) from JSON input</a>
                      </li>
                      <li>
                        <a href="#22212_Create_multiple_resources_8220podcasts8221_from_JSON_input">2.2.2.1.2. Create multiple resources (&#8220;podcasts&#8221;) from JSON input</a>
                      </li>
                      <li>
                        <a href="#22213_Create_a_single_resource_8220podcast8221_from_form">2.2.2.1.3. Create a single resource (&#8220;podcast&#8221;) from form</a>
                      </li>
                    </ul>
                  </li>

                  <li>
                    <a href="#2222_READ">2.2.2.2. READ</a><ul>
                      <li>
                        <a href="#22221_Read_all_resources">2.2.2.2.1. Read all resources</a>
                      </li>
                      <li>
                        <a href="#22222_Read_one_resource">2.2.2.2.2. Read one resource</a>
                      </li>
                    </ul>
                  </li>

                  <li>
                    <a href="#2223_UPDATE">2.2.2.3. UPDATE</a>
                  </li>
                  <li>
                    <a href="#2224_DELETE">2.2.2.4. DELETE</a><ul>
                      <li>
                        <a href="#22241_Delete_all_resources">2.2.2.4.1. Delete all resources</a>
                      </li>
                      <li>
                        <a href="#22242_Delete_one_resource">2.2.2.4.2. Delete one resource</a>
                      </li>
                    </ul>
                  </li>
                </ul>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#3_Testing">3. Testing</a><ul>
          <li>
            <a href="#31_Integration_tests">3.1. Integration tests</a><ul>
              <li>
                <a href="#311_Configuration">3.1.1. Configuration</a><ul>
                  <li>
                    <a href="#3111_Jersey_client_dependency">3.1.1.1 Jersey client dependency</a>
                  </li>
                  <li>
                    <a href="#3112_Failsafe_plugin">3.1.1.2. Failsafe plugin</a>
                  </li>
                  <li>
                    <a href="#3112_Jetty_Maven_Plugin">3.1.1.2. Jetty Maven Plugin</a>
                  </li>
                </ul>
              </li>

              <li>
                <a href="#312_Build_the_integration_tests">3.1.2. Build the integration tests</a>
              </li>
              <li>
                <a href="#313_Running_the_integration_tests">3.1.3. Running the integration tests</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#32_Live_tests">3.2. Live tests</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#4_Summary">4. Summary</a>
      </li>
      <li>
        <a href="#5_Resources">5. Resources</a><ul>
          <li>
            <a href="#51_Source_Code">5.1. Source Code</a>
          </li>
          <li>
            <a href="#52_Web_resources">5.2. Web resources</a>
          </li>
          <li>
            <a href="#53_Codingpedia_resources">5.3. Codingpedia resources</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</p>

<h2 style="text-align: justify;">
  <span id="1_The_example">1. The example</span>
</h2>

<h3 style="text-align: justify;">
  <span id="11_Why">1.1. Why?</span>
</h3>

<p style="text-align: justify;">
  My intention is to move some of the parts from <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, that are currently implemented in Spring MVC, to JavaScript-land and for that I&#8217;ll need to use more backend REST web services. Of course I could use <a title="Designing and Implementing RESTful Web Services with Spring" href="https://spring.io/guides/tutorials/rest/" target="_blank">Spring&#8217;s own REST implementation</a>, as I currently do for the AJAX calls, but I wanted also to see how the &#8220;official&#8221; implementation looks like.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> You can visit my post <a title="Autocomplete search box with jQuery and Spring MVC" href="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" target="_blank">Autocomplete search box with jQuery and Spring MVC</a> to see how Spring handles REST requests.
</p>

<h3 style="text-align: justify;">
  <span id="12_What_does_it_do">1.2. What does it do?</span>
</h3>

<p style="text-align: justify;">
  So, the best way to get to know the technology is build a prototype with it. And that&#8217;s exactly what I did and what I will present in this post. I&#8217;ve build a simple application that &#8220;manages&#8221; podcasts via a REST API. It does CRUD operations on a single database table (Podcasts), triggered via the REST web services API. Though fairly simple, the example highlights the most common annotations you&#8217;ll need to build your own REST API.
</p>

<h3 style="text-align: justify;">
  <span id="13_Architecture_and_technologies">1.3. Architecture and technologies</span>
</h3>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/01/Architecture.png"><img class="alignnone size-medium wp-image-1080" src="{{site.url}}/wp-content/uploads/2014/01/Architecture-300x140.png" alt="Demo architecture" width="300" height="140" srcset="{{site.url}}/wp-content/uploads/2014/01/Architecture-300x140.png 300w, {{site.url}}/wp-content/uploads/2014/01/Architecture.png 602w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

<h4 style="text-align: justify;">
  <span id="131_Jersey">1.3.1. Jersey</span>
</h4>

<p style="text-align: justify;">
  The architecture is straightforward: with any REST client you can call the application&#8217;s API exposed via <a title="Jersey - RESTful Web Services in Java" href="https://jersey.java.net/" target="_blank"><strong>Jersey</strong> RESTful Web Services in JAVA</a>. The <em>Jersey RESTful Web Services framework</em> is open source, production quality, framework for developing RESTful Web Services in Java that provides support for JAX-RS APIs and serves as a <a title="Java API for RESTful Services (JAX-RS)" href="https://jax-rs-spec.java.net/" target="_blank">JAX-RS</a> (JSR 311 & JSR 339) Reference Implementation.
</p>

<h4 style="text-align: justify;">
  <span id="132_Spring">1.3.2. Spring</span>
</h4>

<p style="text-align: justify;">
  I like glueing stuff together with <strong><a title="Spring Framework" href="http://projects.spring.io/spring-framework/" target="_blank">Spring</a></strong>, and this example is no exception. You&#8217;ll find out how Jersey 2 integrates with Spring.
</p>

<h4 style="text-align: justify;">
  <span id="133_MyBatis">1.3.3. MyBatis</span>
</h4>

<p style="text-align: justify;">
  For the persistence layer, I chose Mybatis because I like it the most and it integrates fairly easy with Spring (see my post <a title="Spring MyBatis integration example" href="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" target="_blank">Spring MyBatis integration example</a> for more on that), but you are free to choose any framework and technology you&#8217;re familiar with (e.g. JPA/Hibernate, Hibernate, JDBC etc).
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> If you want to see how the persistence layer is implemented for the same application with JPA2 and Hibernate 4 see the post <a title="Java Persistence Example with Spring, JPA2 and Hibernate" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate</a>.
</p>

<h4 style="text-align: justify;">
  <span id="134_Web_Container">1.3.4. Web Container</span>
</h4>

<p style="text-align: justify;">
  Everything gets packaged as a <code>.war</code> file and can be deployed on any web container &#8211; I used <a title="Apache Tomcat" href="http://tomcat.apache.org/" target="_blank">Tomcat</a> and <a title="Jetty" href="http://www.eclipse.org/jetty/" target="_blank">Jetty</a> but, it could also be Glassfih, Weblogic, JBoss or WebSphere.
</p>

<h4 style="text-align: justify;">
  <span id="135_MySQL">1.3.5. MySQL</span>
</h4>

<p style="text-align: justify;">
  The sample data is stored in a <a title="MySQL" href="http://www.mysql.com/" target="_blank">MySQL</a> table:
</p>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/01/database-schema.png"><img class="alignnone size-medium wp-image-1089" src="{{site.url}}/wp-content/uploads/2014/01/database-schema-300x280.png" alt="database schema" width="300" height="280" srcset="{{site.url}}/wp-content/uploads/2014/01/database-schema-300x280.png 300w, {{site.url}}/wp-content/uploads/2014/01/database-schema.png 566w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The main focus in the post will be on the Jersey JAX-RS implementation, all the other technologies are viewed as enablers.
</p>

#### <span id="136_Technologies">1.3.6. Technologies</span>

  1. Jersey 2.4
  2. Spring 3.2
  3. Maven 3
  4. Tomcat 7
  5. Jetty 9
  6. MySql 5.6

#### <span id="137_Follow_along">1.3.7. Follow along</span>

If you want to follow along, you find all you need on GitHub:

  * <a title="Source code" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis (source code of the project)</a>
  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/resources/input_data/DumpRESTdemoDB.sql" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/resources/input_data/DumpRESTdemoDB.sql" target="_blank">MySQL DB creation self-contained file</a> &#8211; everything is configured for the `username/password` : `rest_demo/rest_demo`

## <span id="2_The_coding">2. The coding</span>

### <span id="21_Configuration">2.1. Configuration</span>

#### <span id="211_Project_dependencies">2.1.1. Project dependencies</span>

<p style="text-align: justify;">
  Among other things you need to have Jersey Spring extension in your project&#8217;s classpath. You can easily add that with Maven by having the following dependencies to the <code>pom.xml</code> file of the project:
</p>

<pre><code class="xml">&lt;dependency&gt;
	&lt;groupId&gt;org.glassfish.jersey.ext&lt;/groupId&gt;
	&lt;artifactId&gt;jersey-spring3&lt;/artifactId&gt;
	&lt;version&gt;2.4.1&lt;/version&gt;
	&lt;exclusions&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-core&lt;/artifactId&gt;
		&lt;/exclusion&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-web&lt;/artifactId&gt;
		&lt;/exclusion&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-beans&lt;/artifactId&gt;
		&lt;/exclusion&gt;
	&lt;/exclusions&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.glassfish.jersey.media&lt;/groupId&gt;
	&lt;artifactId&gt;jersey-media-json-jackson&lt;/artifactId&gt;
	&lt;version&gt;2.4.1&lt;/version&gt;
&lt;/dependency&gt;</code></pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The jersey-spring3.jar, uses its own version for Spring libraries, so to use the ones you want, you need to exclude these libraries manually.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> If you want to see what other dependencies are used (for Spring, Jetty, testing) in the project or how Jetty is configured so that you can start the project directly in Jetty, you can download the complete <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" target="_blank">pom.xml</a> file from GitHub &#8211; <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml</a>
</p>

#### <span id="212_Web_Application_Deployment_Descriptor_8211_webxml">2.1.2. Web Application Deployment Descriptor &#8211; web.xml</span>

<pre><code class="xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"&gt;
	&lt;display-name&gt;Demo - Restful Web Application&lt;/display-name&gt;

	&lt;listener&gt;
		&lt;listener-class&gt;
			org.springframework.web.context.ContextLoaderListener
		&lt;/listener-class&gt;
	&lt;/listener&gt;

	&lt;context-param&gt;
		&lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
		&lt;param-value&gt;classpath:spring/applicationContext.xml&lt;/param-value&gt;
	&lt;/context-param&gt;

	&lt;servlet&gt;
		&lt;servlet-name&gt;jersey-serlvet&lt;/servlet-name&gt;
		&lt;servlet-class&gt;
			org.glassfish.jersey.servlet.ServletContainer
		&lt;/servlet-class&gt;
		&lt;init-param&gt;
			&lt;param-name&gt;javax.ws.rs.Application&lt;/param-name&gt;
			&lt;param-value&gt;org.codingpedia.demo.rest.service.MyApplication&lt;/param-value&gt;
		&lt;/init-param&gt;
		&lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
	&lt;/servlet&gt;

	&lt;servlet-mapping&gt;
		&lt;servlet-name&gt;jersey-serlvet&lt;/servlet-name&gt;
		&lt;url-pattern&gt;/*&lt;/url-pattern&gt;
	&lt;/servlet-mapping&gt;

	&lt;resource-ref&gt;
        &lt;description&gt;Database resource rest demo web application &lt;/description&gt;
        &lt;res-ref-name&gt;jdbc/restDemoDB&lt;/res-ref-name&gt;
        &lt;res-type&gt;javax.sql.DataSource&lt;/res-type&gt;
        &lt;res-auth&gt;Container&lt;/res-auth&gt;
    &lt;/resource-ref&gt;
&lt;/web-app&gt;</code></pre>

##### <span id="2121_Jersey-servlet">2.1.2.1. Jersey-servlet</span>

<p style="text-align: justify;">
  Notice the Jersey servlet configuration [lines 18-33]. The <code>javax.ws.rs.core.Application</code> class defines the components of the JAX-RS application. Because I extended the <code>Application (ResourceConfig)</code> class to provide the list of relevant root resource classes (<code class="literal">getResources()</code>) and singletons (<code class="literal">getSingletons()</code>), i.e. the JAX-RS application model, I needed to register it in my web application <code class="literal">web.xml</code> deployment descriptor using a Servlet or Servlet filter initialization parameter with a name of <code class="literal">javax.ws.rs.Application.</code>Check out the <a title="https://jersey.java.net/documentation/latest/deployment.html" href="https://jersey.java.net/documentation/latest/deployment.html" target="_blank">documentation</a> for other possibilities.
</p>

<p style="text-align: justify;">
  The implementation of <code>org.codingpedia.demo.rest.service.MyApplication</code> looks like the following in the project:
</p>

<pre><code class="java">package org.codingpedia.demo.rest.service;

import org.glassfish.jersey.jackson.JacksonFeature;
import org.glassfish.jersey.server.ResourceConfig;
import org.glassfish.jersey.server.spring.scope.RequestContextFilter;

/**
 * Registers the components to be used by the JAX-RS application
 *
 * @author ama
 *
 */
public class MyDemoApplication extends ResourceConfig {

    /**
	* Register JAX-RS application components.
	*/
	public MyDemoApplication(){
		register(RequestContextFilter.class);
		register(PodcastRestService.class);
		register(JacksonFeature.class);
	}
}</code></pre>

The class registers the following components

<li style="text-align: justify;">
  <code>org.glassfish.jersey.server.spring.scope.RequestContextFilter</code>, which is a Spring filter that provides a bridge between JAX-RS and Spring request attributes
</li>
<li style="text-align: justify;">
  <code>org.codingpedia.demo.rest.service.PodcastRestService</code>, which is the service component that exposes the REST API via annotations, will be presented later in detail
</li>
<li style="text-align: justify;">
  <code>org.glassfish.jersey.jackson.JacksonFeature</code>, which is a feature that registers Jackson JSON providers &#8211; you need it for the application to understand JSON data
</li>

##### <span id="2122_Spring_application_context_configuration">2.1.2.2. Spring application context configuration</span>

The Spring application context configuration is located in the classpath under `spring/applicationContext.xml`:

<pre><code class="xml">&lt;beans xmlns="http://www.springframework.org/schema/beans"
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

	&lt;!--
		Instruct Spring to perform declarative transaction management
		automatically on annotated classes.
	--&gt;
	&lt;tx:annotation-driven transaction-manager="transactionManager" /&gt;
    &lt;bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager"&gt;
        &lt;property name="dataSource" ref="dataSource"/&gt;
    &lt;/bean&gt;

	&lt;!-- =============== MyBATIS beans configuration ================== --&gt;
	&lt;bean id="podcastDao" class="org.mybatis.spring.mapper.MapperFactoryBean"&gt;
	   &lt;property name="sqlSessionFactory" ref="sqlSessionFactory"/&gt;
	   &lt;property name="mapperInterface" value="org.codingpedia.demo.rest.dao.PodcastDao" /&gt;
	&lt;/bean&gt;

	&lt;bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"&gt;
	    &lt;property name="dataSource" ref="dataSource" /&gt;
	    &lt;property name="configLocation" value="classpath:config/mybatisV3.xml"/&gt;
	&lt;/bean&gt;

    &lt;bean id="podcastRestService" class="org.codingpedia.demo.rest.service.PodcastRestService" /&gt;

	&lt;bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoDB" /&gt;
	    &lt;property name="resourceRef" value="true" /&gt;
	&lt;/bean&gt;
&lt;/beans&gt;</code></pre>

<p style="text-align: justify;">
  Nothing special here, it just defines the beans that are needed throughout the demo application. The most important one is the <code>podcastRestService</code> which is actually the entry point class for our RESTful API, and will be thouroughly described in the next paragraphs.
</p>

### <span id="22_The_RESTful_API">2.2. The RESTful API</span>

#### <span id="221_Resources">2.2.1. Resources</span>

<p style="text-align: justify;">
  As mentioned earlier, the demo application manages podcasts, which represent the <a title="Wikipedia - Web resource" href="http://en.wikipedia.org/wiki/Web_resource" target="_blank">resources</a> in our web API. Resources are the central concept in REST and are characterized by two main things:
</p>

  * each is referenced with a global identifier (e.g. a [URI](http://en.wikipedia.org/wiki/Uniform_resource_identifier "Uniform resource identifier") in HTTP).
  * has one or more representations, that they expose to the outer world and can be manipulated with (we&#8217;ll be working mostly with JSON representations in this example)

The podcast resources are represented in our application by the <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/entities/Podcast.java" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/entities/Podcast.java" target="_blank">Podcast</a> class:

<pre><code class="java">package org.codingpedia.demo.rest.entities;

import java.io.Serializable;
import java.util.Date;

import javax.xml.bind.annotation.XmlRootElement;

/**
 * Podcast entity
 *
 * @author ama
 *
 */
@XmlRootElement
public class Podcast implements Serializable {

	private static final long serialVersionUID = -8039686696076337053L;

	/** id of the podcas */
	private Long id;

	/** title of the podcast */
	private String title;

	/** link of the podcast on Podcastpedia.org */
	private String linkOnPodcastpedia;

	/** url of the feed */
	private String feed;

	/** description of the podcast */
	private String description;

	/** when an episode was last published on the feed*/
	private Date insertionDate;

	public Podcast(){}

	public Podcast(String title, String linkOnPodcastpedia, String feed,
			String description) {

		this.title = title;
		this.linkOnPodcastpedia = linkOnPodcastpedia;
		this.feed = feed;
		this.description = description;

	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getLinkOnPodcastpedia() {
		return linkOnPodcastpedia;
	}

	public void setLinkOnPodcastpedia(String linkOnPodcastpedia) {
		this.linkOnPodcastpedia = linkOnPodcastpedia;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFeed() {
		return feed;
	}

	public void setFeed(String feed) {
		this.feed = feed;
	}

	public Date getInsertionDate() {
		return insertionDate;
	}

	public void setInsertionDate(Date insertionDate) {
		this.insertionDate = insertionDate;
	}

}</code></pre>

The strucuture is pretty simple &#8211; there are an `id`, which identifies a podcast, and several other fields that we&#8217;ll can see in the JSON representation:

<pre><code class="json">{
	"id":1,
	"title":"Quarks & Co - zum Mitnehmen-modified",
	"linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen",
	"feed":"http://podcast.wdr.de/quarks.xml",
	"description":"Quarks & Co: Das Wissenschaftsmagazin",
	"insertionDate":1388213547000
}</code></pre>

#### <span id="222_Methods">2.2.2. Methods</span>

The API exposed by our example is described in the following table:

<table style="width: 100%;" border="1" cellspacing="0" cellpadding="5">
  <colgroup> <col width="85*" /> <col width="85*" /> <col width="85*" /> </colgroup> <tr valign="TOP">
    <td bgcolor="#808080" width="33%">
      <b> Resource</b>
    </td>

    <td bgcolor="#808080" width="33%">
      <b> URI</b>
    </td>

    <td bgcolor="#808080" width="33%">
      <b> Method</b>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="19">
      <p align="CENTER">
        <b>CREATE</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="19">
      Add a list podcasts
    </td>

    <td width="33%">
      /podcasts/list
    </td>

    <td width="33%">
      <p align="CENTER">
        POST
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="19">
      Add a new podcast
    </td>

    <td width="33%">
      /podcasts/
    </td>

    <td width="33%">
      <p align="CENTER">
        POST
      </p>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="16">
      <p align="CENTER">
        <b>READ</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      List of all podcasts
    </td>

    <td width="33%">
      /podcasts/
    </td>

    <td width="33%">
      <p align="CENTER">
        GET
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      List a single podcast
    </td>

    <td width="33%">
      /podcasts/{id}
    </td>

    <td width="33%">
      <p align="CENTER">
        GET
      </p>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="16">
      <p align="CENTER">
        <b>UPDATE</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      Updates a single podcasts or creates one if not existent
    </td>

    <td width="33%">
      /podcasts/{id}
    </td>

    <td width="33%">
      <p align="CENTER">
        PUT
      </p>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="16">
      <p align="CENTER">
        <b>DELETE</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      Delete all podcasts
    </td>

    <td width="33%">
      /podcasts/
    </td>

    <td width="33%">
      <p align="CENTER">
        DELETE
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="15">
      Delete a single podcast
    </td>

    <td width="33%">
      /podcasts/{id}
    </td>

    <td width="33%">
      <p align="CENTER">
        DELETE
      </p>
    </td>
  </tr>
</table>

As already mentioned the `PodcastRestService` class is the one handling all the rest requests:

<pre><code class="java">package org.codingpedia.demo.rest.service;
......................
@Component
@Path("/podcasts")
public class PodcastRestService {

	private static final String HOST = "http://localhost:8080/demo-rest-spring-jersey-tomcat-mybatis-0.0.1-SNAPSHOT";

	@Autowired
	private PodcastDao podcastDao;
	......................
}</code></pre>

<p style="text-align: justify;">
  Notice the <code>@Path("/podcasts")</code> before the class definition. The <a href="http://jax-rs-spec.java.net/nonav/2.0/apidocs/javax/ws/rs/Path.html" target="_top">@Path</a> annotation&#8217;s value is a relative URI path. In the example above, the Java class will be hosted at the URI path <code>/podcasts</code>. The <code>PodcastDao</code> interface is used to communicate with the database.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> You can find the entire content of the class on GitHub &#8211; <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java</a>. We&#8217;ll be going through the file step by step and explain the different methods corresponding to the different operations.
</p>

##### <span id="2221_CREATE">2.2.2.1. CREATE</span>

For the creation of new resources(&#8220;podcasts&#8221;) I use the <a title="Wikpedia - POST (HTTP)" href="http://en.wikipedia.org/wiki/POST_%28HTTP%29" target="_blank">POST (HTTP)</a> method.

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> In JAX-RS (Jersey) you specifies the HTTP methods (GET, POST, PUT, DELETE) by placing the corresponding annotation in front of the method.
</p>

###### <span id="22211_Create_a_single_resource_8220podcast8221_from_JSON_input">2.2.2.1.1. Create a single resource (&#8220;podcast&#8221;) from JSON input</span>

<pre><code class="java">/**
 * Adds a new resource (podcast) from the given json format (at least title and feed elements are required
 * at the DB level)
 *
 * @param podcast
 * @return
 */
@POST
@Consumes({MediaType.APPLICATION_JSON})
@Produces({MediaType.TEXT_HTML})
@Transactional
public Response createPodcast(Podcast podcast) {
	Long id = podcastDao.createPodcast(podcast);

	return Response.status(201).entity(buildNewPodcastResourceURL(id)).build();
}</code></pre>

Annotations

  * `<code>@POST`</code> &#8211; indicates that the method responds to HTTP POST requests
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
<li style="text-align: justify;">
  <code>@Produces({MediaType.TEXT_HTML})</code> &#8211; defines the media type) that the method can produce, in this case <code>"text/html"</code>. The response will be a html document, with a status of 201, indicating to the caller that the request has been fulfilled and resulted in a new resource being created.
</li>
<li style="text-align: justify;">
  <code>@Transactional</code> &#8211; Spring annotation, specifies that the method execution, should take place inside a transaction
</li>

###### <span id="22212_Create_multiple_resources_8220podcasts8221_from_JSON_input">2.2.2.1.2. Create multiple resources (&#8220;podcasts&#8221;) from JSON input</span>

<pre><code class="java">/**
	 * A list of resources (here podcasts) provided in json format will be added
	 * to the database.
	 *
	 * @param podcasts
	 * @return
	 */
	@POST @Path("list")
	@Consumes({MediaType.APPLICATION_JSON})
	@Transactional
	public Response createPodcasts(List&lt;Podcast&gt; podcasts) {
		for(Podcast podcast : podcasts){
			podcastDao.createPodcast(podcast);
		}

		return Response.status(204).build();
	}</code></pre>

Annotations

  * `@POST` &#8211; indicates that the method responds to HTTP POST requests
<li style="text-align: justify;">
  <code>@Path("/list")</code> &#8211; identifies the URI path that the class method will serve requests for. Paths are relative. The combined path here will be <code>"/podcasts/list"</code>, because as we have seen we have <code>@Path</code> annotation at the class level
</li>
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
<li style="text-align: justify;">
  <code>@Transactional</code>&#8211; Spring annotation, specifies that the method execution, should take place inside a transaction
</li>

<p style="text-align: justify;">
  In this case the method returns a status of 204 (&#8220;No Content&#8221;), suggesting that the server has fulfilled the request but does not need to return an entity-body, and might want to return updated metainformation.
</p>

###### <span id="22213_Create_a_single_resource_8220podcast8221_from_form">2.2.2.1.3. Create a single resource (&#8220;podcast&#8221;) from form</span>

<pre><code class="java">/**
 * Adds a new resource (podcast) from "form" (at least title and feed elements are required
 * at the DB level)
 *
 * @param title
 * @param linkOnPodcastpedia
 * @param feed
 * @param description
 * @return
 */
@POST
@Consumes(MediaType.APPLICATION_FORM_URLENCODED)
@Produces({MediaType.TEXT_HTML})
@Transactional
public Response createPodcastFromForm(
					@FormParam("title") String title,
					@FormParam("linkOnPodcastpedia") String linkOnPodcastpedia,
					@FormParam("feed") String feed,
					@FormParam("description") String description
					) {
	Podcast podcast = new Podcast(title, linkOnPodcastpedia, feed, description);
	Long id = podcastDao.createPodcast(podcast);

	return Response.status(201).entity(buildNewPodcastResourceURL(id)).build();
}</code></pre>

Annotations

  * `@POST` &#8211; indicates that the method responds to HTTP POST requests
<li style="text-align: justify;">
  <code>@Consumes({MediaType.APPLICATION_FORM_URLENCODED})</code> <ul>
    <ul>
      &#8211; defines the media type, the method accepts, in this case
    </ul>
  </ul>

  <p>
    <code>"application/x-www-form-urlencoded"</code>
  </p>

  <ul>
    <li style="text-align: justify;">
      @FormParam &#8211; present before the input parameters of the method, this annotation binds the value(s) of a form parameter contained within a request entity body to a resource method parameter. Values are URL decoded unless this is disabled using the <code>Encoded</code> annotation
    </li>
  </ul>
</li>

<li style="text-align: justify;">
  <code>&lt;code>@Produces({MediaType.TEXT_HTML})</code> - defines the media type) that the method can produce, in this case "text/html". The response will be a html document, with a status of 201, indicating to the caller that the request has been fulfilled and resulted in a new resource being created.</code>
</li>
  * `@Transactional` &#8211; Spring annotation, specifies that the method execution, should take place inside a transaction

##### <span id="2222_READ">2.2.2.2. READ</span>

###### <span id="22221_Read_all_resources">2.2.2.2.1. Read all resources</span>

<pre><code class="java">/**
 * Returns all resources (podcasts) from the database
 * @return
 */
@GET
@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})
public List&lt;Podcast&gt; getPodcasts() {
	return podcastDao.getPodcasts();
}</code></pre>

Annotations

  * ``<p class="note_alert" style="text-align: justify;">
  <strong><span style="font-size: 1.2em;">Note:</span> At the time of writing this post I was just starting with REST and Jersey so I  suggest you have a look at  <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank"><span style="font-size: 1.2em;">Tutorial – REST API design and implementation in Java with Jersey and Spring </span> </a>instead. My gained REST knowledge will be from now on reflected in this post, which is already an &#8220;(r)evolution&#8221; regarding REST API design, REST best practices used and backend architecture/implementation supporting the REST API presented in the tutorial.</strong>
</p>

<p style="text-align: justify;">
  Looking to REST? In Java? There&#8217;s never time for that :), but if you are looking to use an <em>&#8220;architectural style consisting of a coordinated set of constraints applied to components, connectors, and data elements, within a distributed <a title="Hypermedia" href="http://en.wikipedia.org/wiki/Hypermedia">hypermedia</a> system&#8221;</em> in Java<em>,</em> then you have come to the right place, because in this post I will present a simple RESTful API that maps REST calls to backend services offering <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD</a> functionality.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> I will not focus too much on <a title="Wikipedia - REST" href="http://en.wikipedia.org/wiki/Representational_State_Transfer" target="_blank"><b>Representational state transfer</b></a> (<b>REST</b>) itself, because there are plenty of resources on the topic in the internet, some of which I listed under Resources at the end of the post.
</p>

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#1_The_example">1. The example</a><ul>
          <li>
            <a href="#11_Why">1.1. Why?</a>
          </li>
          <li>
            <a href="#12_What_does_it_do">1.2. What does it do?</a>
          </li>
          <li>
            <a href="#13_Architecture_and_technologies">1.3. Architecture and technologies</a><ul>
              <li>
                <a href="#131_Jersey">1.3.1. Jersey</a>
              </li>
              <li>
                <a href="#132_Spring">1.3.2. Spring</a>
              </li>
              <li>
                <a href="#133_MyBatis">1.3.3. MyBatis</a>
              </li>
              <li>
                <a href="#134_Web_Container">1.3.4. Web Container</a>
              </li>
              <li>
                <a href="#135_MySQL">1.3.5. MySQL</a>
              </li>
              <li>
                <a href="#136_Technologies">1.3.6. Technologies</a>
              </li>
              <li>
                <a href="#137_Follow_along">1.3.7. Follow along</a>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#2_The_coding">2. The coding</a><ul>
          <li>
            <a href="#21_Configuration">2.1. Configuration</a><ul>
              <li>
                <a href="#211_Project_dependencies">2.1.1. Project dependencies</a>
              </li>
              <li>
                <a href="#212_Web_Application_Deployment_Descriptor_8211_webxml">2.1.2. Web Application Deployment Descriptor &#8211; web.xml</a><ul>
                  <li>
                    <a href="#2121_Jersey-servlet">2.1.2.1. Jersey-servlet</a>
                  </li>
                  <li>
                    <a href="#2122_Spring_application_context_configuration">2.1.2.2. Spring application context configuration</a>
                  </li>
                </ul>
              </li>
            </ul>
          </li>

          <li>
            <a href="#22_The_RESTful_API">2.2. The RESTful API</a><ul>
              <li>
                <a href="#221_Resources">2.2.1. Resources</a>
              </li>
              <li>
                <a href="#222_Methods">2.2.2. Methods</a><ul>
                  <li>
                    <a href="#2221_CREATE">2.2.2.1. CREATE</a><ul>
                      <li>
                        <a href="#22211_Create_a_single_resource_8220podcast8221_from_JSON_input">2.2.2.1.1. Create a single resource (&#8220;podcast&#8221;) from JSON input</a>
                      </li>
                      <li>
                        <a href="#22212_Create_multiple_resources_8220podcasts8221_from_JSON_input">2.2.2.1.2. Create multiple resources (&#8220;podcasts&#8221;) from JSON input</a>
                      </li>
                      <li>
                        <a href="#22213_Create_a_single_resource_8220podcast8221_from_form">2.2.2.1.3. Create a single resource (&#8220;podcast&#8221;) from form</a>
                      </li>
                    </ul>
                  </li>

                  <li>
                    <a href="#2222_READ">2.2.2.2. READ</a><ul>
                      <li>
                        <a href="#22221_Read_all_resources">2.2.2.2.1. Read all resources</a>
                      </li>
                      <li>
                        <a href="#22222_Read_one_resource">2.2.2.2.2. Read one resource</a>
                      </li>
                    </ul>
                  </li>

                  <li>
                    <a href="#2223_UPDATE">2.2.2.3. UPDATE</a>
                  </li>
                  <li>
                    <a href="#2224_DELETE">2.2.2.4. DELETE</a><ul>
                      <li>
                        <a href="#22241_Delete_all_resources">2.2.2.4.1. Delete all resources</a>
                      </li>
                      <li>
                        <a href="#22242_Delete_one_resource">2.2.2.4.2. Delete one resource</a>
                      </li>
                    </ul>
                  </li>
                </ul>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#3_Testing">3. Testing</a><ul>
          <li>
            <a href="#31_Integration_tests">3.1. Integration tests</a><ul>
              <li>
                <a href="#311_Configuration">3.1.1. Configuration</a><ul>
                  <li>
                    <a href="#3111_Jersey_client_dependency">3.1.1.1 Jersey client dependency</a>
                  </li>
                  <li>
                    <a href="#3112_Failsafe_plugin">3.1.1.2. Failsafe plugin</a>
                  </li>
                  <li>
                    <a href="#3112_Jetty_Maven_Plugin">3.1.1.2. Jetty Maven Plugin</a>
                  </li>
                </ul>
              </li>

              <li>
                <a href="#312_Build_the_integration_tests">3.1.2. Build the integration tests</a>
              </li>
              <li>
                <a href="#313_Running_the_integration_tests">3.1.3. Running the integration tests</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#32_Live_tests">3.2. Live tests</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#4_Summary">4. Summary</a>
      </li>
      <li>
        <a href="#5_Resources">5. Resources</a><ul>
          <li>
            <a href="#51_Source_Code">5.1. Source Code</a>
          </li>
          <li>
            <a href="#52_Web_resources">5.2. Web resources</a>
          </li>
          <li>
            <a href="#53_Codingpedia_resources">5.3. Codingpedia resources</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</p>

<p style="text-align: justify;">
  <!--more-->
</p>

<h2 style="text-align: justify;">
  <span id="1_The_example">1. The example</span>
</h2>

<h3 style="text-align: justify;">
  <span id="11_Why">1.1. Why?</span>
</h3>

<p style="text-align: justify;">
  My intention is to move some of the parts from <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, that are currently implemented in Spring MVC, to JavaScript-land and for that I&#8217;ll need to use more backend REST web services. Of course I could use <a title="Designing and Implementing RESTful Web Services with Spring" href="https://spring.io/guides/tutorials/rest/" target="_blank">Spring&#8217;s own REST implementation</a>, as I currently do for the AJAX calls, but I wanted also to see how the &#8220;official&#8221; implementation looks like.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> You can visit my post <a title="Autocomplete search box with jQuery and Spring MVC" href="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" target="_blank">Autocomplete search box with jQuery and Spring MVC</a> to see how Spring handles REST requests.
</p>

<h3 style="text-align: justify;">
  <span id="12_What_does_it_do">1.2. What does it do?</span>
</h3>

<p style="text-align: justify;">
  So, the best way to get to know the technology is build a prototype with it. And that&#8217;s exactly what I did and what I will present in this post. I&#8217;ve build a simple application that &#8220;manages&#8221; podcasts via a REST API. It does CRUD operations on a single database table (Podcasts), triggered via the REST web services API. Though fairly simple, the example highlights the most common annotations you&#8217;ll need to build your own REST API.
</p>

<h3 style="text-align: justify;">
  <span id="13_Architecture_and_technologies">1.3. Architecture and technologies</span>
</h3>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/01/Architecture.png"><img class="alignnone size-medium wp-image-1080" src="{{site.url}}/wp-content/uploads/2014/01/Architecture-300x140.png" alt="Demo architecture" width="300" height="140" srcset="{{site.url}}/wp-content/uploads/2014/01/Architecture-300x140.png 300w, {{site.url}}/wp-content/uploads/2014/01/Architecture.png 602w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

<h4 style="text-align: justify;">
  <span id="131_Jersey">1.3.1. Jersey</span>
</h4>

<p style="text-align: justify;">
  The architecture is straightforward: with any REST client you can call the application&#8217;s API exposed via <a title="Jersey - RESTful Web Services in Java" href="https://jersey.java.net/" target="_blank"><strong>Jersey</strong> RESTful Web Services in JAVA</a>. The <em>Jersey RESTful Web Services framework</em> is open source, production quality, framework for developing RESTful Web Services in Java that provides support for JAX-RS APIs and serves as a <a title="Java API for RESTful Services (JAX-RS)" href="https://jax-rs-spec.java.net/" target="_blank">JAX-RS</a> (JSR 311 & JSR 339) Reference Implementation.
</p>

<h4 style="text-align: justify;">
  <span id="132_Spring">1.3.2. Spring</span>
</h4>

<p style="text-align: justify;">
  I like glueing stuff together with <strong><a title="Spring Framework" href="http://projects.spring.io/spring-framework/" target="_blank">Spring</a></strong>, and this example is no exception. You&#8217;ll find out how Jersey 2 integrates with Spring.
</p>

<h4 style="text-align: justify;">
  <span id="133_MyBatis">1.3.3. MyBatis</span>
</h4>

<p style="text-align: justify;">
  For the persistence layer, I chose Mybatis because I like it the most and it integrates fairly easy with Spring (see my post <a title="Spring MyBatis integration example" href="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" target="_blank">Spring MyBatis integration example</a> for more on that), but you are free to choose any framework and technology you&#8217;re familiar with (e.g. JPA/Hibernate, Hibernate, JDBC etc).
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> If you want to see how the persistence layer is implemented for the same application with JPA2 and Hibernate 4 see the post <a title="Java Persistence Example with Spring, JPA2 and Hibernate" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate</a>.
</p>

<h4 style="text-align: justify;">
  <span id="134_Web_Container">1.3.4. Web Container</span>
</h4>

<p style="text-align: justify;">
  Everything gets packaged as a <code>.war</code> file and can be deployed on any web container &#8211; I used <a title="Apache Tomcat" href="http://tomcat.apache.org/" target="_blank">Tomcat</a> and <a title="Jetty" href="http://www.eclipse.org/jetty/" target="_blank">Jetty</a> but, it could also be Glassfih, Weblogic, JBoss or WebSphere.
</p>

<h4 style="text-align: justify;">
  <span id="135_MySQL">1.3.5. MySQL</span>
</h4>

<p style="text-align: justify;">
  The sample data is stored in a <a title="MySQL" href="http://www.mysql.com/" target="_blank">MySQL</a> table:
</p>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/01/database-schema.png"><img class="alignnone size-medium wp-image-1089" src="{{site.url}}/wp-content/uploads/2014/01/database-schema-300x280.png" alt="database schema" width="300" height="280" srcset="{{site.url}}/wp-content/uploads/2014/01/database-schema-300x280.png 300w, {{site.url}}/wp-content/uploads/2014/01/database-schema.png 566w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The main focus in the post will be on the Jersey JAX-RS implementation, all the other technologies are viewed as enablers.
</p>

#### <span id="136_Technologies">1.3.6. Technologies</span>

  1. Jersey 2.4
  2. Spring 3.2
  3. Maven 3
  4. Tomcat 7
  5. Jetty 9
  6. MySql 5.6

#### <span id="137_Follow_along">1.3.7. Follow along</span>

If you want to follow along, you find all you need on GitHub:

  * <a title="Source code" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis (source code of the project)</a>
  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/resources/input_data/DumpRESTdemoDB.sql" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/resources/input_data/DumpRESTdemoDB.sql" target="_blank">MySQL DB creation self-contained file</a> &#8211; everything is configured for the `username/password` : `rest_demo/rest_demo`

## <span id="2_The_coding">2. The coding</span>

### <span id="21_Configuration">2.1. Configuration</span>

#### <span id="211_Project_dependencies">2.1.1. Project dependencies</span>

<p style="text-align: justify;">
  Among other things you need to have Jersey Spring extension in your project&#8217;s classpath. You can easily add that with Maven by having the following dependencies to the <code>pom.xml</code> file of the project:
</p>

<pre><code class="xml">&lt;dependency&gt;
	&lt;groupId&gt;org.glassfish.jersey.ext&lt;/groupId&gt;
	&lt;artifactId&gt;jersey-spring3&lt;/artifactId&gt;
	&lt;version&gt;2.4.1&lt;/version&gt;
	&lt;exclusions&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-core&lt;/artifactId&gt;
		&lt;/exclusion&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-web&lt;/artifactId&gt;
		&lt;/exclusion&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-beans&lt;/artifactId&gt;
		&lt;/exclusion&gt;
	&lt;/exclusions&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.glassfish.jersey.media&lt;/groupId&gt;
	&lt;artifactId&gt;jersey-media-json-jackson&lt;/artifactId&gt;
	&lt;version&gt;2.4.1&lt;/version&gt;
&lt;/dependency&gt;</code></pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The jersey-spring3.jar, uses its own version for Spring libraries, so to use the ones you want, you need to exclude these libraries manually.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> If you want to see what other dependencies are used (for Spring, Jetty, testing) in the project or how Jetty is configured so that you can start the project directly in Jetty, you can download the complete <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" target="_blank">pom.xml</a> file from GitHub &#8211; <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml</a>
</p>

#### <span id="212_Web_Application_Deployment_Descriptor_8211_webxml">2.1.2. Web Application Deployment Descriptor &#8211; web.xml</span>

<pre><code class="xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"&gt;
	&lt;display-name&gt;Demo - Restful Web Application&lt;/display-name&gt;

	&lt;listener&gt;
		&lt;listener-class&gt;
			org.springframework.web.context.ContextLoaderListener
		&lt;/listener-class&gt;
	&lt;/listener&gt;

	&lt;context-param&gt;
		&lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
		&lt;param-value&gt;classpath:spring/applicationContext.xml&lt;/param-value&gt;
	&lt;/context-param&gt;

	&lt;servlet&gt;
		&lt;servlet-name&gt;jersey-serlvet&lt;/servlet-name&gt;
		&lt;servlet-class&gt;
			org.glassfish.jersey.servlet.ServletContainer
		&lt;/servlet-class&gt;
		&lt;init-param&gt;
			&lt;param-name&gt;javax.ws.rs.Application&lt;/param-name&gt;
			&lt;param-value&gt;org.codingpedia.demo.rest.service.MyApplication&lt;/param-value&gt;
		&lt;/init-param&gt;
		&lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
	&lt;/servlet&gt;

	&lt;servlet-mapping&gt;
		&lt;servlet-name&gt;jersey-serlvet&lt;/servlet-name&gt;
		&lt;url-pattern&gt;/*&lt;/url-pattern&gt;
	&lt;/servlet-mapping&gt;

	&lt;resource-ref&gt;
        &lt;description&gt;Database resource rest demo web application &lt;/description&gt;
        &lt;res-ref-name&gt;jdbc/restDemoDB&lt;/res-ref-name&gt;
        &lt;res-type&gt;javax.sql.DataSource&lt;/res-type&gt;
        &lt;res-auth&gt;Container&lt;/res-auth&gt;
    &lt;/resource-ref&gt;
&lt;/web-app&gt;</code></pre>

##### <span id="2121_Jersey-servlet">2.1.2.1. Jersey-servlet</span>

<p style="text-align: justify;">
  Notice the Jersey servlet configuration [lines 18-33]. The <code>javax.ws.rs.core.Application</code> class defines the components of the JAX-RS application. Because I extended the <code>Application (ResourceConfig)</code> class to provide the list of relevant root resource classes (<code class="literal">getResources()</code>) and singletons (<code class="literal">getSingletons()</code>), i.e. the JAX-RS application model, I needed to register it in my web application <code class="literal">web.xml</code> deployment descriptor using a Servlet or Servlet filter initialization parameter with a name of <code class="literal">javax.ws.rs.Application.</code>Check out the <a title="https://jersey.java.net/documentation/latest/deployment.html" href="https://jersey.java.net/documentation/latest/deployment.html" target="_blank">documentation</a> for other possibilities.
</p>

<p style="text-align: justify;">
  The implementation of <code>org.codingpedia.demo.rest.service.MyApplication</code> looks like the following in the project:
</p>

<pre><code class="java">package org.codingpedia.demo.rest.service;

import org.glassfish.jersey.jackson.JacksonFeature;
import org.glassfish.jersey.server.ResourceConfig;
import org.glassfish.jersey.server.spring.scope.RequestContextFilter;

/**
 * Registers the components to be used by the JAX-RS application
 *
 * @author ama
 *
 */
public class MyDemoApplication extends ResourceConfig {

    /**
	* Register JAX-RS application components.
	*/
	public MyDemoApplication(){
		register(RequestContextFilter.class);
		register(PodcastRestService.class);
		register(JacksonFeature.class);
	}
}</code></pre>

The class registers the following components

<li style="text-align: justify;">
  <code>org.glassfish.jersey.server.spring.scope.RequestContextFilter</code>, which is a Spring filter that provides a bridge between JAX-RS and Spring request attributes
</li>
<li style="text-align: justify;">
  <code>org.codingpedia.demo.rest.service.PodcastRestService</code>, which is the service component that exposes the REST API via annotations, will be presented later in detail
</li>
<li style="text-align: justify;">
  <code>org.glassfish.jersey.jackson.JacksonFeature</code>, which is a feature that registers Jackson JSON providers &#8211; you need it for the application to understand JSON data
</li>

##### <span id="2122_Spring_application_context_configuration">2.1.2.2. Spring application context configuration</span>

The Spring application context configuration is located in the classpath under `spring/applicationContext.xml`:

<pre><code class="xml">&lt;beans xmlns="http://www.springframework.org/schema/beans"
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

	&lt;!--
		Instruct Spring to perform declarative transaction management
		automatically on annotated classes.
	--&gt;
	&lt;tx:annotation-driven transaction-manager="transactionManager" /&gt;
    &lt;bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager"&gt;
        &lt;property name="dataSource" ref="dataSource"/&gt;
    &lt;/bean&gt;

	&lt;!-- =============== MyBATIS beans configuration ================== --&gt;
	&lt;bean id="podcastDao" class="org.mybatis.spring.mapper.MapperFactoryBean"&gt;
	   &lt;property name="sqlSessionFactory" ref="sqlSessionFactory"/&gt;
	   &lt;property name="mapperInterface" value="org.codingpedia.demo.rest.dao.PodcastDao" /&gt;
	&lt;/bean&gt;

	&lt;bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"&gt;
	    &lt;property name="dataSource" ref="dataSource" /&gt;
	    &lt;property name="configLocation" value="classpath:config/mybatisV3.xml"/&gt;
	&lt;/bean&gt;

    &lt;bean id="podcastRestService" class="org.codingpedia.demo.rest.service.PodcastRestService" /&gt;

	&lt;bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoDB" /&gt;
	    &lt;property name="resourceRef" value="true" /&gt;
	&lt;/bean&gt;
&lt;/beans&gt;</code></pre>

<p style="text-align: justify;">
  Nothing special here, it just defines the beans that are needed throughout the demo application. The most important one is the <code>podcastRestService</code> which is actually the entry point class for our RESTful API, and will be thouroughly described in the next paragraphs.
</p>

### <span id="22_The_RESTful_API">2.2. The RESTful API</span>

#### <span id="221_Resources">2.2.1. Resources</span>

<p style="text-align: justify;">
  As mentioned earlier, the demo application manages podcasts, which represent the <a title="Wikipedia - Web resource" href="http://en.wikipedia.org/wiki/Web_resource" target="_blank">resources</a> in our web API. Resources are the central concept in REST and are characterized by two main things:
</p>

  * each is referenced with a global identifier (e.g. a [URI](http://en.wikipedia.org/wiki/Uniform_resource_identifier "Uniform resource identifier") in HTTP).
  * has one or more representations, that they expose to the outer world and can be manipulated with (we&#8217;ll be working mostly with JSON representations in this example)

The podcast resources are represented in our application by the <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/entities/Podcast.java" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/entities/Podcast.java" target="_blank">Podcast</a> class:

<pre><code class="java">package org.codingpedia.demo.rest.entities;

import java.io.Serializable;
import java.util.Date;

import javax.xml.bind.annotation.XmlRootElement;

/**
 * Podcast entity
 *
 * @author ama
 *
 */
@XmlRootElement
public class Podcast implements Serializable {

	private static final long serialVersionUID = -8039686696076337053L;

	/** id of the podcas */
	private Long id;

	/** title of the podcast */
	private String title;

	/** link of the podcast on Podcastpedia.org */
	private String linkOnPodcastpedia;

	/** url of the feed */
	private String feed;

	/** description of the podcast */
	private String description;

	/** when an episode was last published on the feed*/
	private Date insertionDate;

	public Podcast(){}

	public Podcast(String title, String linkOnPodcastpedia, String feed,
			String description) {

		this.title = title;
		this.linkOnPodcastpedia = linkOnPodcastpedia;
		this.feed = feed;
		this.description = description;

	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getLinkOnPodcastpedia() {
		return linkOnPodcastpedia;
	}

	public void setLinkOnPodcastpedia(String linkOnPodcastpedia) {
		this.linkOnPodcastpedia = linkOnPodcastpedia;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFeed() {
		return feed;
	}

	public void setFeed(String feed) {
		this.feed = feed;
	}

	public Date getInsertionDate() {
		return insertionDate;
	}

	public void setInsertionDate(Date insertionDate) {
		this.insertionDate = insertionDate;
	}

}</code></pre>

The strucuture is pretty simple &#8211; there are an `id`, which identifies a podcast, and several other fields that we&#8217;ll can see in the JSON representation:

<pre><code class="json">{
	"id":1,
	"title":"Quarks & Co - zum Mitnehmen-modified",
	"linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen",
	"feed":"http://podcast.wdr.de/quarks.xml",
	"description":"Quarks & Co: Das Wissenschaftsmagazin",
	"insertionDate":1388213547000
}</code></pre>

#### <span id="222_Methods">2.2.2. Methods</span>

The API exposed by our example is described in the following table:

<table style="width: 100%;" border="1" cellspacing="0" cellpadding="5">
  <colgroup> <col width="85*" /> <col width="85*" /> <col width="85*" /> </colgroup> <tr valign="TOP">
    <td bgcolor="#808080" width="33%">
      <b> Resource</b>
    </td>

    <td bgcolor="#808080" width="33%">
      <b> URI</b>
    </td>

    <td bgcolor="#808080" width="33%">
      <b> Method</b>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="19">
      <p align="CENTER">
        <b>CREATE</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="19">
      Add a list podcasts
    </td>

    <td width="33%">
      /podcasts/list
    </td>

    <td width="33%">
      <p align="CENTER">
        POST
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="19">
      Add a new podcast
    </td>

    <td width="33%">
      /podcasts/
    </td>

    <td width="33%">
      <p align="CENTER">
        POST
      </p>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="16">
      <p align="CENTER">
        <b>READ</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      List of all podcasts
    </td>

    <td width="33%">
      /podcasts/
    </td>

    <td width="33%">
      <p align="CENTER">
        GET
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      List a single podcast
    </td>

    <td width="33%">
      /podcasts/{id}
    </td>

    <td width="33%">
      <p align="CENTER">
        GET
      </p>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="16">
      <p align="CENTER">
        <b>UPDATE</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      Updates a single podcasts or creates one if not existent
    </td>

    <td width="33%">
      /podcasts/{id}
    </td>

    <td width="33%">
      <p align="CENTER">
        PUT
      </p>
    </td>
  </tr>

  <tr>
    <td colspan="3" valign="TOP" width="100%" height="16">
      <p align="CENTER">
        <b>DELETE</b>
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="16">
      Delete all podcasts
    </td>

    <td width="33%">
      /podcasts/
    </td>

    <td width="33%">
      <p align="CENTER">
        DELETE
      </p>
    </td>
  </tr>

  <tr style="height: 10px;">
    <td width="33%" height="15">
      Delete a single podcast
    </td>

    <td width="33%">
      /podcasts/{id}
    </td>

    <td width="33%">
      <p align="CENTER">
        DELETE
      </p>
    </td>
  </tr>
</table>

As already mentioned the `PodcastRestService` class is the one handling all the rest requests:

<pre><code class="java">package org.codingpedia.demo.rest.service;
......................
@Component
@Path("/podcasts")
public class PodcastRestService {

	private static final String HOST = "http://localhost:8080/demo-rest-spring-jersey-tomcat-mybatis-0.0.1-SNAPSHOT";

	@Autowired
	private PodcastDao podcastDao;
	......................
}</code></pre>

<p style="text-align: justify;">
  Notice the <code>@Path("/podcasts")</code> before the class definition. The <a href="http://jax-rs-spec.java.net/nonav/2.0/apidocs/javax/ws/rs/Path.html" target="_top">@Path</a> annotation&#8217;s value is a relative URI path. In the example above, the Java class will be hosted at the URI path <code>/podcasts</code>. The <code>PodcastDao</code> interface is used to communicate with the database.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> You can find the entire content of the class on GitHub &#8211; <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/src/main/java/org/codingpedia/demo/rest/service/PodcastRestService.java</a>. We&#8217;ll be going through the file step by step and explain the different methods corresponding to the different operations.
</p>

##### <span id="2221_CREATE">2.2.2.1. CREATE</span>

For the creation of new resources(&#8220;podcasts&#8221;) I use the <a title="Wikpedia - POST (HTTP)" href="http://en.wikipedia.org/wiki/POST_%28HTTP%29" target="_blank">POST (HTTP)</a> method.

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> In JAX-RS (Jersey) you specifies the HTTP methods (GET, POST, PUT, DELETE) by placing the corresponding annotation in front of the method.
</p>

###### <span id="22211_Create_a_single_resource_8220podcast8221_from_JSON_input">2.2.2.1.1. Create a single resource (&#8220;podcast&#8221;) from JSON input</span>

<pre><code class="java">/**
 * Adds a new resource (podcast) from the given json format (at least title and feed elements are required
 * at the DB level)
 *
 * @param podcast
 * @return
 */
@POST
@Consumes({MediaType.APPLICATION_JSON})
@Produces({MediaType.TEXT_HTML})
@Transactional
public Response createPodcast(Podcast podcast) {
	Long id = podcastDao.createPodcast(podcast);

	return Response.status(201).entity(buildNewPodcastResourceURL(id)).build();
}</code></pre>

Annotations

  * `<code>@POST`</code> &#8211; indicates that the method responds to HTTP POST requests
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
<li style="text-align: justify;">
  <code>@Produces({MediaType.TEXT_HTML})</code> &#8211; defines the media type) that the method can produce, in this case <code>"text/html"</code>. The response will be a html document, with a status of 201, indicating to the caller that the request has been fulfilled and resulted in a new resource being created.
</li>
<li style="text-align: justify;">
  <code>@Transactional</code> &#8211; Spring annotation, specifies that the method execution, should take place inside a transaction
</li>

###### <span id="22212_Create_multiple_resources_8220podcasts8221_from_JSON_input">2.2.2.1.2. Create multiple resources (&#8220;podcasts&#8221;) from JSON input</span>

<pre><code class="java">/**
	 * A list of resources (here podcasts) provided in json format will be added
	 * to the database.
	 *
	 * @param podcasts
	 * @return
	 */
	@POST @Path("list")
	@Consumes({MediaType.APPLICATION_JSON})
	@Transactional
	public Response createPodcasts(List&lt;Podcast&gt; podcasts) {
		for(Podcast podcast : podcasts){
			podcastDao.createPodcast(podcast);
		}

		return Response.status(204).build();
	}</code></pre>

Annotations

  * `@POST` &#8211; indicates that the method responds to HTTP POST requests
<li style="text-align: justify;">
  <code>@Path("/list")</code> &#8211; identifies the URI path that the class method will serve requests for. Paths are relative. The combined path here will be <code>"/podcasts/list"</code>, because as we have seen we have <code>@Path</code> annotation at the class level
</li>
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
<li style="text-align: justify;">
  <code>@Transactional</code>&#8211; Spring annotation, specifies that the method execution, should take place inside a transaction
</li>

<p style="text-align: justify;">
  In this case the method returns a status of 204 (&#8220;No Content&#8221;), suggesting that the server has fulfilled the request but does not need to return an entity-body, and might want to return updated metainformation.
</p>

###### <span id="22213_Create_a_single_resource_8220podcast8221_from_form">2.2.2.1.3. Create a single resource (&#8220;podcast&#8221;) from form</span>

<pre><code class="java">/**
 * Adds a new resource (podcast) from "form" (at least title and feed elements are required
 * at the DB level)
 *
 * @param title
 * @param linkOnPodcastpedia
 * @param feed
 * @param description
 * @return
 */
@POST
@Consumes(MediaType.APPLICATION_FORM_URLENCODED)
@Produces({MediaType.TEXT_HTML})
@Transactional
public Response createPodcastFromForm(
					@FormParam("title") String title,
					@FormParam("linkOnPodcastpedia") String linkOnPodcastpedia,
					@FormParam("feed") String feed,
					@FormParam("description") String description
					) {
	Podcast podcast = new Podcast(title, linkOnPodcastpedia, feed, description);
	Long id = podcastDao.createPodcast(podcast);

	return Response.status(201).entity(buildNewPodcastResourceURL(id)).build();
}</code></pre>

Annotations

  * `@POST` &#8211; indicates that the method responds to HTTP POST requests
<li style="text-align: justify;">
  <code>@Consumes({MediaType.APPLICATION_FORM_URLENCODED})</code> <ul>
    <ul>
      &#8211; defines the media type, the method accepts, in this case
    </ul>
  </ul>

  <p>
    <code>"application/x-www-form-urlencoded"</code>
  </p>

  <ul>
    <li style="text-align: justify;">
      @FormParam &#8211; present before the input parameters of the method, this annotation binds the value(s) of a form parameter contained within a request entity body to a resource method parameter. Values are URL decoded unless this is disabled using the <code>Encoded</code> annotation
    </li>
  </ul>
</li>

<li style="text-align: justify;">
  <code>&lt;code>@Produces({MediaType.TEXT_HTML})</code> - defines the media type) that the method can produce, in this case "text/html". The response will be a html document, with a status of 201, indicating to the caller that the request has been fulfilled and resulted in a new resource being created.</code>
</li>
  * `@Transactional` &#8211; Spring annotation, specifies that the method execution, should take place inside a transaction

##### <span id="2222_READ">2.2.2.2. READ</span>

###### <span id="22221_Read_all_resources">2.2.2.2.1. Read all resources</span>

<pre><code class="java">/**
 * Returns all resources (podcasts) from the database
 * @return
 */
@GET
@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})
public List&lt;Podcast&gt; getPodcasts() {
	return podcastDao.getPodcasts();
}</code></pre>

Annotations

  *`` &#8211; indicates that the method responds to HTTP GET requests
<li style="text-align: justify;">
  <code>@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})</code> &#8211; defines the media type) that the method can produce, in this case <code>"application/json"</code> or <code>"application/xml"</code>(you need the <code>@XmlRootElement</code> in front of the <code>Podcast</code> class to produce xml formatted response). The response will be a list of podcasts either in JSON or XML format.
</li>

###### <span id="22222_Read_one_resource">2.2.2.2.2. Read one resource</span>

<pre><code class="java">@GET @Path("{id}")
@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})
public Response findById(@PathParam("id") Long id) {
	Podcast podcastById = podcastDao.getPodcastById(id);
	if(podcastById != null) {
		return Response.status(200).entity(podcastById).build();
	} else {
		return Response.status(404).entity("The podcast with the id " + id + " does not exist").build();
	}
}</code></pre>

Annotations

  * `@GET` &#8211; indicates that the method responds to HTTP GET requests
<li style="text-align: justify;">
  <code>@Path("{id}")</code> &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the <code>@PathParam</code> variable. <ul>
    <li style="text-align: justify;">
      <code>@PathParam("id")</code> &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the <code>@Encoded</code> annotation. A default value can be specified using the <code>@DefaultValue</code> annotation.
    </li>
  </ul>
</li>

<li style="text-align: justify;">
  <code>@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})</code> &#8211; defines the media type) that the method can produce, in this case <code>"application/json"</code> or <code>"application/xml"</code>(you need the <code>@XmlRootElement</code> in front of the Podcast class to produce the response in xml format). The response will be a podcast either in JSON or XML format with the &#8220;200 OK&#8221; status, or a message saying the podcast does not exit with a &#8220;404 Not Found&#8221; status.
</li>

##### <span id="2223_UPDATE">2.2.2.3. UPDATE</span>

<pre><code class="java">/**
 * Updates the attributes of the podcast received via JSON for the given @param id
 *
 * If the podcast does not exist yet in the database (verified by &lt;strong&gt;id&lt;/strong&gt;) then
 * the application will try to create a new podcast resource in the db
 *
 * @param id
 * @param podcast
 * @return
 */
@PUT @Path("{id}")
@Consumes({MediaType.APPLICATION_JSON})
@Produces({MediaType.TEXT_HTML})
@Transactional
public Response updatePodcastById(@PathParam("id") Long id, Podcast podcast) {
	if(podcast.getId() == null) podcast.setId(id);
	String message;
	int status;
	if(podcastWasUpdated(podcast)){
		status = 200; //OK
		message = "Podcast has been updated";
	} else if(podcastCanBeCreated(podcast)){
		podcastDao.createPodcast(podcast);
		status = 201; //Created
		message = "The podcast you provided has been added to the database";
	} else {
		status = 406; //Not acceptable
		message = "The information you provided is not sufficient to perform either an UPDATE or "
				+ " an INSERTION of the new podcast resource &lt;br/&gt;"
				+ " If you want to UPDATE please make sure you provide an existent &lt;strong&gt;id&lt;/strong&gt; &lt;br/&gt;"
				+ " If you want to insert a new podcast please provide at least a &lt;strong&gt;title&lt;/strong&gt; and the &lt;strong&gt;feed&lt;/strong&gt; for the podcast resource";
	}

	return Response.status(status).entity(message).build();
}</code></pre>

Annotations

  * `@PUT`&#8211; indicates that the method responds to HTTP PUT requests
<li style="text-align: justify;">
  <code>@Path("{id}")</code> &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the <code>@PathParam</code> variable. <ul>
    <li style="text-align: justify;">
      <code>@PathParam("id")</code> &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the <code>@Encoded</code> annotation. A default value can be specified using the <code>@DefaultValue</code> annotation.
    </li>
  </ul>
</li>

<li style="text-align: justify;">
  <code>@Consumes({MediaType.APPLICATION_JSON})</code> &#8211; defines the media type, the method accepts, in this case <code>"application/json"</code>
</li>
<li style="text-align: justify;">
  <code>@Produces({MediaType.TEXT_HTML})</code> &#8211; defines the media type) that the method can produce, in this case &#8220;text/html&#8221;. The response will be a html document containing different messages and stati depending on what action has been taken <ul>
    <li style="text-align: justify;">
      200 &#8211; OK, &#8220;podcast updated successfully &#8220;
    </li>
    <li style="text-align: justify;">
      201 &#8211; id given was not found in the db, so a new podcast resource has been created
    </li>
    <li style="text-align: justify;">
      406 &#8211; if id was not found and you haven&#8217;t provided enough information for the creation of a new resource, the request is &#8220;Not Acceptable&#8221;
    </li>
  </ul>
</li>

##### <span id="2224_DELETE">2.2.2.4. DELETE</span>

###### <span id="22241_Delete_all_resources">2.2.2.4.1. Delete all resources</span>

<pre><code class="java">@DELETE
@Produces({MediaType.TEXT_HTML})
public Response deletePodcasts() {
	podcastDao.deletePodcasts();
	return Response.status(200).entity("All podcasts have been successfully removed").build();
}</code></pre>

Annotations

  * `@DELETE` &#8211; indicates that the method responds to HTTP DELETE requests
<li style="text-align: justify;">
  <code>@Produces({MediaType.TEXT_HTML})</code> &#8211; defines the media type that the method can produce, in this case &#8220;text/html&#8221;. The response will be a html document, with a status of 200, indicating to the caller that the request has been fulfilled.
</li>
  * `@Transactional` &#8211; Spring annotation, specifies that the method execution, should take place inside a transaction

###### <span id="22242_Delete_one_resource">2.2.2.4.2. Delete one resource</span>

<pre class="lang:java decode:true">@DELETE @Path("{id}")
@Produces({MediaType.TEXT_HTML})
@Transactional
public Response deletePodcastById(@PathParam("id") Long id) {
	if(podcastDao.deletePodcastById(id) == 1){
		return Response.status(204).build();
	} else {
		return Response.status(404).entity("Podcast with the id " + id + " is not present in the database").build();
	}
}</code></pre>

Annotations

  * `@DELETE` &#8211; indicates that the method responds to HTTP DELETE requests
  * `@Path("{id}")` &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the `@PathParam` variable.
      * `@PathParam("id")` &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the `@Encoded` annotation. A default value can be specified using the `@DefaultValue` annotation.
  * `@Produces({MediaType.TEXT_HTML})` &#8211; defines the media type that the method can produce, in this case &#8220;text/html&#8221;. If the podcast is deleted, that is found in the database, a 204 &#8220;No Content&#8221; success status is returnred, otherwise an html document with the status of 404 &#8220;Not found&#8221; is returned
  * `@Transactional` &#8211; Spring annotation, specifies that the method execution, should take place inside a transaction

## <span id="3_Testing">3. Testing</span>

### <span id="31_Integration_tests">3.1. Integration tests</span>

<p style="text-align: justify;">
  To test the application I will use the <code>Jersey Client</code> and execute requests against a running Jetty server with the application deployed on it. For that I will use the <a title="https://maven.apache.org/surefire/maven-failsafe-plugin/" href="https://maven.apache.org/surefire/maven-failsafe-plugin/">Maven Failsafe Plugin</a>.
</p>

#### <span id="311_Configuration">3.1.1. Configuration</span>

##### <span id="3111_Jersey_client_dependency">3.1.1.1 Jersey client dependency</span>

To build a Jersey client the `jersey-client` jar is required in the classpath. With Maven you can add it as a dependency to the `pom.xml` file:

<pre><code class="xml">&lt;dependency&gt;
    &lt;groupId&gt;org.glassfish.jersey.core&lt;/groupId&gt;
    &lt;artifactId&gt;jersey-client&lt;/artifactId&gt;
    &lt;version&gt;2.4.1&lt;/version&gt;
    &lt;scope&gt;test&lt;/scope&gt;
&lt;/dependency&gt;</code></pre>

<h5 style="text-align: justify;">
  <span id="3112_Failsafe_plugin">3.1.1.2. Failsafe plugin</span>
</h5>

<p style="text-align: justify;">
  The Failsafe Plugin is used during the integration-test and verify phases of the build lifecycle to execute the integration tests of the application. The Failsafe Plugin will not fail the build during the integration-test phase thus enabling the post-integration-test phase to execute.<br /> To use the Failsafe Plugin, you need to add the following configuration to your <code>pom.xml</code>
</p>

<pre><code class="xml">&lt;plugins&gt;
	[...]
    &lt;plugin&gt;
        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
        &lt;artifactId&gt;maven-failsafe-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.16&lt;/version&gt;
        &lt;executions&gt;
            &lt;execution&gt;
                &lt;id&gt;integration-test&lt;/id&gt;
                &lt;goals&gt;
                    &lt;goal&gt;integration-test&lt;/goal&gt;
                &lt;/goals&gt;
            &lt;/execution&gt;
            &lt;execution&gt;
                &lt;id&gt;verify&lt;/id&gt;
                &lt;goals&gt;
                    &lt;goal&gt;verify&lt;/goal&gt;
                &lt;/goals&gt;
            &lt;/execution&gt;
        &lt;/executions&gt;
    &lt;/plugin&gt;
	[...]
&lt;/plugins&gt;</code></pre>

##### <span id="3112_Jetty_Maven_Plugin">3.1.1.2. Jetty Maven Plugin</span>

As mentioned, the integration tests will be executed against a running jetty server, that will be started only for the execution of the tests. For that the following execution has to be configured in the `jetty-maven-plugin` configuration:

<pre class="lang:default mark:15-34 decode:true" title="Jetty Maven Plugin configuration for integration tests">&lt;plugins&gt;
	&lt;plugin&gt;
		&lt;groupId&gt;org.eclipse.jetty&lt;/groupId&gt;
		&lt;artifactId&gt;jetty-maven-plugin&lt;/artifactId&gt;
		&lt;version&gt;${jetty.version}&lt;/version&gt;
		&lt;configuration&gt;
			&lt;jettyConfig&gt;${project.basedir}/src/main/resources/config/jetty9.xml&lt;/jettyConfig&gt;
			&lt;stopKey&gt;STOP&lt;/stopKey&gt;
			&lt;stopPort&gt;9999&lt;/stopPort&gt;
			&lt;stopWait&gt;5&lt;/stopWait&gt;
			&lt;scanIntervalSeconds&gt;5&lt;/scanIntervalSeconds&gt;
		[...]
		&lt;/configuration&gt;
		&lt;executions&gt;
			&lt;execution&gt;
				&lt;id&gt;start-jetty&lt;/id&gt;
				&lt;phase&gt;pre-integration-test&lt;/phase&gt;
				&lt;goals&gt;
					&lt;!-- stop any previous instance to free up the port --&gt;
					&lt;goal&gt;stop&lt;/goal&gt;
					&lt;goal&gt;run-exploded&lt;/goal&gt;
				&lt;/goals&gt;
				&lt;configuration&gt;
					&lt;scanIntervalSeconds&gt;0&lt;/scanIntervalSeconds&gt;
					&lt;daemon&gt;true&lt;/daemon&gt;
				&lt;/configuration&gt;
			&lt;/execution&gt;
			&lt;execution&gt;
				&lt;id&gt;stop-jetty&lt;/id&gt;
				&lt;phase&gt;post-integration-test&lt;/phase&gt;
				&lt;goals&gt;
					&lt;goal&gt;stop&lt;/goal&gt;
				&lt;/goals&gt;
			&lt;/execution&gt;
		&lt;/executions&gt;
	&lt;/plugin&gt;
	[...]
&lt;/plugins&gt;</code></pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> In the <code>pre-integration-test</code> phase the Jetty server will be started, after stopping any running instance to free up the port, and in the <code>post-integration-phase</code> it will be stopped. The <code>scanIntervalSeconds</code> has to be set to 0, and <code>daemon</code> to true.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> See the complete <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" target="_blank">pom.xml</a> file on GitHub &#8211; <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/blob/master/pom.xml</a>
</p>

#### <span id="312_Build_the_integration_tests">3.1.2. Build the integration tests</span>

I am using JUnit as the testing framework. By default, the Failsafe Plugin will automatically include all test classes with the following wildcard patterns:

  * `<tt>"**/IT*.java"</tt>` &#8211; includes all of its subdirectories and all java filenames that start with &#8220;IT&#8221;.
  * `<tt>"**/*IT.java"</tt>` &#8211; includes all of its subdirectories and all java filenames that end with &#8220;IT&#8221;.
  * `<tt>"**/*ITCase.java"</tt>` &#8211; includes all of its subdirectories and all java filenames that end with &#8220;ITCase&#8221;.

I have created a single test class &#8211; `RestDemoServiceIT` &#8211; that will test the read (GET) methods, but the procedure should be the same for all the other:

<pre><code class="java">public class RestDemoServiceIT {

	[....]
	@Test
	public void testGetPodcast() throws JsonGenerationException,
			JsonMappingException, IOException {

		ClientConfig clientConfig = new ClientConfig();
		clientConfig.register(JacksonFeature.class);

		Client client = ClientBuilder.newClient(clientConfig);

		WebTarget webTarget = client
				.target("http://localhost:8888/demo-rest-spring-jersey-tomcat-mybatis-0.0.1-SNAPSHOT/podcasts/2");

		Builder request = webTarget.request(MediaType.APPLICATION_JSON);

		Response response = request.get();
		Assert.assertTrue(response.getStatus() == 200);

		Podcast podcast = response.readEntity(Podcast.class);

		ObjectMapper mapper = new ObjectMapper();
		System.out
				.print("Received podcast from database *************************** "
						+ mapper.writerWithDefaultPrettyPrinter()
								.writeValueAsString(podcast));

	}
}</code></pre>

<p style="text-align: justify;">
  <strong>Note:</strong>
</p>

  * I had to register the JacksonFeature for the client too so that I can marshall the podcast response in JSON format &#8211; response.readEntity(Podcast.class)
  * I am testing against a running Jetty on port 8888 &#8211; I will show you in the next section how to start Jetty on a desired port
  * I am expecting a 200 status for my request
  * With the help `org.codehaus.jackson.map.ObjectMapper` I am displaying the JSON response nicely formatted

#### <span id="313_Running_the_integration_tests">3.1.3. Running the integration tests</span>

The Failsafe Plugin can be invoked by calling the `<tt>verify</tt>` phase of the build lifecycle.

<pre><code class="bash">mvn verify</code></pre>

To start jetty on port 8888 you need to set the `jetty.port` property to 8888. In Eclipse I use the following configuration:

<div id="attachment_1190" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse.png"><img class="size-medium wp-image-1190" src="{{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse-300x148.png" alt="Run integration tests from Eclipse" width="300" height="148" srcset="{{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse-300x148.png 300w, {{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse.png 991w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Run integration tests from Eclipse
  </p>
</div>

&nbsp;

<h3 style="text-align: justify;">
  <span id="32_Live_tests">3.2. Live tests</span>
</h3>

<p style="text-align: justify;">
  In the following video I will will show how to test the API application from Chrome with the help of <a title="https://plus.google.com/104025798250320128549/posts" href="https://plus.google.com/104025798250320128549/posts" target="_blank">DEV HTTP Client</a>, which I highly recommend:
</p>



<h2 style="text-align: justify;">
  <span id="4_Summary">4. Summary</span>
</h2>

<p style="text-align: justify;">
  Well, that&#8217;s it. You&#8217;ve learned how to create a REST API with the help of Jersey 2, how it integrates with Spring with the new jersey-spring3 dependency and how to test the API with the Jersey Client and from Chrome with the help of Dev HTTP Client.
</p>

<p class="note_normal" style="text-align: justify;">
  If you&#8217;ve found some usefulnes in this post, I&#8217;d be very grateful if you helped it spread by leaving a comment or sharing it on Twitter, Google+ or Facebook. Thank you! Don&#8217;t forget also to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you&#8217;ll find for sure interesting podcasts and episodes. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

## <span id="5_Resources">5. Resources</span>

### <span id="51_Source_Code">5.1. Source Code</span>

  * <a title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis" href="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis" target="_blank">https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis</a>
  * [https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/tree/master/src/main/resources/input_data  ](https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/tree/master/src/main/resources/input_datahttp:// "https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis/tree/master/src/main/resources/input_data")(create db schema and restore data .sql files)

### <span id="52_Web_resources">5.2. Web resources</span>

  * <a href="http://en.wikipedia.org/wiki/Representational_State_Transfer" target="_blank">http://en.wikipedia.org/wiki/Representational_State_Transfer</a>
  * <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">http://en.wikipedia.org/wiki/Create,_read,_update_and_delete</a>
  * <a title="Java API for RESTful Services (JAX-RS)" href="https://jax-rs-spec.java.net/" target="_blank">Java API for RESTful Services (JAX-RS)</a>
  * <a title="Jersey REST" href="https://jersey.java.net/" target="_blank">Jersey &#8211; RESTful Web Services in Java </a>
  * <a title="Status Code Definitions" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP Status Code Definitions</a>
  * <a title="https://maven.apache.org/surefire/maven-failsafe-plugin/" href="https://maven.apache.org/surefire/maven-failsafe-plugin/" target="_blank">Maven Failsafe Plugin</a>
  * <a title="http://maven.apache.org/surefire/maven-failsafe-plugin/usage.html" href="http://maven.apache.org/surefire/maven-failsafe-plugin/usage.html" target="_blank">Maven Failsafe Plugin Usage</a>

### <span id="53_Codingpedia_resources">5.3. Codingpedia resources</span>

  * <a title="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate</a>
  * <a title="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" href="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" target="_blank">http://www.codingpedia.org/ama/spring-mybatis-integration-example/</a>
  * <a title="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" href="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" target="_blank">http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/</a>
  * <a title="http://www.codingpedia.org/ama/error-when-executing-jettyrun-with-jetty-maven-plugin-version-9-java-lang-unsupportedclassversionerror-unsupported-major-minor-version-51-0/" href="http://www.codingpedia.org/ama/error-when-executing-jettyrun-with-jetty-maven-plugin-version-9-java-lang-unsupportedclassversionerror-unsupported-major-minor-version-51-0/" target="_blank">http://www.codingpedia.org/ama/error-when-executing-jettyrun-with-jetty-maven-plugin-version-9-java-lang-unsupportedclassversionerror-unsupported-major-minor-version-51-0/</a>
  * <a title="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" href="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" target="_blank">http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/</a>

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
