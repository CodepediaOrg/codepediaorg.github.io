---
id: 2231
title: How to secure Jersey REST services with Spring Security and Basic authentication
date: 2015-02-12T17:36:35+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2231
permalink: /ama/how-to-secure-jersey-rest-services-with-spring-security-and-basic-authentication/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 7
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 3
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3509817746
categories:
  - java
  - Java EE
  - spring
tags:
  - basic authentication
  - jersey
  - rest
  - rest api
  - security
  - web services
---
<p style="text-align: justify;">
  In my previous blog post, <a title="http://www.codingpedia.org/ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/" href="http://www.codingpedia.org/ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/" target="_blank">Quick way to check if the REST API is alive &#8211; GET details from Manifest file</a>, I showed how to develop a REST resource to easy check if the developed REST API is reachable. In this post I will present how you can secure this resource with Spring Security and <a title="http://en.wikipedia.org/wiki/Basic_access_authentication" href="http://en.wikipedia.org/wiki/Basic_access_authentication" target="_blank">Basic authentication</a> &#8211; <em>&#8220;In the context of an HTTP transaction, basic access authentication is a method for an HTTP user agent to provide a user name and password when making a request.&#8221;</em>
</p>

<p class="note_normal" style="text-align: justify;">
  The secured REST resources introduced here are part of bigger project, presented extensively in the <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>
</p>

<p style="text-align: justify;">
  <!--more-->
</p>

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#Software_used">Software used</a>
      </li>
      <li>
        <a href="#Spring_securityconfiguration">Spring security configuration</a><ul>
          <li>
            <a href="#Libraries">Libraries</a>
          </li>
          <li>
            <a href="#Security-application_context">Security-application context</a>
          </li>
          <li>
            <a href="#Webxml">Web.xml</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Testing">Testing</a><ul>
          <li>
            <a href="#Browser">Browser</a>
          </li>
          <li>
            <a href="#SoapUI">SoapUI</a><ul>
              <li>
                <a href="#Request">Request</a>
              </li>
              <li>
                <a href="#Response">Response</a>
              </li>
            </ul>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Summary">Summary</a>
      </li>
      <li>
        <a href="#Resources">Resources</a>
      </li>
    </ul>
  </div>
</p>

## <span id="Software_used">Software used</span>

  1. Jersey JAX-RS implementation 2.14
  2. Spring 4.1.4
  3. Spring security 3.2.5
  4. Maven 3.1.1
  5. JDK 7

## <span id="Spring_securityconfiguration">Spring security configuration</span>

### <span id="Libraries">Libraries</span>

<p style="text-align: justify;">
  To secure the REST services with basic authentication, the following Spring security libraries are necessary in the classpath. Because I am using Maven, they are listed as Maven dependencies in the pom.xml:
</p>

<pre class="lang:default decode:true" title="Spring security libraries">&lt;!-- Spring security --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
	&lt;artifactId&gt;spring-security-core&lt;/artifactId&gt;
	&lt;version&gt;${spring.security.version}&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
	&lt;artifactId&gt;spring-security-web&lt;/artifactId&gt;
	&lt;version&gt;${spring.security.version}&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
	&lt;artifactId&gt;spring-security-config&lt;/artifactId&gt;
	&lt;version&gt;${spring.security.version}&lt;/version&gt;
&lt;/dependency&gt;</pre>

### <span id="Security-application_context">Security-application context</span>

<pre class="lang:default decode:true" title="Spring security configuration">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans:beans xmlns="http://www.springframework.org/schema/security"
    xmlns:beans="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:security="http://www.springframework.org/schema/security"
    xsi:schemaLocation="
    	http://www.springframework.org/schema/beans
    	http://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/security
        http://www.springframework.org/schema/security/spring-security.xsd"&gt;

	&lt;!-- Stateless RESTful services use BASIC authentication --&gt;
    &lt;security:http create-session="stateless" pattern="/manifest/**"&gt;
        &lt;security:intercept-url pattern="/**" access="ROLE_REST"/&gt;
        &lt;security:http-basic/&gt;
    &lt;/security:http&gt;

    &lt;security:authentication-manager&gt;
        &lt;security:authentication-provider&gt;
            &lt;security:user-service&gt;
                &lt;security:user name="rest" password="rest" authorities="ROLE_REST"/&gt;
            &lt;/security:user-service&gt;
        &lt;/security:authentication-provider&gt;
    &lt;/security:authentication-manager&gt;

&lt;/beans:beans&gt;</pre>

<p style="text-align: justify;">
  As you can see, a <em><strong>&#8220;rest&#8221;</strong></em> user and role is defined in memory. These are defined in the element <code>&lt;security:user-service&gt;</code> and its child element <code>&lt;security:user&gt;</code>. This makes sure that only users with <code>ROLE_REST</code> role are able to reach:
</p>

<pre class="lang:default decode:true" title="authenticaion manager with in memory setup">&lt;security:authentication-manager&gt;
	&lt;security:authentication-provider&gt;
		&lt;security:user-service&gt;
			&lt;security:user name="rest" password="rest" authorities="ROLE_REST"/&gt;
		&lt;/security:user-service&gt;
	&lt;/security:authentication-provider&gt;
&lt;/security:authentication-manager&gt;</pre>

The next step is to secure the `/manifest/*` URLs, allowing access only to the newly defined rest user:

<pre class="lang:default decode:true" title="Securing URLs with role-based access">&lt;security:http create-session="stateless" pattern="/manifest/**"&gt;
	&lt;security:intercept-url pattern="/**" access="ROLE_REST"/&gt;
	&lt;security:http-basic/&gt;
&lt;/security:http&gt;</pre>

