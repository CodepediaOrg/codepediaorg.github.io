---
id: 931
title: Upgrade from Tiles 2 to Tiles 3 on Spring MVC 3 application
date: 2013-11-14T21:29:36+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=931
permalink: /ama/upgrade-from-tiles-2-to-tiles-3-on-spring-mvc-3-application/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 1
dsq_thread_id:
  - 1966447834
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - spring
tags:
  - apache
  - spring
  - tiles
  - upgrade
format: aside
---
<p style="text-align: justify;">
  I&#8217;ve just upgraded the web application that powers <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> to use Apache Tiles 3 instead of Tiles 2. I am particularly interested in the Tiles integration with Velocity for email generation.
</p>

<p style="text-align: justify; padding-left: 30px;">
  <em><strong>Note:</strong> You need at least the Spring 3.2 release for this to work.</em>
</p>

<p style="text-align: justify;">
  This occured in three small steps:
</p>

  1. upgrade maven dependencies
  2. upgrade tiles elements in the Spring application context
  3. upgrade tiles dtd version <!--more-->

#### 1. Upgrade maven dependencies

<p style="text-align: justify;">
  What I had to do was just replace the <code>${tiles.version}</code> in the maven <code>pom.xml</code> file from <code>2.2.2</code> to <code>3.0.1</code> for the Tiles dependencies I use, and now it looks like this:
</p>

<pre class="lang:default decode:true">&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
			&lt;artifactId&gt;tiles-extras&lt;/artifactId&gt;
			&lt;version&gt;${tiles.version}&lt;/version&gt;
		&lt;/dependency&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
			&lt;artifactId&gt;tiles-jsp&lt;/artifactId&gt;
			&lt;version&gt;${tiles.version}&lt;/version&gt;
		&lt;/dependency&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
			&lt;artifactId&gt;tiles-servlet&lt;/artifactId&gt;
			&lt;version&gt;${tiles.version}&lt;/version&gt;
		&lt;/dependency&gt;	
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
			&lt;artifactId&gt;tiles-template&lt;/artifactId&gt;
			&lt;version&gt;${tiles.version}&lt;/version&gt;
		&lt;/dependency&gt;	
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
			&lt;artifactId&gt;tiles-el&lt;/artifactId&gt;
			&lt;version&gt;${tiles.version}&lt;/version&gt;
		&lt;/dependency&gt;</pre>

#### 2. Upgrad Tiles configuration in Spring application context

The next thing was to upgrade to version 3 the `tilesViewResolver` and `tilesConfigurer` in the Spring application context:

<pre class="lang:default mark:5,19 decode:true" title="Tiles configuration in the Spring application context">....................
&lt;!-- Convenience subclass of UrlBasedViewResolver that supports TilesView (i.e. Tiles definitions) and custom subclasses of it. --&gt;
&lt;!-- Don't forget to set the order if you declared other ViewResolvers 
--&gt;    
&lt;bean id="tilesViewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver"&gt;
	&lt;property name="viewClass" value="org.springframework.web.servlet.view.tiles3.TilesView" /&gt;
	&lt;property name="order" value="#{contentNegotiatingResolver.order+1}" /&gt;
&lt;/bean&gt;

&lt;bean id="viewResolver" class="org.springframework.web.servlet.view.ResourceBundleViewResolver"&gt;
  &lt;property name="basename" value="views"/&gt;
  &lt;property name="order" value="#{tilesViewResolver.order+1}" /&gt;
&lt;/bean&gt;

&lt;!-- Helper class to configure Tiles 2.x for the Spring Framework --&gt;
&lt;!-- See http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/web/servlet/view/tiles2/TilesConfigurer.html --&gt;
&lt;!-- The actual tiles templates are in the tiles-definitions.xml  --&gt;

&lt;bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles3.TilesConfigurer"&gt;	
	&lt;property name="definitions"&gt;		
		&lt;list&gt;
			&lt;value&gt;/WEB-INF/tile-defs/templates.xml&lt;/value&gt;
			&lt;value&gt;/WEB-INF/tile-defs/definitions.xml&lt;/value&gt;
		&lt;/list&gt;		
	&lt;/property&gt;	
&lt;/bean&gt;
....................</pre>

### 3. Upgrade tiles.dtd version

In the Tiles definitions templates upgrade to Tiles 3.0 dtd

<pre class="lang:default mark:2 decode:true" title="Tiles definition snippet">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd"&gt;        
&lt;tiles-definitions&gt;  
.........         
&lt;/tiles-definitions&gt;</pre>

That&#8217;s it.

P.S.
  
For a complete example of Tiles integration with Spring MVC see my post &#8211; <a title="Spring 3 and Tiles 2 integration" href="http://www.codingpedia.org/ama/spring-3-and-tiles-2-integration/" target="_blank">Spring 3 and Tiles 2 Integration</a>
