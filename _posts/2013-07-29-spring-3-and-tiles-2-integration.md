---
id: 66
title: Spring 3 and Tiles 2 Integration
date: 2013-07-29T14:47:12+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=66
permalink: /ama/spring-3-and-tiles-2-integration/
dsq_thread_id:
  - 1544101307
fsb_social_facebook:
  - 1
fsb_show_social:
  - 0
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - spring
tags:
  - spring
  - spring mvc
  - tiles
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Necessary_artifacts">Necessary artifacts</a>
    </li>
    <li>
      <a href="#Spring_application_context_configuration_for_Tiles">Spring application context configuration for Tiles</a>
    </li>
    <li>
      <a href="#Creating_and_using_Tiles_pages">Creating and using Tiles pages</a><ul>
        <li>
          <a href="#Create_a_template">Create a template</a>
        </li>
        <li>
          <a href="#Create_the_composing_pages">Create the composing pages</a>
        </li>
        <li>
          <a href="#Create_a_definition">Create a definition</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Resources">Resources</a>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  <a title="Podcastpedia.org, knowledge to go" href="http://podcastpedia.org" target="_blank">Podcastpedia.org</a> makes use of <a title="Apache Tiles" href="http://tiles.apache.org/index.html" target="_blank">Apache Tiles</a> for its layout. This approach was taken because the website pages share a similar structure. Please read first the <a title="Tiles, getting started" href="http://tiles.apache.org/getting_started.html" target="_blank">Getting started guide</a> and <a title="Tiles concepts" href="http://tiles.apache.org/2.2/framework/tutorial/basic/concepts.html" target="_blank">Tiles concepts</a> to have a better understanding of what follows. This post will present how Tiles is integrated with Spring MVC for Podcastpedia.org
</p>

<p style="padding-left: 30px;">
  <em><strong>Note:</strong> Spring is the basic technology used for developing Podcastpedia.org</em>
</p>

## <span id="Necessary_artifacts">Necessary artifacts</span>

<p style="text-align: justify;">
  First of all Tiles jars are required in the classpath. They can be directly downloadded from the <a title="Download Tiles" href="http://tiles.apache.org/download.html" target="_blank">official website</a>, but Tiles has also been published to the public Maven repository.
</p>

<p style="text-align: justify;">
  You can use one dependency to download all Tiles supported technologies with the following dependency declaration:
</p>

<div>
  <pre class="lang:default decode:true" title="Tiles extras dependency">&lt;dependency&gt;
    &lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
    &lt;artifactId&gt;tiles-extras&lt;/artifactId&gt;
    &lt;version&gt;2.2.2&lt;/version&gt;
&lt;/dependency&gt;</pre>
</div>

<!--more-->

The basic Tiles dependency with only servlet support can be added this way:

<div>
  <pre class="lang:default decode:true" title="Tiles servlet dependency">&lt;dependency&gt;
	&lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
	&lt;artifactId&gt;tiles-servlet&lt;/artifactId&gt;
	&lt;version&gt;2.2.2&lt;/version&gt;
&lt;/dependency&gt;</pre>
</div>

If you need a dependency to Tiles JSP support, Declare the dependency this way:

<div>
  <pre class="lang:default decode:true" title="Tiles jsp dependency">&lt;dependency&gt;
    &lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
    &lt;artifactId&gt;tiles-jsp&lt;/artifactId&gt;
    &lt;version&gt;2.2.2&lt;/version&gt;
&lt;/dependency&gt;</pre>
</div>

## <span id="Spring_application_context_configuration_for_Tiles">Spring application context configuration for Tiles</span>

For better readability the Spring Tiles configuration has been placed in a separate file _pcm-tiles.xml_ :