<p style="text-align: justify;">
  Basic HTTP authentication is enabled in our application by the <code>&lt;security:http-basic/&gt;</code> line.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong><br /> You cannot define the security constraints of Spring Security in the applicationContext.xml file, because they need to be loaded up with the Servlet listeneer and the filter chain. They need to be in a proper WebApplicationContext defined with a Servlet listener, not the Servlet-related one. This is because the DelegatingFilterProxy will look for the root application context defined in the ServletContext that is loaded by the ContextLoaderListener.  If you define only applicationContext.xml, because the filters load first, before the servlets, the fiilter won&#8217;t be able to find any application context, so it won&#8217;t be able to load up correctly.
</p>

### <span id="Webxml">Web.xml</span>

Extend now the `contextConfigLocation` context parameter, to be aware of the the new spring security configuration file `security-context.xml`:

<pre class="lang:default decode:true" title="web.xml - context-param extension">&lt;context-param&gt;
	&lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
	&lt;param-value&gt;
		classpath:spring/applicationContext.xml
		classpath:spring/security-context.xml
	&lt;/param-value&gt;
&lt;/context-param&gt;</pre>

Hook in Spring security only for URLs related to the _manifest_:

<pre class="lang:default mark:18-27 decode:true" title="Hook into Spring security">&lt;servlet&gt;
	&lt;servlet-name&gt;jersey-servlet&lt;/servlet-name&gt;
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
	&lt;servlet-name&gt;jersey-servlet&lt;/servlet-name&gt;
	&lt;url-pattern&gt;/*&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;

&lt;!--Hook into spring security--&gt;
&lt;filter&gt;
	&lt;filter-name&gt;springSecurityFilterChain&lt;/filter-name&gt;
	&lt;filter-class&gt;org.springframework.web.filter.DelegatingFilterProxy&lt;/filter-class&gt;
&lt;/filter&gt;

&lt;filter-mapping&gt;
  &lt;filter-name&gt;springSecurityFilterChain&lt;/filter-name&gt;
  &lt;url-pattern&gt;/manifest/*&lt;/url-pattern&gt;
&lt;/filter-mapping&gt;</pre>

The Spring security filter chain needs to be activated.

## <span id="Testing">Testing</span>

### <span id="Browser">Browser</span>

If you access a secured location via the browser, a standard HTTP authentication popup appears asking for the authentication details:

[<img class="alignnone size-full wp-image-2263" src="{{site.url}}/wp-content/uploads/2015/02/standard-popup-browser.png" alt="standard-popup-browser" width="895" height="462" srcset="{{site.url}}/wp-content/uploads/2015/02/standard-popup-browser.png 895w, {{site.url}}/wp-content/uploads/2015/02/standard-popup-browser-300x154.png 300w" sizes="(max-width: 895px) 100vw, 895px" />]({{site.url}}/wp-content/uploads/2015/02/standard-popup-browser.png)

Put in _rest/rest_ and you should receive the JSON response.

### <span id="SoapUI">SoapUI</span>

It&#8217;s fairly easy to test a secured REST with Basic Authentication via soapUI. See the following video to find out more:



#### <span id="Request">Request</span>

<pre class="lang:default decode:true" title="Request resource with Basic authentication">GET http://localhost:8888/demo-rest-jersey-spring/manifest HTTP/1.1
Accept-Encoding: gzip,deflate
Accept: application/json
Host: localhost:8888
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)
Authorization: Basic cmVzdDpyZXN0
</pre>

<p style="text-align: justify;">
  Note the Authorization header which is constructed as follows:
</p>

<ol style="color: #252525;">
  <li style="text-align: justify;">
    Username and password are combined into a string &#8220;username:password&#8221;
  </li>
  <li style="text-align: justify;">
    The resulting string is then encoded using the RFC2045-MIME variant of <a style="color: #0b0080;" title="Base64" href="http://en.wikipedia.org/wiki/Base64">Base64</a>, except not limited to 76 char/line
  </li>
  <li style="text-align: justify;">
    The authorization method and a space i.e. &#8220;Basic &#8221; is then put before the encoded string.
  </li>
</ol>

#### <span id="Response">Response</span>

<pre class="lang:default decode:true" title="Response - manifest details">HTTP/1.1 200 OK
Date: Tue, 03 Feb 2015 15:47:32 GMT
Content-Type: application/json
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, PUT
Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 196
Server: Jetty(9.2.6.v20141205)

{"Implementation-Title":"DemoRestWS","Implementation-Version":"0.0.1-SNAPSHOT","Implementation-Vendor-Id":"org.codingpedia","Built-By":"Matei1.Adrian","Build-Jdk":"1.7.0_40","Manifest-Version":"1.0","Created-By":"Apache Maven 3.1.1","Specification-Title":"DemoRestWS","Specification-Version":"0.0.1-SNAPSHOT"}</pre>

<h2 style="text-align: justify;">
  <span id="Summary">Summary</span>
</h2>

<p style="text-align: justify;">
  Well, that&#8217;s it. Spring Security is a very powerful framework with a gazzilion of configuration options. In this post I just shown one of them, namely how to secure REST resources with Basic Authentication. Of course in a more realistic scenario you would store the users and roles in an LDAP directory, or database&#8230;
</p>

<p class="note_alert" style="text-align: justify;">
  <strong>Note:<br /> </strong>If you decide to use Basic Authentication to secure your REST resources, please make sure they are called over HTTPS. The preferred way nowadays to secure REST resources is with <a title="http://oauth.net/" href="http://oauth.net/" target="_blank">OAuth</a>. More on that on a later post.
</p>

## <span id="Resources">Resources</span>

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
