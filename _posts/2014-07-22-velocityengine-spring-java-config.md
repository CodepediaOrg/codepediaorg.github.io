---
id: 1697
title: VelocityEngine Spring Java Config
date: 2014-07-22T06:55:01+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1697
permalink: /ama/velocityengine-spring-java-config/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 1
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2863114755
categories:
  - java
  - spring
tags:
  - configuration
  - velocity
  - xml to java config
---
This is a first post in a series of short code snippets that will present the configuration of Spring beans from XML to Java.

**XML:**

<pre class="lang:default decode:true" title="XML config">&lt;bean id="velocityEngine" class="org.springframework.ui.velocity.VelocityEngineFactoryBean"&gt;
  &lt;property name="velocityProperties"&gt;
	 &lt;value&gt;
	  resource.loader=class
	  class.resource.loader.class=org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader
	 &lt;/value&gt;
  &lt;/property&gt;
&lt;/bean&gt;</pre>

**Java:**<!--more-->

<pre class="lang:java decode:true" title="Java config">@Bean
public VelocityEngine velocityEngine() throws VelocityException, IOException{
	VelocityEngineFactoryBean factory = new VelocityEngineFactoryBean();
	Properties props = new Properties();
	props.put("resource.loader", "class");
	props.put("class.resource.loader.class",
			  "org.apache.velocity.runtime.resource.loader." +
			  "ClasspathResourceLoader");
	factory.setVelocityProperties(props);

	return factory.createVelocityEngine();
}</pre>

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