<pre class="lang:default mark:34,48 decode:true" title="Spring application context - Tiles configuration">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

    &lt;!-- Views mapped in views.properties (PDF, XLS classes, and others) --&gt;
    &lt;bean id="contentNegotiatingResolver"
              class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver"&gt;
        &lt;property name="order"
                  value="#{T(org.springframework.core.Ordered).HIGHEST_PRECEDENCE}" /&gt;
         &lt;property name="favorPathExtension" value="true"/&gt;
        &lt;property name="contentNegotiationManager"&gt;
            &lt;bean class="org.springframework.web.accept.ContentNegotiationManager"&gt;
                &lt;constructor-arg&gt;
                    &lt;bean class="org.springframework.web.accept.PathExtensionContentNegotiationStrategy"&gt;
                        &lt;constructor-arg&gt;
                            &lt;map&gt;
                              &lt;entry key="html" value="text/html"/&gt;
                              &lt;entry key="pdf" value="application/pdf"/&gt;
                              &lt;entry key="xsl" value="application/vnd.ms-excel"/&gt;
                              &lt;entry key="xml" value="application/xml"/&gt;
                              &lt;entry key="json" value="application/json"/&gt;
                              &lt;entry key="atom" value="application/xml"/&gt;
                            &lt;/map&gt;
                        &lt;/constructor-arg&gt;
                    &lt;/bean&gt;
                &lt;/constructor-arg&gt;
            &lt;/bean&gt;
        &lt;/property&gt;
     &lt;/bean&gt;
    &lt;bean id="tilesViewResolver"
     class="org.springframework.web.servlet.view.UrlBasedViewResolver"&gt;
        &lt;property name="viewClass" value="org.springframework.web.servlet.view.tiles2.TilesView" /&gt;
        &lt;property name="order" value="#{contentNegotiatingResolver.order+1}" /&gt;
    &lt;/bean&gt;

    &lt;bean id="viewResolver" class="org.springframework.web.servlet.view.ResourceBundleViewResolver"&gt;
      &lt;property name="basename" value="views"/&gt;
      &lt;property name="order" value="#{tilesViewResolver.order+1}" /&gt;
    &lt;/bean&gt;

    &lt;!-- Helper class to configure Tiles 2.x for the Spring Framework --&gt;
    &lt;!-- See http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/web/servlet/view/tiles2/TilesConfigurer.html --&gt;
    &lt;!-- The actual tiles templates are in the tiles-definitions.xml  --&gt;
    &lt;bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles2.TilesConfigurer"&gt;
        &lt;property name="definitions"&gt;
            &lt;list&gt;
                &lt;value&gt;/WEB-INF/tile-defs/templates.xml&lt;/value&gt;
                &lt;value&gt;/WEB-INF/tile-defs/definitions.xml&lt;/value&gt;
            &lt;/list&gt;
        &lt;/property&gt;
    &lt;/bean&gt;
&lt;/beans&gt;</pre>

Of interest are

<li style="text-align: justify;">
  the <strong>TilesConfigurer</strong> (line 53) entry bean, which simply configures a TilesContainer using a set of files containing definitions (<em>templates.xml</em> and <em>definitions.xml</em>), to be accessed by TilesView instance. This is a Spring-based alternative (for usage in Spring configuration) to the Tiles-provided TilesListener (for usage in web.xml)
</li>
  * the **tilesViewResolver** instance (line 37) which is a `org.springframework.web.servlet.View` implementation that retrieves the Tiles definitions. The _order_ property and the _contentNegotiatingResolver_ will be discussed in another post.

## <span id="Creating_and_using_Tiles_pages">Creating and using Tiles pages</span>

After installing and learning some of Tiles concepts, it&#8217;s time to show you how the pages are created for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>

### <span id="Create_a_template">Create a template</span>

<a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> uses sort of a **classic  layout** page structure:

<div id="attachment_939" style="width: 370px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/07/Template.png"><img class="size-full wp-image-939" src="{{site.url}}/wp-content/uploads/2013/07/Template.png" alt="Template" width="360" height="378" srcset="{{site.url}}/wp-content/uploads/2013/07/Template.png 360w, {{site.url}}/wp-content/uploads/2013/07/Template-285x300.png 285w" sizes="(max-width: 360px) 100vw, 360px" /></a>

  <p class="wp-caption-text">
    Template
  </p>
</div>

As you can see the layout of the <a title="Podcastpedia.org, knowledge to go" href="http://podcastpedia.org" target="_blank">website</a> is made of

  * header which includes the social media connect, language selection logo and search bar
  * navigation bar (menu)
  * content &#8211; bulk data (body)
  * footer which includes links to Contact, About Us, Disclaimer pages

The first step was to create a JSP page that acts as this layout and place it under __/WEB-INF/template/_template.jsp_ file:

<pre class="lang:default mark:24,27,30,34 decode:true" title="File:  /WEB-INF/template/template.jsp">&lt;!DOCTYPE HTML&gt;
&lt;%@ include file="/WEB-INF/template/includes.jsp" %&gt;

&lt;%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" %&gt;

&lt;html&gt;
	&lt;head&gt;
		&lt;meta name="google-site-verification" content="xxxxxxxxx" /&gt;
		&lt;title&gt;&lt;tiles:insertAttribute name="title" ignore="true"/&gt;&lt;/title&gt;
		&lt;meta name="description" content="&lt;tiles:insertAttribute name="page_description" ignore="true"/&gt;"&gt;
		&lt;c:url var="cssURL" value="/static/css/style.min.css"/&gt;
		&lt;link href="${cssURL}" rel="stylesheet" type="text/css"/&gt;
		&lt;link rel="icon" href="&lt;c:url value="/static/images/favicon.ico"/&gt;" type="image/x-icon" /&gt;
		&lt;link rel="shortcut icon" href="&lt;c:url value="/static/images/favicon.ico"/&gt;" type="image/x-icon" /&gt;
		&lt;meta charset="utf-8"&gt;
		&lt;meta property="og:image" content="&lt;tiles:insertAttribute name="og_image" ignore="true"/&gt;" /&gt;
		&lt;meta property="og:title" content="&lt;tiles:insertAttribute name="og_title" ignore="true"/&gt;" /&gt;
		&lt;meta property="og:description" content="&lt;tiles:insertAttribute name="og_desc" ignore="true"/&gt;"/&gt;
		&lt;link rel="stylesheet" href="&lt;tiles:insertAttribute name="jquery_ui_css" ignore="true"/&gt;" /&gt;
	&lt;/head&gt;
    &lt;body&gt;
    	&lt;div id="banner"&gt;
			&lt;tiles:insertAttribute name="header" /&gt;
		&lt;/div&gt;
		&lt;div&gt;&lt;/div&gt;
		&lt;tiles:insertAttribute name="navigation_bar" /&gt;
		&lt;div&gt;&lt;/div&gt;
		&lt;div id="page"&gt;
			&lt;tiles:insertAttribute name="content" /&gt;
		&lt;/div&gt;
		&lt;div&gt;&lt;/div&gt;
		&lt;div id="footer_wrapper"&gt;
			&lt;tiles:insertAttribute name="footer" /&gt;
		&lt;/div&gt;
	&lt;/body&gt;
&lt;/html&gt;</pre>

You can see highlighted the four attributes  `header`, `navigation_bar` (menu), `content` (body) and `footer`, which resemble the template layout from the picture.  Besides these there are still some other Tiles attributes, like `title`, `page_description` or `og_image`,  that will get filled when rendering actual pages.

### <span id="Create_the_composing_pages">Create the composing pages</span>

In this phase I had to create four JSP pages, that will take place of `header`, `navigation_bar` (menu), `content` (body) and `footer` attributes in the previously created template.

### <span id="Create_a_definition">Create a definition</span>

A **definition** is a composition to be rendered to the end user; essentially a definition is composed of a **template** and completely or partially **filled attributes**.

  * If **all** of its attributes are filled, it can be rendered to the end user.
  * If **not all** of its attributes are filled, it is called an **abstract definition**, and it can be used as a base definition for extended definitions, or their missing attributes can be filled at runtime.

Initially I created a /`<em>WEB-INF/<em>tile-defs/template</em>.xml,</em>` which acts as an abstract definition and is extended by all the other Tiles definitions across the Podcastpedia application:

<pre class="lang:default decode:true" title="File: /WebContent/WEB-INF/tile-defs/template.xml">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 2.1//EN" "http://tiles.apache.org/dtds/tiles-config_2_1.dtd"&gt;

&lt;tiles-definitions&gt;
 ......
    &lt;definition name="defaultTemplate" template="/WEB-INF/template/template.jsp"&gt;
        &lt;put-attribute name="header" value="/WEB-INF/template/defaultHeader.jsp" /&gt;
        &lt;put-attribute name="navigation_bar" value="/WEB-INF/jsp/navigation_bar/default_navigation_bar.jsp" /&gt;
        &lt;put-attribute name="content" value="/WEB-INF/template/defaultContent.jsp"/&gt;
        &lt;put-attribute name="footer" value="/WEB-INF/template/defaultFooter.jsp" /&gt;
    &lt;/definition&gt;
 ......
