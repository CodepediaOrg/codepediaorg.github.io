---
id: 2173
title: How to integrate Jersey in a Spring MVC application
date: 2015-01-05T09:52:36+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2173
permalink: /ama/how-to-integrate-jersey-in-a-spring-mvc-application/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 6
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3391292323
categories:
  - java
  - spring
tags:
  - configuration
  - jax-rs
  - jax-rs 2.0
  - jersey
  - maven
  - spring mvc
---
<p style="text-align: justify;">
  I have recently started to build a public REST API with Java for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> and for the <a title="https://jax-rs-spec.java.net/" href="https://jax-rs-spec.java.net/" target="_blank">JAX-RS</a> implementation I have chosen <a title="https://jersey.java.net/" href="https://jersey.java.net/" target="_blank">Jersey</a>, as I find it &#8220;natural&#8221; and powerful &#8211; you can find out more about it by following the <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring.</a>  Because <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> is a web application powered by Spring MVC, I wanted to integrate both frameworks in <a title="https://github.com/podcastpedia/podcastpedia-web" href="https://github.com/podcastpedia/podcastpedia-web" target="_blank">podcastpedia-web</a>, to take advantage of the backend service functionality already present in the project. Anyway this short post will present the steps I had to take to make the integration between the two frameworks work.<!--more-->
</p>

## Framework versions

Current versions used:

<pre class="lang:default decode:true" title="Spring and Jersey used versions ">&lt;properties&gt;
	&lt;spring.version&gt;4.1.0.RELEASE&lt;/spring.version&gt;
	&lt;jersey.version&gt;2.14&lt;/jersey.version&gt;
&lt;/properties&gt;</pre>

## Project dependencies

The <a style="color: #bc360a;" title="https://jersey.java.net/documentation/latest/spring.html" href="https://jersey.java.net/documentation/latest/spring.html" target="_blank">Jersey Spring extension</a> must be present in your project’s classpath. If you are using Maven add it to the `pom.xml` file of your project:

<pre class="lang:default decode:true" title="Dependencies snippet - pom.xml"><code>&lt;!-- Jersey-Spring http://mvnrepository.com/artifact/org.glassfish.jersey.ext/jersey-spring3/2.4.1 --&gt;
&lt;dependency&gt;
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
	&lt;version&gt;${jersey.version}&lt;/version&gt;
	&lt;exclusions&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;com.fasterxml.jackson.jaxrs&lt;/groupId&gt;
			&lt;artifactId&gt;jackson-jaxrs-base&lt;/artifactId&gt;
		&lt;/exclusion&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;com.fasterxml.jackson.core&lt;/groupId&gt;
			&lt;artifactId&gt;jackson-annotations&lt;/artifactId&gt;
		&lt;/exclusion&gt;
		&lt;exclusion&gt;
			&lt;groupId&gt;com.fasterxml.jackson.jaxrs&lt;/groupId&gt;
			&lt;artifactId&gt;jackson-jaxrs-json-provider&lt;/artifactId&gt;
		&lt;/exclusion&gt;
	&lt;/exclusions&gt;
&lt;/dependency&gt;</code></pre>

<p class="note_normal">
  <strong>Note:</strong> I have explicitly excluded the Spring core and the Jackson implementation libraries as they have been already imported in the project with preferred versions.
</p>

## Web.xml  configuration

In the `web.xml`, in addition  to the Spring MVC servlet configuration I added the jersey-servlet configuration, that will map all requests starting with`/api/`:

<pre class="lang:default mark:18-32 decode:true" title="Configuration snippet from web.xml">&lt;servlet&gt;
	&lt;servlet-name&gt;Spring MVC Dispatcher Servlet&lt;/servlet-name&gt;
	&lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;

	&lt;init-param&gt;
		&lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
		&lt;param-value&gt;
			classpath:spring/application-context.xml
		&lt;/param-value&gt;
	&lt;/init-param&gt;
	&lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
&lt;/servlet&gt;
&lt;servlet-mapping&gt;
	&lt;servlet-name&gt;Spring MVC Dispatcher Servlet&lt;/servlet-name&gt;
	&lt;url-pattern&gt;/&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;

&lt;servlet&gt;
	&lt;servlet-name&gt;jersey-serlvet&lt;/servlet-name&gt;
	&lt;servlet-class&gt;
		org.glassfish.jersey.servlet.ServletContainer
	&lt;/servlet-class&gt;
	&lt;init-param&gt;
		&lt;param-name&gt;javax.ws.rs.Application&lt;/param-name&gt;
		&lt;param-value&gt;org.podcastpedia.web.api.JaxRsApplication&lt;/param-value&gt;
	&lt;/init-param&gt;
	&lt;load-on-startup&gt;2&lt;/load-on-startup&gt;
&lt;/servlet&gt;
&lt;servlet-mapping&gt;
	&lt;servlet-name&gt;jersey-serlvet&lt;/servlet-name&gt;
	&lt;url-pattern&gt;/api/*&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;
</pre>

<p style="text-align: justify;">
  Well, that&#8217;s pretty much it&#8230; If you have any questions drop me a line or comment in the discussion below.
</p>

<p style="text-align: justify;">
  In the coming post I will present some of the results of this integration, by showing how to call one method of the REST public API with jQuery, to dynamically load recent episodes of a podcast, so stay tuned.
</p>

<p style="text-align: justify;">
   

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
</p>
