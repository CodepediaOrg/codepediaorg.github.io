---
id: 944
title: Spring MVC and Apache Tiles integration example
date: 2013-11-19T21:44:37+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=944
permalink: /ama/spring-mvc-and-apache-tiles-integration-example/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 8
dsq_thread_id:
  - 1980324968
fsb_social_google:
  - 9
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 1
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:49:"https://github.com/podcastpedia/podcastpedia-web/";s:11:"ribbon-type";i:10;}'
categories:
  - spring
tags:
  - apache
  - example
  - integration
  - spring
  - spring mvc
  - tiles
---
<p class="note_alert" style="text-align: justify;">
  <strong>Note:</strong> This is a re-edit of the post <a title="Spring 3 and Tiles 2 Integration" href="http://www.codingpedia.org/ama/spring-3-and-tiles-2-integration/" target="_blank">Spring 3 and Tiles 2 Integration.</a> It uses now the latest version of Apache Tiles (at the time of the writing 3.0.1) and presents how Apache Tiles is used on top of Spring/Spring MVC to construct the layout of the <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> website
</p>

## <span id="1_Why_Apache_Tiles">1. Why Apache Tiles?</span>

Well, because it

<div>
  <ul>
    <li style="text-align: justify;">
      is a free open-sourced templating framework for modern Java applications.  Based upon the Composite pattern it is built to simplify the development of user interfaces.
    </li>
    <li style="text-align: justify;">
      remains, for complex web sites, the easiest and most elegant way to work alongside any MVC technology.
    </li>
  </ul>

  <p style="text-align: justify;">
    Tiles allows authors to define page fragments which can be assembled into a complete pages at runtime. These fragments, or tiles, can be used as simple includes in order to reduce the duplication of common page elements or embedded within other tiles to develop a series of reusable templates. These templates streamline the development of a consistent look and feel across an entire application.
  </p>

  <p style="text-align: justify;">
    <p class="note_normal">
      <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
    </p>

  </p>
</div>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Why_Apache_Tiles">1. Why Apache Tiles?</a>
    </li>
    <li>
      <a href="#2_Necessary_artifacts">2. Necessary artifacts</a>
    </li>
    <li>
      <a href="#3_Spring_application_context_configuration_for_Tiles">3. Spring application context configuration for Tiles</a>
    </li>
    <li>
      <a href="#4_Creating_and_using_Tiles_pages">4. Creating and using Tiles pages</a><ul>
        <li>
          <a href="#41_Create_a_template">4.1. Create a template</a>
        </li>
        <li>
          <a href="#42_Create_the_composing_pages">4.2. Create the composing pages</a>
        </li>
        <li>
          <a href="#44_Create_a_definition">4.4. Create a definition</a><ul>
            <li>
              <a href="#441_Create_abstract_definition">4.4.1. Create abstract definition</a>
            </li>
            <li>
              <a href="#442_Create_concrete_definitions">4.4.2. Create concrete definitions</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#5_Summary">5. Summary</a>
    </li>
    <li>
      <a href="#6_Resources">6. Resources</a>
    </li>
  </ul>
</div>

## <span id="2_Necessary_artifacts">2. Necessary artifacts</span>

<p style="text-align: justify;">
  Make sure you have the Tiles jars in the classpath. They can be downloaded directly from the <a title="Download Tiles" href="http://tiles.apache.org/download.html" target="_blank">official website</a>, or by adding the proper dependencies to the <code>pom.xml</code> file, if you use <a title="Apache Maven" href="http://maven.apache.org/" target="_blank">Apache Maven</a>. I use the latter.
</p>

<p style="text-align: justify;">
  You can use one dependency to download all Tiles supported technologies with the following dependency declaration:
</p>


<pre><code class="xml">&lt;dependency&gt;
    &lt;groupId&gt;org.apache.tiles&lt;/groupId&gt;
    &lt;artifactId&gt;tiles-extras&lt;/artifactId&gt;
    &lt;version&gt;${tiles.version}&lt;/version&gt;
&lt;/dependency&gt;</code></pre>

or, as in my case where I didn&#8217;t need all the extras, you can put in your `pom.xml` only the ones you need:

<pre><code class="xml">&lt;dependency&gt;
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
&lt;/dependency&gt;</code></pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The <code>tiles.version</code> is currently 3.0.1. For this to work you need to use at least version <strong>3.2</strong> of Spring MVC.
</p>

## <span id="3_Spring_application_context_configuration_for_Tiles">3. Spring application context configuration for Tiles</span>

For better readability and separation of concerns the Spring Tiles configuration has been placed in a separate application context file _`pcm-tiles.xml`_ :

<pre><code class="xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
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
        &lt;property name="viewClass" value="org.springframework.web.servlet.view.tiles3.TilesView" /&gt;
        &lt;property name="order" value="#{contentNegotiatingResolver.order+1}" /&gt;
    &lt;/bean&gt;

    &lt;bean id="viewResolver" class="org.springframework.web.servlet.view.ResourceBundleViewResolver"&gt;
      &lt;property name="basename" value="views"/&gt;
      &lt;property name="order" value="#{tilesViewResolver.order+1}" /&gt;
    &lt;/bean&gt;

    &lt;!-- Helper class to configure Tiles 3.x for the Spring Framework --&gt;
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
&lt;/beans&gt;</code></pre>

Of interest here are

<li style="text-align: justify;">
  the <strong>TilesConfigurer</strong> (line 53) entry bean, which simply configures a TilesContainer using a set of files containing definitions (<em>templates.xml</em> and <em>definitions.xml</em>), to be accessed by TilesView instance. This is a Spring-based alternative (for usage in Spring configuration) to the Tiles-provided TilesListener (for usage in web.xml)