&lt;/tiles-definitions&gt;</pre>

Now let&#8217;s see some examples of definitions extending the `defaultTemplate` one defined above. For example the definition rendering the home page  of Podcastpedia.org looks like the following:

<pre class="lang:default mark:3 decode:true" title="Snippet start page tiles definition from /WEB-INF/tile-defs/definitions.xml">&lt;tiles-definitions&gt;
 ......
	&lt;definition name="startPage_def" extends="defaultTemplate"&gt;
		&lt;put-attribute name="title" value="Podcastpedia, knowledge to go" /&gt;
    	&lt;put-attribute name="page_description" value="Educate yourself with selected podcasts from various domains such as science, technology, education, medicine, people, environment, spirituality and much more..."/&gt;
		&lt;put-attribute name="navigation_bar" value="/WEB-INF/jsp/navigation_bar/start_navigation_bar.jsp" /&gt;
		&lt;put-attribute name="content" value="/WEB-INF/jsp/start/start_page.jsp"/&gt;
    	&lt;put-attribute name="og_title" value="Podcastpedia, knowledge to go"/&gt;
 	    &lt;put-attribute name="og_desc" value="Educate yourself with selected podcasts from various domains such as science, technology, education, medicine, people, environment, spirituality and much more..."/&gt;
 	    &lt;put-attribute name="og_image" value="https://github.com/Codingpedia/podcastpedia/static/images/fb_share.png"/&gt;
	&lt;/definition&gt;
 ......
&lt;/tiles-definitions&gt;</pre>

<p style="text-align: justify;">
  Note that the Tiles attributes are filled with <strong>static</strong> values specific to the start page, and there are some attributes from the defaultTemplate (like header and footer) that are not overriden &#8211; that means they will be inherited from the <code>defaultTemplate</code> (in the end <strong>reuse</strong> is one of the advantages of inheritence).
</p>

<p style="text-align: justify;">
  Let&#8217;s see another example. This definition &#8211; <code>podcastDetails</code> &#8211; is used to render a podcast details page :
</p>

<pre class="lang:default mark:4 decode:true" title="Dynamic Tiles attributes">&lt;tiles-definitions&gt;
 ......
    &lt;definition name="podcastDetails" extends="defaultTemplate"&gt;
    	&lt;put-attribute name="title" expression="${podcast.title}"/&gt;
 	&lt;put-attribute name="page_description" expression="${podcast.description}"/&gt;
    	&lt;put-attribute name="navigation_bar" value="/WEB-INF/jsp/navigation_bar/podcast_details_navigation_bar.jsp" /&gt;
    	&lt;put-attribute name="content" value="/WEB-INF/jsp/podcastDetails.jsp"/&gt;
    	&lt;put-attribute name="og_title" expression="${podcast.title}"/&gt;
 	&lt;put-attribute name="og_desc" expression="${podcast.description}"/&gt;
 	&lt;put-attribute name="og_image" expression="${podcast.urlOfImageToDisplay}"/&gt;
    &lt;/definition&gt;
 ......
&lt;/tiles-definitions&gt;</pre>

<p style="text-align: justify;">
  Notice here you can also use <strong>dynamic</strong> values for the Tiles attributes &#8211; for example the <code>title</code> attribute (line 4) is set dynamically with a value,(<code>${podcast.title}</code>), passed from the controller . If you follow the link &#8211; <a title="Podcast details example" href="https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science" target="_blank">The Naked Scientist Podcast</a> &#8211; and inspect the source code or the tab name in your browser, you will notice the title of the page was set to &#8220;- The Naked Scientists Podcast &#8211; Stripping Down Science&#8221;
</p>

Well, this is pretty much all you need to do to integrate Tiles 2 in Spring MVC.

If you liked this, please show your support by <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">helping us</a> with <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>

We promise to only share high quality podcasts and episodes.

## <span id="Resources">Resources</span>

  1. <a title="Apache Tiles" href="http://tiles.apache.org/" target="_blank">http://tiles.apache.org/</a>
  2. <a title="Springsource" href="http://www.springsource.org/" target="_blank">http://www.springsource.org/</a>

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
