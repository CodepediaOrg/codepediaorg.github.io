---
id: 1432
title: 'Tutorial &#8211; REST API design and implementation in Java with Jersey and Spring'
date: 2014-05-31T07:16:51+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1432
permalink: /ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 30
fsb_social_google:
  - 17
fsb_social_linkedin:
  - 4
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 5
dsq_thread_id:
  - 2725445707
categories:
  - java
  - spring
tags:
  - api
  - api design
  - GitHub
  - jersey
  - jersey2
  - open source
  - rest
  - rest api
  - spring
---
<p style="text-align: justify;">
  Looking to REST in Java? Then you&#8217;ve come to the right place, because in the blog post I will present you how to &#8220;beautifully&#8221; design a REST API and also, how to implement it in Java with the Jersey framework. The RESTful API developed in this tutorial will demonstrate a complete <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">Create,_read,_update_and_delete (CRUD)</a> functionality for <a title="https://github.com/Codingpedia/podcastpedia/podcasting" href="https://github.com/Codingpedia/podcastpedia/podcasting" target="_blank">podcast</a> resources stored in a MySql database.
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
                <a href="#131_Jersey_Facade">1.3.1. Jersey (Facade)</a>
              </li>
              <li>
                <a href="#132_Spring_Business_layer">1.3.2. Spring (Business layer)</a>
              </li>
              <li>
                <a href="#133_JPA_2_Hibernate_Persistence_layer">1.3.3. JPA 2 / Hibernate (Persistence layer)</a>
              </li>
              <li>
                <a href="#134_Web_Container">1.3.4. Web Container</a>
              </li>
              <li>
                <a href="#135_MySQL">1.3.5. MySQL</a>
              </li>
              <li>
                <a href="#136_Technology_versions">1.3.6. Technology versions</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#14_Source_code">1.4. Source code</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#2_Configuration">2. Configuration</a><ul>
          <li>
            <a href="#21_Project_dependencies">2.1. Project dependencies</a>
          </li>
          <li>
            <a href="#22_webxml">2.2. web.xml</a><ul>
              <li>
                <a href="#221_Jersey-servlet">2.2.1. Jersey-servlet</a><ul>
                  <li>
                    <a href="#2122_Spring_application_context_configuration">2.1.2.2. Spring application context configuration</a>
                  </li>
                </ul>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#3_The_REST_API_design_implementation">3. The REST API (design & implementation)</a><ul>
          <li>
            <a href="#31_Resources">3.1. Resources</a><ul>
              <li>
                <a href="#311_Design">3.1.1. Design</a>
              </li>
              <li>
                <a href="#312_Implementation">3.1.2. Implementation</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#32_Methods">3.2. Methods</a><ul>
              <li>
                <a href="#321_Create_podcasts">3.2.1. Create podcast(s)</a><ul>
                  <li>
                    <a href="#3211_Design">3.2.1.1. Design</a>
                  </li>
                  <li>
                    <a href="#3212_Implementation">3.2.1.2. Implementation</a><ul>
                      <li>
                        <a href="#32121_Create_a_single_resource_with_POST">3.2.1.2.1. Create a single resource with POST</a>
                      </li>
                      <li>
                        <a href="#32122_Create_a_single_resource_8220podcast8221_with_PUT">3.2.1.2.2. Create a single resource (&#8220;podcast&#8221;) with PUT</a>
                      </li>
                      <li>
                        <a href="#32123_Bonus_8211_Create_a_single_resource_8220podcast8221_from_form">3.2.1.2.3. Bonus &#8211; Create a single resource (&#8220;podcast&#8221;) from form</a>
                      </li>
                    </ul>
                  </li>
                </ul>
              </li>

              <li>
                <a href="#322_Read_podcasts">3.2.2. Read podcast(s)</a><ul>
                  <li>
                    <a href="#3221_Design">3.2.2.1. Design</a>
                  </li>
                  <li>
                    <a href="#3222_Implementation">3.2.2.2. Implementation</a><ul>
                      <li>
                        <a href="#32221_Read_all_podcasts_82208221">3.2.2.2.1. Read all podcasts (&#8220;/&#8221;)</a>
                      </li>
                      <li>
                        <a href="#32221_Read_one_podcast">3.2.2.2.1. Read one podcast</a>
                      </li>
                    </ul>
                  </li>
                </ul>
              </li>

              <li>
                <a href="#323_Update_podcast">3.2.3. Update podcast</a><ul>
                  <li>
                    <a href="#3231_Design">3.2.3.1. Design</a>
                  </li>
                  <li>
                    <a href="#3231_Implementation">3.2.3.1. Implementation</a><ul>
                      <li>
                        <a href="#32311_Full_Update">3.2.3.1.1. Full Update</a>
                      </li>
                      <li>
                        <a href="#32312_Partial_Update">3.2.3.1.2. Partial Update</a>
                      </li>
                    </ul>
                  </li>
                </ul>
              </li>

              <li>
                <a href="#324_Delete_podcast">3.2.4. Delete podcast</a><ul>
                  <li>
                    <a href="#3241_Design">3.2.4.1. Design</a>
                  </li>
                  <li>
                    <a href="#3242_Implementation">3.2.4.2. Implementation</a><ul>
                      <li>
                        <a href="#32421_Delete_all_resources">3.2.4.2.1. Delete all resources</a>
                      </li>
                      <li>
                        <a href="#32422_Delete_one_resource">3.2.4.2.2. Delete one resource</a>
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
        <a href="#4_Logging">4. Logging</a>
      </li>
      <li>
        <a href="#5_Exception_handling">5. Exception handling</a><ul>
          <li>
            <a href="#51_Custom_Reason_Phrase_in_HTTP_status_error_message_response_with_JAX-RS_Jersey">5.1. Custom Reason Phrase in HTTP status error message response with JAX-RS (Jersey)</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#6_Add_CORS_support_on_the_server_side">6. Add CORS support on the server side</a>
      </li>
      <li>
        <a href="#7_Testing">7. Testing</a><ul>
          <li>
            <a href="#71_Integration_tests_in_Java">7.1. Integration tests in Java</a><ul>
              <li>
                <a href="#711_Configuration">7.1.1. Configuration</a><ul>
                  <li>
                    <a href="#7111_Jersey_client_dependency">7.1.1.1 Jersey client dependency</a>
                  </li>
                  <li>
                    <a href="#7112_Failsafe_plugin">7.1.1.2. Failsafe plugin</a>
                  </li>
                  <li>
                    <a href="#7112_Jetty_Maven_Plugin">7.1.1.2. Jetty Maven Plugin</a>
                  </li>
                </ul>
              </li>

              <li>
                <a href="#712_Build_the_integration_tests">7.1.2. Build the integration tests</a>
              </li>
              <li>
                <a href="#713_Running_the_integration_tests">7.1.3. Running the integration tests</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#72_Integration_tests_with_SoapUI">7.2. Integration tests with SoapUI</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#8_Versioning">8. Versioning</a>
      </li>
      <li>
        <a href="#9_Compression">9. Compression</a>
      </li>
      <li>
        <a href="#10Quick_way_to_check_if_the_REST_API_is_alive">10. Quick way to check if the REST API is alive</a>
      </li>
      <li>
        <a href="#11_Security">11. Security</a><ul>
          <li>
            <a href="#111_Basic_Authentication">11.1. Basic Authentication</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#12_Summary">12. Summary</a>
      </li>
      <li>
        <a href="#Resources">Resources</a><ul>
          <li>
            <a href="#Source_Code">Source Code</a>
          </li>
          <li>
            <a href="#Web_resources">Web resources</a>
          </li>
          <li>
            <a href="#Codingpedia_related_resources">Codingpedia related resources</a>
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
  Before we start, let me tell you why I&#8217;ve written this post &#8211; well, my intention is to offer in the future a REST API for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>. Of course I could use <a title="Designing and Implementing RESTful Web Services with Spring" href="https://spring.io/guides/tutorials/rest/" target="_blank">Spring&#8217;s own REST implementation</a>, as I currently do for the AJAX calls, but I wanted also to see how the &#8220;official&#8221; implementation looks like. So, the best way to get to know the technology is to build a prototype with it. That is what I did and what I am presenting here, and I can say that I am pretty damn satisfied with Jersey. Read along to understand why!!!
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> You can visit my post <a title="Autocomplete search box with jQuery and Spring MVC" href="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" target="_blank">Autocomplete search box with jQuery and Spring MVC</a> to see how Spring handles REST requests.
</p>

<h3 style="text-align: justify;">
  <span id="12_What_does_it_do">1.2. What does it do?</span>
</h3>

<p style="text-align: justify;">
  The <a title="http://en.wikipedia.org/wiki/Web_resource" href="http://en.wikipedia.org/wiki/Web_resource" target="_blank">resource</a> managed in this tutorial are podcasts. The REST API will allow creation, retrieval, update and deletion of such resources.
</p>

<h3 style="text-align: justify;">
  <span id="13_Architecture_and_technologies">1.3. Architecture and technologies</span>
</h3>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/05/Rest-Demo-Diagram.png"><img class="alignnone size-full wp-image-1456" src="{{site.url}}/wp-content/uploads/2014/05/Rest-Demo-Diagram.png" alt="Architecture diagram" width="591" height="246" srcset="{{site.url}}/wp-content/uploads/2014/05/Rest-Demo-Diagram.png 591w, {{site.url}}/wp-content/uploads/2014/05/Rest-Demo-Diagram-300x124.png 300w" sizes="(max-width: 591px) 100vw, 591px" /></a>
</p>

<p style="text-align: justify;">
  The demo application uses a multi-layered architecture, based on the <em>&#8220;Law of Demeter (LoD) or principle of least knowledge&#8221;[16]</em>:
</p>

  * the **first layer** is the REST support implemented with Jersey, has the role of a <a title="Facade pattern" href="http://en.wikipedia.org/wiki/Facade_pattern" target="_blank">facade</a> and delegates the logic to the business layer
  * the **business layer** is where the logic happens
  * the **data access layer** is where the communcation with the pesistence storage (in our case the MySql database) takes place

A few words on the technologies/frameworks used:

<h4 style="text-align: justify;">
  <span id="131_Jersey_Facade">1.3.1. Jersey (Facade)</span>
</h4>

<p style="text-align: justify;">
  The <a title="https://jersey.java.net/" href="https://jersey.java.net/" target="_blank"><em>Jersey RESTful Web Services framework</em></a> is open source, production quality, framework for developing RESTful Web Services in Java that provides support for JAX-RS APIs and serves as a <a title="Java API for RESTful Services (JAX-RS)" href="https://jax-rs-spec.java.net/" target="_blank">JAX-RS</a> (JSR 311 & JSR 339) Reference Implementation.
</p>

<h4 style="text-align: justify;">
  <span id="132_Spring_Business_layer">1.3.2. Spring (Business layer)</span>
</h4>

<p style="text-align: justify;">
  I like glueing stuff together with <strong><a title="Spring Framework" href="http://projects.spring.io/spring-framework/" target="_blank">Spring</a></strong>, and this example makes no exception. In my opinion there&#8217;s no better way to make POJOs with different functionalities. You&#8217;ll find out in the tutorial what it takes to integrate Jersey 2 with Spring.
</p>

<h4 style="text-align: justify;">
  <span id="133_JPA_2_Hibernate_Persistence_layer">1.3.3. JPA 2 / Hibernate (Persistence layer)</span>
</h4>

<p style="text-align: justify;">
  For the persistence layer I still use a DAO pattern, even though for implementing it I am using JPA 2, which, as some people say, should make DAOs superfluous (I, for one, don’t like my service classes cluttered with EntityManager/JPA specific code). AS supporting framework for JPA 2 I am using Hibernate.
</p>

<p style="text-align: justify;">
  See my post <a title="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" href="http://www.codingpedia.org/ama/java-persistence-example-with-spring-jpa2-and-hibernate/" target="_blank">Java Persistence Example with Spring, JPA2 and Hibernate</a> for an interesting discussion around persistence thema in Java.
</p>

<h4 style="text-align: justify;">
  <span id="134_Web_Container">1.3.4. Web Container</span>
</h4>

<p style="text-align: justify;">
  Everything gets packaged with Maven as a <code>.war</code> file and can be deployed on any web container &#8211; I used <a title="Apache Tomcat" href="http://tomcat.apache.org/" target="_blank">Tomcat</a> and <a title="Jetty" href="http://www.eclipse.org/jetty/" target="_blank">Jetty</a> but, it could also be Glassfih, Weblogic, JBoss or WebSphere.
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

#### <span id="136_Technology_versions">1.3.6. Technology versions</span>

  1. Jersey 2.9
  2. Spring 4.0.3
  3. Hibernate 4
  4. Maven 3
  5. Tomcat 7
  6. Jetty 9
  7. MySql 5.6

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The main focus in the post will be on the REST api design and its implementation with the Jersey JAX-RS implementation, all the other technologies/layers are considered as enablers.
</p>

### <span id="14_Source_code">1.4. Source code</span>

The source code for the project presented here is available on GitHub, with complete instructions on how to install and run the project:

  * <a title="Source code for project" href="https://github.com/Codingpedia/demo-rest-jersey-spring" target="_blank">Codingpedia / demo-rest-jersey-spring </a>

## <span id="2_Configuration">2. Configuration</span>

Before I start presenting the design and implementation of the REST API, we need to do a little configuration so that all these wonderful technologies can come and play together

### <span id="21_Project_dependencies">2.1. Project dependencies</span>

<p style="text-align: justify;">
  The <a title="https://jersey.java.net/documentation/latest/spring.html" href="https://jersey.java.net/documentation/latest/spring.html" target="_blank">Jersey Spring extension</a> must be present in your project&#8217;s classpath. If you are using Maven add it to the <code>pom.xml</code> file of your project:
</p>

<pre><code class="xml">&lt;dependency&gt;
        &lt;groupId&gt;org.glassfish.jersey.ext&lt;/groupId&gt;
        &lt;artifactId&gt;jersey-spring3&lt;/artifactId&gt;
        &lt;version&gt;${jersey.version}&lt;/version&gt;
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
  <strong>Note:</strong> The jersey-spring3.jar, uses its own version for Spring libraries, so to use the ones you want (Spring 4.0.3.Release in this case), you need to exclude these libraries manually.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> If you want to see what other dependencies are needed (e.g. Spring, Hibernate, Jetty maven plugin, testing etc.) in the project you can have a look at the the complete <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/pom.xml" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/pom.xml" target="_blank">pom.xml</a> file available on GitHub.
</p>

### <span id="22_webxml">2.2. web.xml</span>
<pre><code class="xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
    &lt;web-app version=&quot;3.0&quot; xmlns=&quot;http://java.sun.com/xml/ns/javaee&quot;
    	xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
    	xsi:schemaLocation=&quot;http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd&quot;&gt;
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
    			&lt;param-value&gt;org.codingpedia.demo.rest.RestDemoJaxRsApplication&lt;/param-value&gt;
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
    &lt;/web-app&gt;
  </code>
</pre>

#### <span id="221_Jersey-servlet">2.2.1. Jersey-servlet</span>

<p style="text-align: justify;">
  Notice the Jersey servlet configuration [lines 18-33]. The <code>javax.ws.rs.core.Application</code> class defines the components (root resource and provider classes,) of the JAX-RS application. I used <code> ResourceConfig,</code> which is Jersey&#8217;s own implementation of the class <code>Application</code>, and which provides advanced capabilites to simplify registration of JAX-RS components.<code> </code><code class="literal"></code>Check out the <a title="https://jersey.java.net/documentation/latest/user-guide.html#environmenmt.appmodel" href="https://jersey.java.net/documentation/latest/user-guide.html#environmenmt.appmodel" target="_blank">JAX-RS Application Model</a> in the documentation for more possibilities.
</p>

My implementation of the `ResourceConfig` class, ``<p style="text-align: justify;">
  Looking to REST in Java? Then you&#8217;ve come to the right place, because in the blog post I will present you how to &#8220;beautifully&#8221; design a REST API and also, how to implement it in Java with the Jersey framework. The RESTful API developed in this tutorial will demonstrate a complete <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">Create,_read,_update_and_delete (CRUD)</a> functionality for <a title="https://github.com/Codingpedia/podcastpedia/podcasting" href="https://github.com/Codingpedia/podcastpedia/podcasting" target="_blank">podcast</a> resources stored in a MySql database.
</p>

My implementation of the `ResourceConfig` class,`` registers application resources, filters, exception mappers and feature :

<pre><code class="java">package org.codingpedia.demo.rest.service;

    //imports omitted for brevity

    /**
     * Registers the components to be used by the JAX-RS application
     *
     * @author ama
     *
     */
    public class RestDemoJaxRsApplication extends ResourceConfig {

    	/**
    	 * Register JAX-RS application components.
    	 */
    	public RestDemoJaxRsApplication() {
    		// register application resources
    		register(PodcastResource.class);
    		register(PodcastLegacyResource.class);

    		// register filters
    		register(RequestContextFilter.class);
    		register(LoggingResponseFilter.class);
    		register(CORSResponseFilter.class);

    		// register exception mappers
    		register(GenericExceptionMapper.class);
    		register(AppExceptionMapper.class);
    		register(NotFoundExceptionMapper.class);

    		// register features
    		register(JacksonFeature.class);
    		register(MultiPartFeature.class);
    	}
    }</code>
</pre>

Please note the

<li style="text-align: justify;">
  <code>org.glassfish.jersey.server.spring.scope.RequestContextFilter</code>, which is a Spring filter that provides a bridge between JAX-RS and Spring request attributes
</li>
<li style="text-align: justify;">
  <code>org.codingpedia.demo.rest.resource.PodcastsResource</code>, which is the &#8220;facade&#8221; component that exposes the REST API via annotations and will be thouroughly presented later in the post
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

    	&lt;bean id="podcastDao" class="org.codingpedia.demo.rest.dao.PodcastDaoJPA2Impl"/&gt;
        &lt;bean id="podcastService" class="org.codingpedia.demo.rest.service.PodcastServiceDbAccessImpl" /&gt;
        &lt;bean id="podcastsResource" class="org.codingpedia.demo.rest.resource.PodcastsResource" /&gt;
        &lt;bean id="podcastLegacyResource" class="org.codingpedia.demo.rest.resource.PodcastLegacyResource" /&gt;

    	&lt;bean id="restDemoDS" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
    	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoDB" /&gt;
    	    &lt;property name="resourceRef" value="true" /&gt;
    	&lt;/bean&gt;
    	&lt;bean id="restDemoLegacyDS" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
    	    &lt;property name="jndiName" value="java:comp/env/jdbc/restDemoLegacyDB" /&gt;
    	    &lt;property name="resourceRef" value="true" /&gt;
    	&lt;/bean&gt;
    &lt;/beans&gt;
  </code>
</pre>

<p style="text-align: justify;">
  Nothing special here, it just defines the beans that are needed throughout the demo application (e.g. <code>podcastsResource</code> which is the entry point class for our REST API).
</p>

## <span id="3_The_REST_API_design_implementation">3. The REST API (design & implementation)</span>

### <span id="31_Resources">3.1. Resources</span>

#### <span id="311_Design">3.1.1. Design</span>

<p style="text-align: justify;">
  As mentioned earlier, the demo application manages podcasts, which represent the <a title="Wikipedia - Web resource" href="http://en.wikipedia.org/wiki/Web_resource" target="_blank">resource</a> in our REST API. Resources are the central concept in REST and are characterized by two main things:
</p>

  * each is referenced with a global identifier (e.g. a [URI](http://en.wikipedia.org/wiki/Uniform_resource_identifier "Uniform resource identifier") in HTTP).
  * has one or more representations, that they expose to the outer world and can be manipulated with (we&#8217;ll be working mostly with JSON representations in this example)

Resources are usually represented in REST by nouns (podcasts, customers, user, accounts etc.) and not verbs (getPodcast, deleteUser etc.)

The endpoints used throughout the tutorial are :

  * `/podcasts` &#8211; _(notice the plural)_ URI identifying a **resource** representing a collection of podcasts
  * `/podcasts/{id}` &#8211; URI identifying a podcast resource, by the podcast&#8217;s id

#### <span id="312_Implementation">3.1.2. Implementation</span>

For the sake of simplicity, a podcast will have only the following properties:

  * `id` &#8211; uniquely identifies the podcast
  * `feed` &#8211; url feed of the podcast
  * `title` &#8211; title of the podcast
  * `linkOnPodcastpedia` &#8211; where you can find the podcast on Podcastpedia.org
  * `description` &#8211; a short description of the podcast

<p style="text-align: justify;">
  I could have used only one Java class for the representation of the podcast resource in the code, but in that case the class and its properties/methods would have gotten cluttered with both JPA and XML/JAXB/JSON annotations. I wanted to avoid that and I used two representations which have pretty much the same properties instead:
</p>

  * <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/dao/PodcastEntity.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/dao/PodcastEntity.java" target="_blank">PodcastEntity.java</a> &#8211; JPA annotated class used in the DB and business layers
  * <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/resource/Podcast.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/resource/Podcast.java" target="_blank">Podcast.java</a> &#8211; JAXB/JSON annotated class used in the facade and business layers

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> I am still trying to convince myself that this is the better approach, so if you have a suggestion on this please leave a comment.
</p>

The <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/resource/Podcast.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/resource/Podcast.java" target="_blank">Podcast.java</a> classs look something like the following:

<pre><code class="java">package org.codingpedia.demo.rest.resource;

    //imports omitted for brevity

    /**
     * Podcast resource placeholder for json/xml representation
     *
     * @author ama
     *
     */
    @SuppressWarnings("restriction")
    @XmlRootElement
    @XmlAccessorType(XmlAccessType.FIELD)
    public class Podcast implements Serializable {

        private static final long serialVersionUID = -8039686696076337053L;

        /** id of the podcast */
        @XmlElement(name = "id")    
        private Long id;
        
        /** title of the podcast */
        @XmlElement(name = "title")    
        private String title;
            
        /** link of the podcast on Podcastpedia.org */
        @XmlElement(name = "linkOnPodcastpedia")    
        private String linkOnPodcastpedia;
        
        /** url of the feed */
        @XmlElement(name = "feed")    
        private String feed;
        
        /** description of the podcast */
        @XmlElement(name = "description")
        private String description;
            
        /** insertion date in the database */
        @XmlElement(name = "insertionDate")
        @XmlJavaTypeAdapter(DateISO8601Adapter.class)    
        @PodcastDetailedView
        private Date insertionDate;

        public Podcast(PodcastEntity podcastEntity){
            try {
                BeanUtils.copyProperties(this, podcastEntity);
            } catch (IllegalAccessException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (InvocationTargetException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        
        public Podcast(String title, String linkOnPodcastpedia, String feed,
                String description) {
            
            this.title = title;
            this.linkOnPodcastpedia = linkOnPodcastpedia;
            this.feed = feed;
            this.description = description;
            
        }
        
        public Podcast(){}

    //getters and setters now shown for brevity
    }
  </code>
</pre>

and translates into the following JSON representation, which is actually the de facto media type used with REST nowadays:

<pre><code class="json">{
    	"id":1,
    	"title":"Quarks & Co - zum Mitnehmen-modified",
    	"linkOnPodcastpedia":"https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen",
    	"feed":"http://podcast.wdr.de/quarks.xml",
    	"description":"Quarks & Co: Das Wissenschaftsmagazin",
    	"insertionDate":"2014-05-30T10:26:12.00+0200"
    }</code>
</pre>

<p class="note_normal" style="text-align: justify;">
  Even though JSON is becoming more and more the preffered representation in REST APIs, you shouldn&#8217;t neglect the XML representation, as most of the systems still use XML format for communication with other parties.
</p>

The good thing is that in Jersey you can kill two rabbits with one shot &#8211; with JAXB beans (as used above) you will be able to use the same Java model to generate JSON as well as XML representations. Another advantage is simplicity of working with such a model and availability of the API in Java SE Platform.

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> Most of the methods defined in this tutorial will produce and consume also the application/xml media type, with application/json being the preferred way.
</p>

### <span id="32_Methods">3.2. Methods</span>

<p style="text-align: justify;">
  Before I present you the API, let me to tell you that
</p>

  * Create = POST
  * Read = GET
  * Update = PUT
  * Delete = DELETE

and is not a strict 1:1 mapping. Why? Because you can also use PUT for Creation and POST for Update. This will be explained and demonstrated in the coming paragraphs.

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> For Read and Delete it is pretty clear, they map indeed one to one with the GET and DELETE HTTP operations. Anyway REST is an architectural style, is not a specification and you should adapt the architecture to your needs, but if you want to make your API public and have somebody willing to use it, you should follow some &#8220;best practices&#8221;.
</p>

As already mentioned the `PodcastRestResource` class is the one handling all the rest requests:

<pre><code class="java">
    package org.codingpedia.demo.rest.resource;
    //imports
    ......................
    @Component
    @Path("/podcasts")
    public class PodcastResource {
        @Autowired
        private PodcastService podcastService;
        .....................
    }
  </code>
</pre>

<p style="text-align: justify;">
  Notice the <code>@Path("/podcasts")</code> before the class definition &#8211; everything related to podcast resources will occur under this path. The <a href="http://jax-rs-spec.java.net/nonav/2.0/apidocs/javax/ws/rs/Path.html" target="_top">@Path</a> annotation&#8217;s value is a relative URI path. In the example above, the Java class will be hosted at the URI path <code>/podcasts</code>. The <code>PodcastService</code> interface exposes the business logic to the REST facade layer.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code alert:</strong> You can find the entire content of the class on GitHub &#8211; <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/resource/PodcastResource.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/resource/PodcastResource.java" target="_blank">PodcastResource.java</a>. We&#8217;ll be going through the file step by step and explain the different methods corresponding to the different operations.
</p>

#### <span id="321_Create_podcasts">3.2.1. Create podcast(s)</span>

##### <span id="3211_Design">3.2.1.1. Design</span>

While the &#8220;most known&#8221; way for resource creation is by using POST, As mentioned before to create a new resource I could use both the POST and PUT methods, and I did just that:

<table border="1" cellspacing="0" cellpadding="5">
  <tr valign="TOP">
    <td bgcolor="#808080" width="25%">
      <b>  Description</b>
    </td>

    <td bgcolor="#808080" width="25%">
      <b>  URI</b>
    </td>

    <td bgcolor="#808080" width="25%">
      <b>  HTTP method<br /> </b>
    </td>

    <td bgcolor="#808080" width="25%">
      <b>  HTTP Status response</b>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Add new podcast
    </td>

    <td width="25%">
       /podcasts/
    </td>

    <td width="25%">
      <p align="CENTER">
        POST
      </p>
    </td>

    <td width="25%">
      <p align="CENTER">
        201 Created
      </p>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Add new podcast (all values must be sent)
    </td>

    <td width="25%">
       /podcasts/{id}
    </td>

    <td width="25%">
      <p align="CENTER">
        PUT
      </p>
    </td>

    <td width="25%">
      <p align="CENTER">
        201 Created
      </p>
    </td>
  </tr>
</table>

The big difference between using POST (not idempotent)

<p style="padding-left: 30px; text-align: justify;">
  <em>&#8220;The POST method is used to request that the origin server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line[&#8230;] If a resource has been created on the origin server, the response SHOULD be 201 (Created) and contain an entity which describes the status of the request and refers to the new resource, and a Location header&#8221; [1]</em>
</p>

and PUT (idempotent)

<p style="padding-left: 30px; text-align: justify;">
  <em>&#8220;The PUT method requests that the enclosed entity be stored under the supplied Request-URI [&#8230;] If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI. If a new resource is created, the origin server MUST inform the user agent via the 201 (Created) response.&#8221; [1]</em>
</p>

, is that for PUT you should know beforehand the location where the resource will be created and send all the possible values of the entry.

##### <span id="3212_Implementation">3.2.1.2. Implementation</span>

###### <span id="32121_Create_a_single_resource_with_POST">3.2.1.2.1. Create a single resource with POST</span>

<pre><code class="java">/**
     * Adds a new resource (podcast) from the given json format (at least title
     * and feed elements are required at the DB level)
     *
     * @param podcast
     * @return
     * @throws AppException
     */
    @POST
    @Consumes({ MediaType.APPLICATION_JSON })
    @Produces({ MediaType.TEXT_HTML })
    public Response createPodcast(Podcast podcast) throws AppException {
    	Long createPodcastId = podcastService.createPodcast(podcast);
    	return Response.status(Response.Status.CREATED)// 201
    			.entity("A new podcast has been created")
    			.header("Location",
    					"http://localhost:8888/demo-rest-jersey-spring/podcasts/"
    							+ String.valueOf(createPodcastId)).build();
    }</code></pre>

**Annotations**

  * `@POST` &#8211; indicates that the method responds to HTTP POST requests
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
  * `@Produces({MediaType.TEXT_HTML})` &#8211; defines the media type) that the method can produce, in this case `"text/html"`.

**<span style="line-height: 1.5;">Response</span>**

  * on success: text/html document, with a HTTP status of `201 Created`, and a Location header specifying where the resource has been created
  * on error:
      * `400 Bad request` if not enough data is provided
      * `409 Conflict` if on the server side is determined a podcast with the same feed exists

###### <span id="32122_Create_a_single_resource_8220podcast8221_with_PUT">3.2.1.2.2. Create a single resource (&#8220;podcast&#8221;) with PUT</span>

This will be treated in the Update Podcast section below.

###### <span id="32123_Bonus_8211_Create_a_single_resource_8220podcast8221_from_form">3.2.1.2.3. Bonus &#8211; Create a single resource (&#8220;podcast&#8221;) from form</span>

<pre><code class="java">/**
   * Adds a new podcast (resource) from "form" (at least title and feed
   * elements are required at the DB level)
   *
   * @param title
   * @param linkOnPodcastpedia
   * @param feed
   * @param description
   * @return
   * @throws AppException
   */
  @POST
  @Consumes({ MediaType.APPLICATION_FORM_URLENCODED })
  @Produces({ MediaType.TEXT_HTML })
  @Transactional
  public Response createPodcastFromApplicationFormURLencoded(
  		@FormParam("title") String title,
  		@FormParam("linkOnPodcastpedia") String linkOnPodcastpedia,
  		@FormParam("feed") String feed,
  		@FormParam("description") String description) throws AppException {

  	Podcast podcast = new Podcast(title, linkOnPodcastpedia, feed,
  			description);
  	Long createPodcastid = podcastService.createPodcast(podcast);

  	return Response
  			.status(Response.Status.CREATED)// 201
  			.entity("A new podcast/resource has been created at /demo-rest-jersey-spring/podcasts/"
  					+ createPodcastid)
  			.header("Location",
  					"http://localhost:8888/demo-rest-jersey-spring/podcasts/"
  							+ String.valueOf(createPodcastid)).build();
  }</code>
</pre>

**Annotations**

  * `@POST` &#8211; indicates that the method responds to HTTP POST requests
<li style="text-align: justify;">
  <code>@Consumes({MediaType.APPLICATION_FORM_URLENCODED})</code>&#8211; defines the media type, the method accepts, in this case<code>"application/x-www-form-urlencoded"</code> <ul>
    <li style="text-align: justify;">
      <code>@FormParam</code> &#8211; present before the input parameters of the method, this annotation binds the value(s) of a form parameter contained within a request entity body to a resource method parameter. Values are URL decoded unless this is disabled using the <code>Encoded</code> annotation
    </li>
  </ul>
</li>

<li style="text-align: justify;">
  <code>@Produces({MediaType.TEXT_HTML})</code> &#8211; defines the media type that the method can produce, in this case &#8220;text/html&#8221;. The response will be a html document, with a status of 201, indicating to the caller that the request has been fulfilled and resulted in a new resource being created.
</li>

**Response**

  * on success: text/html document, with a HTTP status of `201 Created`, and a Location header specifying where the resource has been created
  * on error:
      * `400 Bad request` if not enough data is provided
      * `409 Conflict` if on the server side is determined a podcast with the same feed exists

#### <span id="322_Read_podcasts">3.2.2. Read podcast(s)</span>

##### <span id="3221_Design">3.2.2.1. Design</span>

The API supports two Read operations:

  * return a collection of podcasts
  * return a podcast identified by id

<table border="1" cellspacing="0" cellpadding="5">
  <tr valign="TOP">
    <td bgcolor="#808080" width="25%">
      <b> Description</b>
    </td>

    <td bgcolor="#808080" width="45%">
      <b> URI</b>
    </td>

    <td bgcolor="#808080" width="15%">
      <b> HTTP method<br /> </b>
    </td>

    <td bgcolor="#808080" width="15%">
      <b> HTTP Status response</b>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Return all podcasts
    </td>

    <td width="45%">
       /podcasts/?orderByInsertionDate={ASC|DESC}&numberDaysToLookBack={val}
    </td>

    <td width="15%">
      <p align="CENTER">
        GET
      </p>
    </td>

    <td width="15%">
      <p align="CENTER">
        200 OK
      </p>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Add new podcast (all values must be sent)
    </td>

    <td width="45%">
       /podcasts/{id}
    </td>

    <td width="15%">
      <p align="CENTER">
        GET
      </p>
    </td>

    <td width="15%">
      <p align="CENTER">
        200 OK
      </p>
    </td>
  </tr>
</table>

<p style="text-align: justify;">
  Notice the query parameters for the collection resource &#8211; <span style="color: #000000;">orderByInsertionDate and numberDaysToLookBack. It makes perfect sense to add filters as query parameters in the URI and not be part of the path.</span>
</p>

##### <span id="3222_Implementation">3.2.2.2. Implementation</span>

###### <span id="32221_Read_all_podcasts_82208221">3.2.2.2.1. Read all podcasts (&#8220;/&#8221;)</span>

<pre><code class="java">/**
     * Returns all resources (podcasts) from the database
     *
     * @return
     * @throws IOException
     * @throws JsonMappingException
     * @throws JsonGenerationException
     * @throws AppException
     */
    @GET
    @Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
    public List&lt;Podcast&gt; getPodcasts(
    		@QueryParam("orderByInsertionDate") String orderByInsertionDate,
    		@QueryParam("numberDaysToLookBack") Integer numberDaysToLookBack)
    		throws JsonGenerationException, JsonMappingException, IOException,
    		AppException {
    	List&lt;Podcast&gt; podcasts = podcastService.getPodcasts(
    			orderByInsertionDate, numberDaysToLookBack);
    	return podcasts;
    }</code></pre>

**Annotations**

  *`@GET` &#8211; indicates that the method responds to HTTP GET requests
  * `@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})` &#8211; defines the media type) that the method can produce, in this case either `"application/json"` or `"application/xml"`(you need the `@XmlRootElement` in front of the `Podcast` class ). The response will be a list of podcasts either in JSON or XML format.

**Response**

  * list of podcasts from the database and a HTTP Status of 200 OK

###### <span id="32221_Read_one_podcast">3.2.2.2.1. Read one podcast</span>

<pre><code class="java">@GET
    @Path("{id}")
    @Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
    public Response getPodcastById(@PathParam("id") Long id)
    		throws JsonGenerationException, JsonMappingException, IOException,
    		AppException {
    	Podcast podcastById = podcastService.getPodcastById(id);
    	return Response.status(200).entity(podcastById)
    			.header("Access-Control-Allow-Headers", "X-extra-header")
    			.allow("OPTIONS").build();
    }</code></pre>

**Annotations**

  * `@GET` &#8211; indicates that the method responds to HTTP GET requests
  * `@Path("{id}")` &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the `@PathParam` variable.
    <li style="text-align: justify;">
      <code>@PathParam("id")</code> &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the <code>@Encoded</code> annotation. A default value can be specified using the <code>@DefaultValue</code> annotation.
    </li>
<li style="text-align: justify;">
  <code>@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})</code> &#8211; defines the media type) that the method can produce, in this case <code>"application/json"</code> or <code>"application/xml"</code>(you need the <code>@XmlRootElement</code> in front of the Podcast class ).
</li>

**Response**

  * on success: requested podcast with a `200 OK` HTTP status. The format is either xml or JSON, depending on the Accept -header&#8217;s value sent by the client (might bet application/xml or application/json)
  * on error: `404 Not found` if the podcast with the given id does not exist in the database

#### <span id="323_Update_podcast">3.2.3. Update podcast</span>

##### <span id="3231_Design">3.2.3.1. Design</span>

<table border="1" cellspacing="0" cellpadding="5">
  <tr valign="TOP">
    <td bgcolor="#808080" width="25%">
      <b>Description</b>
    </td>

    <td bgcolor="#808080" width="45%">
      <b>URI</b>
    </td>

    <td bgcolor="#808080" width="15%">
      <b>HTTP method<br /> </b>
    </td>

    <td bgcolor="#808080" width="15%">
      <b>HTTP Status response</b>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Update podcast (<strong>fully</strong>)
    </td>

    <td width="45%">
       /podcasts/{id}
    </td>

    <td width="15%">
      <p align="CENTER">
        PUT
      </p>
    </td>

    <td width="15%">
      <p align="CENTER">
        200 OK
      </p>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Update podcast (<strong>partially</strong>)
    </td>

    <td width="45%">
       /podcasts/{id}
    </td>

    <td width="15%">
      <p align="CENTER">
        POST
      </p>
    </td>

    <td width="15%">
      <p align="CENTER">
        200 OK
      </p>
    </td>
  </tr>
</table>

In the REST arena you will be doing two kind of updates:

  1. full updates &#8211; that is where you will provide all the
  2. partial updates &#8211; when only some properties will be sent over the wire for update

For full updates, it&#8217;s pretty clear you can use the PUT method and you are conform the method&#8217;s specification in the <a title="http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.6" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.6" target="_blank">RFC 2616</a>.

Now for the partial update there&#8217;s a bunch of proposals/debate on what to use:

  1. via PUT
  2. via POST
  3. via PATCH

<p style="text-align: justify;">
  Let me tell why I consider the <strong>first option (with PUT)</strong> is a NO GO. Well, accordingly to the specification
</p>

<p style="padding-left: 30px; text-align: justify;">
  &#8220;<span style="color: #000000;">If the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server.</span>&#8220;[1]
</p>

if I would like to update just the title property of the podcast with the id 2

<pre><code class="http">PUT http://localhost:8888/demo-rest-jersey-spring/podcasts/2 HTTP/1.1
    Accept-Encoding: gzip,deflate
    Content-Type: application/json
    Content-Length: 155
    Host: localhost:8888
    Connection: Keep-Alive
    User-Agent: Apache-HttpClient/4.1.1 (java 1.5)

    {
    	"title":"New Title"
    }</code>
</pre>

<p style="text-align: justify;">
  then, accordingly to the specification the resource &#8220;stored&#8221; at the location should have only id and title, and clearly my intent was not that.
</p>

<p style="text-align: justify;">
  The <strong>second option via POST&#8230; </strong>well we can &#8220;abuse&#8221; this one and that is exactly what I did in the implementation, but it does not seem conform to me, because the spec for POST states:
</p>

<p style="padding-left: 30px;">
  <em>&#8220;<span style="color: #000000;">The posted entity is subordinate to that URI in the same way that a file is subordinate to a directory containing it, a news article is subordinate to a newsgroup to which it is posted, or a record is subordinate to a database.</span>&#8220;[1]</em>
</p>

That does not look like a partial update case to me&#8230;

<p style="text-align: justify;">
  The <strong>third option is to use PATCH,</strong>  and I guess this is the main reason the method came to life:
</p>

<p style="padding-left: 30px;">
  <em>&#8220;Several applications extending the Hypertext Transfer Protocol (HTTP)</em><br /> <em>   require a feature to do partial resource modification.  The existing</em><br /> <em>   HTTP PUT method only allows a complete replacement of a document.</em><br /> <em>   This proposal adds a new HTTP method, PATCH, to modify an existing</em><br /> <em>   HTTP resource.&#8221;[2]</em>
</p>

<p style="text-align: justify;">
  I am pretty sure this will be used in the future for partial updates, but since is not yet part of the specification and not yet implemented in Jersey I chose to use the second option with POST for this demo. If you really want to implement partial update in Java with the PATCH check out this post  &#8211; <a title="http://kingsfleet.blogspot.co.uk/2014/02/transparent-patch-support-in-jax-rs-20.html" href="http://kingsfleet.blogspot.co.uk/2014/02/transparent-patch-support-in-jax-rs-20.html" target="_blank">Transparent PATCH support in JAX-RS 2.0</a>
</p>

##### <span id="3231_Implementation">3.2.3.1. Implementation</span>

###### <span id="32311_Full_Update">3.2.3.1.1. Full Update</span>

<pre><code class="java">@PUT
    @Path("{id}")
    @Consumes({ MediaType.APPLICATION_JSON })
    @Produces({ MediaType.TEXT_HTML })
    public Response putPodcastById(@PathParam("id") Long id, Podcast podcast)
    		throws AppException {

    	Podcast podcastById = podcastService.verifyPodcastExistenceById(id);

    	if (podcastById == null) {
    		// resource not existent yet, and should be created under the
    		// specified URI
    		Long createPodcastId = podcastService.createPodcast(podcast);
    		return Response
    				.status(Response.Status.CREATED)
    				// 201
    				.entity("A new podcast has been created AT THE LOCATION you specified")
    				.header("Location",
    						"http://localhost:8888/demo-rest-jersey-spring/podcasts/"
    								+ String.valueOf(createPodcastId)).build();
    	} else {
    		// resource is existent and a full update should occur
    		podcastService.updateFullyPodcast(podcast);
    		return Response
    				.status(Response.Status.OK)
    				// 200
    				.entity("The podcast you specified has been fully updated created AT THE LOCATION you specified")
    				.header("Location",
    						"http://localhost:8888/demo-rest-jersey-spring/podcasts/"
    								+ String.valueOf(id)).build();
    	}
    }</code>
</pre>

**Annotations**

  * `@PUT `&#8211; indicates that the method responds to HTTP PUT requests
  * `@Path("{id}")` &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the `@PathParam` variable.
      * `@PathParam("id")` &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the `@Encoded` annotation. A default value can be specified using the `@DefaultValue` annotation.
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
  * `@Produces({MediaType.TEXT_HTML})` &#8211; defines the media type) that the method can produce, in this case &#8220;text/html&#8221;.

will be a html document containing different messages and stati depending on what action has been taken

**Respsonse**

  * on creation

  * on success: `201 Created` and in the Location header the specified location where the resource was created
  * on error: `400 Bad request` if the minimum required properties are not provided for insertion

  * on full update
      * on success: `200 OK`
      * on error: `400 Bad Request` if not all properties are provided

###### <span id="32312_Partial_Update">3.2.3.1.2. Partial Update</span>

<pre><code class="java">//PARTIAL update
    @POST
    @Path("{id}")
    @Consumes({ MediaType.APPLICATION_JSON })
    @Produces({ MediaType.TEXT_HTML })
    public Response partialUpdatePodcast(@PathParam("id") Long id, Podcast podcast) throws AppException {
    	podcast.setId(id);
    	podcastService.updatePartiallyPodcast(podcast);
    	return Response.status(Response.Status.OK)// 200
    			.entity("The podcast you specified has been successfully updated")
    			.build();
    }</code></pre>

**Annotations**

  * `<code>@POST`</code> &#8211; indicates that the method responds to HTTP POST requests
  * `@Path("{id}")` &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the `@PathParam` variable.
      * `@PathParam("id")` &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the `@Encoded` annotation. A default value can be specified using the `@DefaultValue` annotation.
  * `@Consumes({MediaType.APPLICATION_JSON})` &#8211; defines the media type, the method accepts, in this case `"application/json"`
  * `@Produces({MediaType.TEXT_HTML})` &#8211; defines the media type) that the method can produce, in this case `"text/html"`.

**<span style="line-height: 1.5;">Response</span>**

  * on success: `200 OK`
  * on error: `404 Not Found`, if there is no resource anymore available at the provided location

#### <span id="324_Delete_podcast">3.2.4. Delete podcast</span>

##### <span id="3241_Design">3.2.4.1. Design</span>

<table border="1" cellspacing="0" cellpadding="5">
  <tr valign="TOP">
    <td bgcolor="#808080" width="25%">
      <b>Description</b>
    </td>

    <td bgcolor="#808080" width="45%">
      <b>URI</b>
    </td>

    <td bgcolor="#808080" width="15%">
      <b>HTTP method<br /> </b>
    </td>

    <td bgcolor="#808080" width="15%">
      <b>HTTP Status response</b>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Removes all podcasts
    </td>

    <td width="45%">
       /podcasts/
    </td>

    <td width="15%">
      <p align="CENTER">
        DELETE
      </p>
    </td>

    <td width="15%">
      <p align="CENTER">
        204 No content
      </p>
    </td>
  </tr>

  <tr>
    <td width="25%" height="19">
       Removes podcast at the specified location
    </td>

    <td width="45%">
       /podcasts/{id}
    </td>

    <td width="15%">
      <p align="CENTER">
        DELETE
      </p>
    </td>

    <td width="15%">
      <p align="CENTER">
        204 No content
      </p>
    </td>
  </tr>
</table>

##### <span id="3242_Implementation">3.2.4.2. Implementation</span>

###### <span id="32421_Delete_all_resources">3.2.4.2.1. Delete all resources</span>

<pre><code class="java">@DELETE
    @Produces({ MediaType.TEXT_HTML })
    public Response deletePodcasts() {
    	podcastService.deletePodcasts();
    	return Response.status(Response.Status.NO_CONTENT)// 204
    			.entity("All podcasts have been successfully removed").build();
    }</code></pre>

**Annotations**

  * `@DELETE` &#8211; indicates that the method responds to HTTP DELETE requests
  * `@Produces({MediaType.TEXT_HTML})` &#8211; defines the media type that the method can produce, in this case &#8220;text/html&#8221;.

**Response**

  * The response will be a html document, with a status of 204 No content, indicating to the caller that the request has been fulfilled.

###### <span id="32422_Delete_one_resource">3.2.4.2.2. Delete one resource</span>

<pre>
  <code class="java">@DELETE
    @Path("{id}")
    @Produces({ MediaType.TEXT_HTML })
    public Response deletePodcastById(@PathParam("id") Long id) {
    	podcastService.deletePodcastById(id);
    	return Response.status(Response.Status.NO_CONTENT)// 204
    			.entity("Podcast successfully removed from database").build();
    }</code></pre>

**Annotations**

  * `@DELETE` &#8211; indicates that the method responds to HTTP DELETE requests
<li style="text-align: justify;">
  <code>@Path("{id}")</code> &#8211; identifies the URI path that the class method will serve requests for. The &#8220;id&#8221; value is an embedded variable making an URI path template. It is used in combination with the <code>@PathParam</code> variable. <ul>
    <li style="text-align: justify;">
      <code>@PathParam("id")</code> &#8211; binds the value of a URI template parameter (&#8220;id&#8221;) to the resource method parameter. The value is URL decoded unless this is di sabled using the <code>@Encoded</code> annotation. A default value can be specified using the <code>@DefaultValue</code> annotation.
    </li>
  </ul>
</li>

<li style="text-align: justify;">
  <code>@Produces({MediaType.TEXT_HTML})</code> &#8211; defines the media type that the method can produce, in this case &#8220;text/html&#8221;.
</li>

**Response**

  * on success: if the podcast is removed a `204 No Content` success status is returned
  * on error: podcast is not available anymore and status of `404 Not found` is returned

## <span id="4_Logging">4. Logging</span>

<p style="text-align: justify;">
  Every request&#8217;s path and the response&#8217;s entity will be logged when the logging level is set to DEBUG. It is developed like a wrapper, AOP-style functionality with the help of Jetty filters.
</p>

See my post <a title="http://www.codingpedia.org/ama/how-to-log-in-spring-with-slf4j-and-logback/" href="http://www.codingpedia.org/ama/how-to-log-in-spring-with-slf4j-and-logback/" target="_blank">How to log in Spring with SLF4J and Logback</a> for more details on the matter.

## <span id="5_Exception_handling">5. Exception handling</span>

In case of errros, I decided to response with unified error message structure.  Here&#8217;s an example how an error response might look like:

<pre><code class="json">{
       "status": 400,
       "code": 400,
       "message": "Provided data not sufficient for insertion",
       "link": "http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-with-jersey-and-spring",
       "developerMessage": "Please verify that the feed is properly generated/set"
    }</code></pre>

<p class="note_normal" style="text-align: justify;">
  A detailed explanation of how errors are handled in the REST api, you can find in the following <a title="http://www.codingpedia.org/ama/error-handling-in-rest-api-with-jersey/" href="http://www.codingpedia.org/ama/error-handling-in-rest-api-with-jersey/" target="_blank">post Error handling in REST API with Jersey</a>
</p>

<h3 style="text-align: justify;">
  <span id="51_Custom_Reason_Phrase_in_HTTP_status_error_message_response_with_JAX-RS_Jersey">5.1. Custom Reason Phrase in HTTP status error message response with JAX-RS (Jersey)</span>
</h3>

<p style="text-align: justify;">
  If for some reason you need to produce a custom Reason Phrase in the HTTP status response delivered when an error occurs than you might want to check the following post
</p>

<p style="padding-left: 30px;">
  <a title="http://www.codingpedia.org/ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/" href="http://www.codingpedia.org/ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/" target="_blank">http://www.codingpedia.org/ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/</a>
</p>

## <span id="6_Add_CORS_support_on_the_server_side">6. Add CORS support on the server side</span>

<p style="text-align: justify;">
  I extended the capabilities of the API developed for the tutorial to support <a style="box-sizing: border-box; color: #bc360a; text-decoration: none; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; font-style: normal; font-variant: normal; font-weight: normal; letter-spacing: normal; line-height: 19.200000762939453px; orphans: auto; text-align: justify; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffffff;" title="http://www.w3.org/TR/cors/" href="http://www.w3.org/TR/cors/" target="_blank">Cross-Origing Resource Sharing (CORS) </a>on the server side.
</p>

<p style="text-align: justify;">
  Please see my post <a title="http://www.codingpedia.org/ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/" href="http://www.codingpedia.org/ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/" target="_blank">How to add CORS support on the server side in Java with Jersey</a> for more details on the matter.
</p>

## <span id="7_Testing">7. Testing</span>

### <span id="71_Integration_tests_in_Java">7.1. Integration tests in Java</span>

<p style="text-align: justify;">
  To test the application I will use the <code>Jersey Client</code> and execute requests against a running Jetty server with the application deployed on it. For that I will use the <a title="https://maven.apache.org/surefire/maven-failsafe-plugin/" href="https://maven.apache.org/surefire/maven-failsafe-plugin/">Maven Failsafe Plugin</a>.
</p>

#### <span id="711_Configuration">7.1.1. Configuration</span>

##### <span id="7111_Jersey_client_dependency">7.1.1.1 Jersey client dependency</span>

To build a Jersey client the `jersey-client` jar is required in the classpath. With Maven you can add it as a dependency to the `pom.xml` file:
<pre><code class="xml">&lt;dependency&gt;
        &lt;groupId&gt;org.glassfish.jersey.core&lt;/groupId&gt;
        &lt;artifactId&gt;jersey-client&lt;/artifactId&gt;
        &lt;version&gt;${jersey.version}&lt;/version&gt;
        &lt;scope&gt;test&lt;/scope&gt;
    &lt;/dependency&gt;</code></pre>

<h5 style="text-align: justify;">
  <span id="7112_Failsafe_plugin">7.1.1.2. Failsafe plugin</span>
</h5>

<p style="text-align: justify;">
  The <a title="http://maven.apache.org/surefire/maven-failsafe-plugin/" href="http://maven.apache.org/surefire/maven-failsafe-plugin/" target="_blank">Failsafe Plugin</a> is used during the integration-test and verify phases of the build lifecycle to execute the integration tests of the application. The Failsafe Plugin will not fail the build during the integration-test phase thus enabling the post-integration-test phase to execute.<br /> To use the Failsafe Plugin, you need to add the following configuration to your <code>pom.xml</code>
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

##### <span id="7112_Jetty_Maven_Plugin">7.1.1.2. Jetty Maven Plugin</span>

The integration tests will be executed against a running jetty server, that will be started only for the execution of the tests. For that you have to configure the following executionin the `jetty-maven-plugin`:

<pre><code class="xml">&lt;plugins&gt;
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
  <strong>Code alert:</strong> Find the complete <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/pom.xml" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/pom.xml" target="_blank">pom.xml</a> file on GitHub
</p>

#### <span id="712_Build_the_integration_tests">7.1.2. Build the integration tests</span>

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
    				.target("http://localhost:8888/demo-rest-jersey-spring/podcasts/2");

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
  * With the help `org.codehaus.jackson.map.ObjectMapper` I am displaying the JSON response pretty formatted

#### <span id="713_Running_the_integration_tests">7.1.3. Running the integration tests</span>

The Failsafe Plugin can be invoked by calling the `<tt>verify</tt>` phase of the build lifecycle.

<pre class="lang:default decode:true" title="Maven command to invoke the integration tests">mvn verify</pre>

To start jetty on port 8888 you need to set the `jetty.port` property to 8888. In Eclipse I use the following configuration:

<div id="attachment_1190" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse.png"><img class="size-medium wp-image-1190" src="{{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse-300x148.png" alt="Run integration tests from Eclipse" width="300" height="148" srcset="{{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse-300x148.png 300w, {{site.url}}/wp-content/uploads/2014/01/run-integration-tests-eclipse.png 991w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Run integration tests from Eclipse
  </p>
</div>

<h3 style="text-align: justify;">
  <span id="72_Integration_tests_with_SoapUI">7.2. Integration tests with SoapUI</span>
</h3>

<p style="text-align: justify;">
  Recently I&#8217;ve rediscovered <a title="http://www.soapui.org/" href="http://www.soapui.org/" target="_blank">SoapUI</a> after using it heavily for testing SOAP based web services. With the recent versions (at the time of writing latest is 5.0.0) it offers pretty good functionality to test REST based web services, and coming versions should improve on this. So unless you develop your own framework/infrastructure to test REST services, why not give it a try to SoapUI. I did, I was satisfied with the results so far and I&#8217;ve decided to do a video tutorial, that you can now find on YouTube on our channel:
</p>

<iframe width="420" height="315" src="https://www.youtube.com/embed/XV7WW0bDy9c" frameborder="0" allowfullscreen></iframe>

<h2 style="text-align: justify;">
  <span id="8_Versioning">8. Versioning</span>
</h2>

There are three major possibilities

  1. **URL**:  &#8220;/**v1**/podcasts/{id}&#8221;
  2. **Accept/Content-type header**: application/json; version=1

Because I am a developer and not a <a title="http://mikeschinkel.com/blog/whatisarestafarian/" href="http://mikeschinkel.com/blog/whatisarestafarian/" target="_blank">RESTafarian</a> yet I would do the URL option. All I would have to do on the implementation side for this example, would be to modify the `@Path`&#8216;s value annotation on the `PodcastResource` class from to

<pre class="lang:java mark:2 decode:true" title="Versioning in the path">@Component
@Path("/v1/podcasts")
public class PodcastResource {...}</pre>

Of course on a production application, you wouldn&#8217;t want every resource class preprefixed with the version number,  you&#8217;d want the version somehow treated through a filter in a AOP manner. Maybe something like this will come in a following post&#8230;

Here are some great resources from people that understand better the matter:

  * <a title="http://youtu.be/hdSrT4yjS1g?t=32m28s" href="http://youtu.be/hdSrT4yjS1g?t=32m28s" target="_blank">[Video] REST+JSON API Design &#8211; Best Practices for Developers</a>
  * <a title="http://www.troyhunt.com/2014/02/your-api-versioning-is-wrong-which-is.html" href="http://www.troyhunt.com/2014/02/your-api-versioning-is-wrong-which-is.html" target="_blank">Your API versioning is wrong, which is why I decided to do it 3 different wrong ways</a> by @troyhunt
  * <a title="http://www.informit.com/articles/article.aspx?p=1566460" href="http://www.informit.com/articles/article.aspx?p=1566460" target="_blank">Versioning REST Services</a>
  * <a title="http://stackoverflow.com/questions/389169/best-practices-for-api-versioning" href="http://stackoverflow.com/questions/389169/best-practices-for-api-versioning" target="_blank">Best practices for API versioning?</a> &#8211; interesting discussion on Stackoverflow

## <span id="9_Compression">9. Compression</span>

<p style="text-align: justify;">
  There may be cases when your REST api provides responses that are very long, and we all know how important transfer speed and bandwidth still are on mobile devices/networks. I think this is the first performance optimization point one needs to address, when developing REST apis that support mobile apps. Guess what? Because responses are text, we can compress them. And with today&#8217;s power of smartphones and tablets uncompressing them on the client side should not be a big deal&#8230;
</p>

<p style="text-align: justify;">
  On the server side, you can easily compress every response with Jersey&#8217;s <code>EncodingFilter</code>, which is a container filter that supports enconding-based content configuration. The filter examines what content encodings are supported by the container  and decides what encoding should be chosen based on the encodings listed in the <code>Accept-Encoding</code> request header and their associated quality values. To use particular content en/decoding, you need to register one or more content encoding providers  that provide specific encoding support; currently there are providers for GZIP and DEFLATE coding.
</p>

<p style="text-align: justify;">
  All I had to do in this case is adding the following line of code to my JaxRs application class:
</p>

<pre><code class="java">public class RestDemoJaxRsApplication extends ResourceConfig {
    	/**
    	 * Register JAX-RS application components.
    	 */
    	public RestDemoJaxRsApplication() {

            packages("org.codingpedia.demo.rest");
    		register(EntityFilteringFeature.class);
    		EncodingFilter.enableFor(this, GZipEncoder.class);

    	}
    }</code></pre>

## <span id="10Quick_way_to_check_if_the_REST_API_is_alive">10. Quick way to check if the REST API is alive</span>

<p style="text-align: justify;">
  There might be cases when you want to quickly verify if your REST API, that is deployed either on dev, test or prod environments, is reachable altogether. A common way to do this is by building a generic resource that delivers for example the version of the deployed API. You can trigger a request to this resource manually or, even better, have a Jenkings/Hudson job, which runs a checkup job after deployment. In the post <a title="http://www.codingpedia.org/ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/" href="http://www.codingpedia.org/ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/" target="_blank">Quick way to check if the REST API is alive – GET details from Manifest file</a>, I present how to implement such a service that reads default implementation and specification details from the application’s manifest file.
</p>

<h2 style="text-align: justify;">
  <span id="11_Security">11. Security</span>
</h2>

### <span id="111_Basic_Authentication">11.1. Basic Authentication</span>

<p style="text-align: justify;">
  Spring Security Framework&#8217;s powerful features were used to secure some REST resources (e.g. &#8220;/manifest&#8221;) with Basic Authentication. Read my following post <a title="http://www.codingpedia.org/ama/how-to-secure-jersey-rest-services-with-spring-security-and-basic-authentication/" href="http://www.codingpedia.org/ama/how-to-secure-jersey-rest-services-with-spring-security-and-basic-authentication/" target="_blank">How to secure Jersey REST services with Spring Security and Basic authentication</a> to find out how.
</p>

<h2 style="text-align: justify;">
  <span id="12_Summary">12. Summary</span>
</h2>

<p class="note_normal" style="text-align: justify;">
  Well, that&#8217;s it. I have to congratulate you, if you&#8217;ve come so far, but I hope you could learn something from this tutorial about REST, like designing a REST API, implementing a REST API in Java, testing a REST API and much more. If you did, I&#8217;d be very grateful if you helped it spread by leaving a comment or sharing it on Twitter, Google+ or Facebook. Thank you! Don&#8217;t forget also to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you&#8217;ll find for sure interesting podcasts and episodes. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support.</a>
</p>

## <span id="Resources">Resources</span>

### <span id="Source_Code">Source Code</span>

  * <a title="https://github.com/Codingpedia/demo-rest-jersey-spring" href="https://github.com/Codingpedia/demo-rest-jersey-spring" target="_blank">GitHub &#8211; Codingpedia/demo-rest-jersey-spring </a>(instructions on how to install and run the project)

### <span id="Web_resources">Web resources</span>

  1. <a title="http://www.w3.org/Protocols/rfc2616/rfc2616.html" href="http://www.w3.org/Protocols/rfc2616/rfc2616.html" target="_blank">HTTP &#8211; Hypertext Transfer Protocol &#8212; HTTP/1.1 &#8211; RFC2616</a>
  2. <a title="http://tools.ietf.org/html/rfc5789" href="http://tools.ietf.org/html/rfc5789" target="_blank">rfc5789 &#8211; PATCH Method for HTTP</a>
  3. <a title="https://jersey.java.net/documentation/latest/user-guide.html" href="https://jersey.java.net/documentation/latest/user-guide.html" target="_blank">Jersey User Guide</a>
  4. <a title="Status Code Definitions" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP Status Code Definitions</a>
  5. <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">REST &#8211; http://en.wikipedia.org/wiki/Representational_State_Transfer</a>
  6. <a title="Wikipedia - CRUD" href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD &#8211; http://en.wikipedia.org/wiki/Create,_read,_update_and_delete</a>
  7. <a title="Java API for RESTful Services (JAX-RS)" href="https://jax-rs-spec.java.net/" target="_blank">Java API for RESTful Services (JAX-RS)</a>
  8. <a title="Jersey REST" href="https://jersey.java.net/" target="_blank">Jersey &#8211; RESTful Web Services in Java </a>
  9. <a title="http://soabits.blogspot.dk/2013/01/http-put-patch-or-post-partial-updates.html" href="http://soabits.blogspot.dk/2013/01/http-put-patch-or-post-partial-updates.html" target="_blank">HTTP PUT, PATCH or POST &#8211; Partial updates or full replacement?</a>
 10. <a title="http://kingsfleet.blogspot.co.uk/2014/02/transparent-patch-support-in-jax-rs-20.html" href="http://kingsfleet.blogspot.co.uk/2014/02/transparent-patch-support-in-jax-rs-20.html" target="_blank">Transparent PATCH support in JAX-RS 2.0</a>
 11. <a title="https://maven.apache.org/surefire/maven-failsafe-plugin/" href="https://maven.apache.org/surefire/maven-failsafe-plugin/" target="_blank">Maven Failsafe Plugin</a>
 12. <a title="http://maven.apache.org/surefire/maven-failsafe-plugin/usage.html" href="http://maven.apache.org/surefire/maven-failsafe-plugin/usage.html" target="_blank">Maven Failsafe Plugin Usage</a>
 13. <a title="http://www.soapui.org/SoapUI-News/soapui-50-released-today.html" href="http://www.soapui.org/SoapUI-News/soapui-50-released-today.html" target="_blank">SoapUI 5.0 released today!</a>
 14. <a title="http://www.soapui.org/Functional-Testing/script-assertions.html" href="http://www.soapui.org/Functional-Testing/script-assertions.html" target="_blank">SoapUI &#8211; Using Script Assertions</a>
 15. <a title="https://www.youtube.com/watch?v=hdSrT4yjS1g" href="https://www.youtube.com/watch?v=hdSrT4yjS1g" target="_blank">[Video] REST+JSON API Design &#8211; Best Practices for Developers</a>
 16. <a title="https://www.youtube.com/watch?v=QpAhXa12xvU" href="https://www.youtube.com/watch?v=QpAhXa12xvU" target="_blank">[Video] RESTful API Design &#8211; Second Edition</a>
 17. <a title="http://en.wikipedia.org/wiki/Law_of_Demeter" href="http://en.wikipedia.org/wiki/Law_of_Demeter" target="_blank">Law of Demeter</a>

### <span id="Codingpedia_related_resources">Codingpedia related resources</span>

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