</li>
  * the **tilesViewResolver** instance (line 37) which is a `org.springframework.web.servlet.View` implementation that retrieves the Tiles definitions. The `order` and the `contentNegotiatingResolver` properties will be discussed in another post.

## <span id="4_Creating_and_using_Tiles_pages">4. Creating and using Tiles pages</span>

After installing and learning some of Tiles concepts, it&#8217;s time to show you how the pages are created for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>

### <span id="41_Create_a_template">4.1. Create a template</span>

<a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> uses sort of a **classic layout** page structure:

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

<pre><code class="xml">&lt;!DOCTYPE HTML&gt;
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
&lt;/html&gt;</code></pre>

You can see highlighted the four attributes `header`, `navigation_bar` (menu), `content` (body) and `footer`, which resemble the template layout from the picture. Besides these there are still some other Tiles attributes, like `title`, `page_description` or `og_image`, that will get filled when rendering actual pages.

### <span id="42_Create_the_composing_pages">4.2. Create the composing pages</span>

In this phase I had to create four JSP pages, that will take place of `header`, `navigation_bar` (menu), `content` (body) and `footer` attributes in the previously created template.

### <span id="44_Create_a_definition">4.4. Create a definition</span>

A **definition** is a composition to be rendered to the end user; essentially a definition is composed of a **template** and completely or partially **filled attributes**.

  * If **all** of its attributes are filled, it can be rendered to the end user.
  * If **not all** of its attributes are filled, it is called an **abstract definition**, and it can be used as a base definition for extended definitions, or their missing attributes can be filled at runtime.

#### <span id="441_Create_abstract_definition">4.4.1. Create abstract definition</span>

Initially I created a /`<em>WEB-INF/<em>tile-defs/template</em>.xml,</em>` with some default .jsp files (`defaultHeader.jsp`, `default_navigation_bar.jsp`,  `defaultContent.jsp` and `defaultFooter.jsp`). This file acts as an abstract definition and is extended by all the other Tiles definitions across the Podcastpedia application:

<pre><code class="xml">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd"&gt;

&lt;tiles-definitions&gt;
 ......
    &lt;definition name="defaultTemplate" template="/WEB-INF/template/template.jsp"&gt;
        &lt;put-attribute name="header" value="/WEB-INF/template/defaultHeader.jsp" /&gt;
        &lt;put-attribute name="navigation_bar" value="/WEB-INF/jsp/navigation_bar/default_navigation_bar.jsp" /&gt;
        &lt;put-attribute name="content" value="/WEB-INF/template/defaultContent.jsp"/&gt;
        &lt;put-attribute name="footer" value="/WEB-INF/template/defaultFooter.jsp" /&gt;
    &lt;/definition&gt;
 ......
&lt;/tiles-definitions&gt;</code></pre>

#### <span id="442_Create_concrete_definitions">4.4.2. Create concrete definitions</span>

Now let&#8217;s see some examples of definitions extending the `defaultTemplate` defined above. For example the definition rendering the <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">home page of Podcastpedia.org</a> looks like the following:

<pre><code class="xml">&lt;tiles-definitions&gt;
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
&lt;/tiles-definitions&gt;</code></pre>

<p style="text-align: justify;">
  Note that the Tiles attributes are filled with <strong>static</strong> values specific to the start page, and there are some attributes from the <code>defaultTemplate</code> (like <code>header</code> and <code>footer</code>) that are not overriden &#8211; that means they will be inherited from the <code>defaultTemplate</code> (one of the advantages of inheritence).
</p>

<p style="text-align: justify;">
  Let&#8217;s see another example. The  <code>podcastDetails</code>-definition is used to render a podcast details page :
</p>

<pre><code class="xml">&lt;tiles-definitions&gt;
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
&lt;/tiles-definitions&gt;</code></pre>

<p style="text-align: justify;">
  Notice here how I use <strong>dynamic</strong> values for the Tiles attributes &#8211; for example the <code>title</code> attribute (line 4) is set dynamically with a value(<code>${podcast.title}</code>) passed from the controller. If you follow the link &#8211; <a title="Podcast details example" href="https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science" target="_blank">The Naked Scientist Podcast</a> &#8211; and inspect the source code or the tab name in your browser, you will notice the title of the page was set to &#8220;- The Naked Scientists Podcast &#8211; Stripping Down Science&#8221;
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>
</p>

## <span id="5_Summary">5. Summary</span>

<p style="text-align: justify;">
  Well, this is pretty much all you need to do to integrate Tiles in Spring MVC &#8211; you&#8217;ve learned what libraries you need, how to configure Tiles in the application context, how to create abstract and concrete Tiles definitions, and fill them with both static and dynamic values.
</p>

## <span id="6_Resources">6. Resources</span>

  1. <a title="Spring 3 and Tiles 2 Integration" href="http://www.codingpedia.org/ama/spring-3-and-tiles-2-integration/" target="_blank">Spring 3 and Tiles 2 Integration</a>
  2. <a title="Upgrade Tiles 2 to Tiles 3" href="http://www.codingpedia.org/ama/upgrade-from-tiles-2-to-tiles-3-on-spring-mvc-3-application/#more-931" target="_blank">Upgrade from Tiles 2 to Tiles 3 on Spring MVC application</a>
  3. <a title="Apache Tiles" href="http://tiles.apache.org/" target="_blank">http://tiles.apache.org/</a>
  4. <a title="Springsource" href="http://www.springsource.org/" target="_blank">http://www.springsource.org/</a>

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
