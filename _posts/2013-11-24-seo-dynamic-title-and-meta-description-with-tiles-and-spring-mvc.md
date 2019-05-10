---
id: 972
title: 'SEO: Dynamic title and meta description with Tiles and Spring MVC'
date: 2013-11-24T16:08:33+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=972
permalink: /ama/seo-dynamic-title-and-meta-description-with-tiles-and-spring-mvc/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
dsq_thread_id:
  - 1994982939
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - seo
  - spring
tags:
  - meta description
  - seo
  - spring
  - tiles
---
<p style="text-align: justify;">
  Every webmaster should know by now, that the <code>&lt;title&gt;</code> of a webpage, is one of the most important factors for ranking in the search results. Not only it is the title of the tab or browser windows, but it&#8217;s also the first line people see in the search results, followed by the URL and the snippet(this is usually the content of the <code>&lt;meta name="description"/&gt;</code> combined maybe with a date):
</p>

<div id="attachment_959" style="width: 721px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/11/search-results-snippet.png"><img class="size-full wp-image-959" src="{{site.url}}/wp-content/uploads/2013/11/search-results-snippet.png" alt="Search results - print screen snippet" width="711" height="268" srcset="{{site.url}}/wp-content/uploads/2013/11/search-results-snippet.png 711w, {{site.url}}/wp-content/uploads/2013/11/search-results-snippet-300x113.png 300w" sizes="(max-width: 711px) 100vw, 711px" /></a>

  <p class="wp-caption-text">
    Search results &#8211; print screen snippet
  </p>
</div>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

<p style="text-align: justify;">
  You&#8217;ve seen in a previous post &#8211; <a title="SEO: Friendly URL construction with Spring MVC" href="http://localhost/wordpress/2013/11/09/seo-friendly-url-construction-with-spring-mvc/" target="_blank">SEO: Friendly URL construction with Spring MVC </a> &#8211; how to build (search engine) friendly URLs (or permalinks). Well, in this post I will present how to generate dynamic titles and meta descriptions with Tiles on top of a Spring MVC application, which currently powers <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>.
</p>

<p style="text-align: justify;">
  <!--more-->
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> A prerequisite to understanding this is to have some knowledge of integrating Tiles and Spring. You can see my posts <a title="Spring MVC and Apache Tiles integration example" href="http://www.codingpedia.org/ama/spring-mvc-and-apache-tiles-integration-example/" target="_blank">Spring MVC and Apache Tiles integration example</a>, <a title="Spring 3 and Tiles 2 integration" href="http://www.codingpedia.org/ama/spring-3-and-tiles-2-integration/" target="_blank">Spring 3 and Tiles 2 integration</a> and <a title="Upgrade from Tiles 2 to Tiles 3 on Spring MVC 3 application" href="http://www.codingpedia.org/ama/upgrade-from-tiles-2-to-tiles-3-on-spring-mvc-3-application/#more-931" target="_blank">Upgrade from Tiles 2 to Tiles 3 on Spring MVC 3 application </a>for that.
</p>

<p style="text-align: justify;">
  Of course your titles should be descriptive, concise, unique and accurate. Of course you your the meta description tag should have reasonable and relevant data for the page, so that it can be used by search engines to show in the snippet when it contains the keyword the searcher was looking for. There are plenty of resources on the web concerning that, some of which I listed in the resources section, but my focus in this post is on technicalities and how to populate these tags dynamically. So here we go.
</p>

After learning some of the Tiles concepts, let&#8217;s have a look at the Tiles template:

<pre class="lang:default mark:10,11 decode:true" title="Tiles template file ">&lt;!DOCTYPE HTML&gt;
&lt;%@ include file="/WEB-INF/template/includes.jsp" %&gt;

&lt;%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" %&gt;

&lt;html&gt;
	&lt;head&gt;
		&lt;meta name="google-site-verification" content="ZkFgaVcUEQ5HhjAA8-LOBUfcOY8Fh2PqiBqvM2xcFk0" /&gt;
		&lt;title&gt;&lt;tiles:insertAttribute name="title" ignore="true"/&gt;&lt;/title&gt;
		&lt;meta name="description" content="&lt;tiles:insertAttribute name="page_description" ignore="true"/&gt;"&gt;
		&lt;link href="&lt;c:url value="/static/css/style.min.m.css?v=1.4"/&gt;" rel="stylesheet" type="text/css"/&gt;
		&lt;link rel="icon" href="&lt;c:url value="/static/images/favicon.ico"/&gt;" type="image/x-icon" /&gt;
		&lt;link rel="shortcut icon" href="&lt;c:url value="/static/images/favicon.ico"/&gt;" type="image/x-icon" /&gt;
		&lt;meta charset="utf-8"&gt;
		&lt;meta property="og:image" content="&lt;tiles:insertAttribute name="og_image" ignore="true"/&gt;" /&gt;
		&lt;meta property="og:title" content="&lt;tiles:insertAttribute name="og_title" ignore="true"/&gt;" /&gt;
		&lt;meta property="og:description" content="&lt;tiles:insertAttribute name="og_desc" ignore="true"/&gt;"/&gt;
		&lt;link rel="stylesheet" href="&lt;tiles:insertAttribute name="jquery_ui_css" ignore="true"/&gt;" /&gt;
		&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
	&lt;/head&gt;
    &lt;body&gt;
    	&lt;div id="banner"&gt;
			&lt;tiles:insertAttribute name="header" /&gt;
		&lt;/div&gt;
		&lt;div class="clear"&gt;&lt;/div&gt;
		&lt;tiles:insertAttribute name="navigation_bar" /&gt;
		&lt;div class="clear"&gt;&lt;/div&gt;
		&lt;div id="page"&gt;
			&lt;tiles:insertAttribute name="content" /&gt;
		&lt;/div&gt;
		&lt;div class="clear"&gt;&lt;/div&gt;
		&lt;div id="footer_wrapper"&gt;
			&lt;tiles:insertAttribute name="footer" /&gt;
		&lt;/div&gt;
	&lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  This represents the layout of the page and the <code>&lt;title&gt;</code> (line 10) and <code>&lt;meta name="description"&gt;</code> (line 11) tags are filled with the help of Tiles attributes.
</p>

<p style="text-align: justify;">
  To recap, an <b>attribute</b> is a gap in a template that needs to be filled in your application. An attribute can be of three types:
</p>

<li style="text-align: justify;">
  <b>string</b>: it is a string to be directly rendered as it is.
</li>
<li style="text-align: justify;">
  <b>template</b>: it is a template, with or without attributes. If it has attributes, you have to fill them too to render a page.
</li>
<li style="text-align: justify;">
  <b>definition</b>: it is a reusable composed page, with all (or some) attributes filled
</li>

Let&#8217;s have look now at the Tiles definition (`podcastDetails`), which renders the podcast details web page:

<pre class="lang:default mark:7,8 decode:true" title="Tiles configuration file">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd"&gt;

&lt;tiles-definitions&gt;
.............
    &lt;definition name="podcastDetails" extends="defaultTemplate"&gt;
    	&lt;put-attribute name="title" expression="${podcast.title}"/&gt;
 	    &lt;put-attribute name="page_description" expression="${podcast.description}"/&gt;
    	&lt;put-attribute name="navigation_bar" value="/WEB-INF/jsp/navigation_bar/podcast_details_navigation_bar.jsp" /&gt;
    	&lt;put-attribute name="content" value="/WEB-INF/jsp/podcastDetails.jsp"/&gt;
    	&lt;put-attribute name="og_title" expression="${podcast.title}"/&gt;
 	    &lt;put-attribute name="og_desc" expression="${podcast.description}"/&gt;
 	    &lt;put-attribute name="og_image" expression="${podcast.urlOfImageToDisplay}"/&gt;
    &lt;/definition&gt;
.............
&lt;/tiles-definitions&gt;</pre>

<p style="text-align: justify;">
  Notice here the line which fills the <strong>title</strong> of the web page : <code>&lt;put-attribute name="title" expression="${podcast.title}"/&gt;</code>. There is the <code>name="title"</code>, which is the same name as the attribute defined in the Template (<code>&lt;title&gt;&lt;tiles:insertAttribute name="title" ignore="true"/&gt;&lt;/title&gt;</code>). The value of the attribute is an attribute expression &#8211; <code>expression="${podcast.title}" - </code>which is interpreted in Unified EL. The Expression Language (EL) is supported since Tiles 2.1. The <code>podcast.title</code> is a property of a <code>Podcast</code> bean supplied by the Spring controller:
</p>

<pre class="lang:java mark:8,21,27 decode:true" title="Spring controller handling podcast details requests">@RequestMapping(value="{podcastId}/*", method=RequestMethod.GET)
public String getPodcastDetails(@PathVariable("podcastId") int podcastId,
							 ModelMap model,
							 HttpServletRequest httpRequest) throws BusinessException{

	LOG.debug("------ getPodcastDetails : Received request to show details for podcast id "	+ podcastId + " ------");

	Podcast  podcast = podcastService.getPodcastById(podcastId);

	//add the last episodes to be displayed under the podcast metadata
	List&lt;Episode&gt; lastEpisodes = null;
	if(podcast.getEpisodes().size() &gt; 6){
		lastEpisodes = podcast.getEpisodes().subList(1, 6);
	} else {
		lastEpisodes = podcast.getEpisodes();
	}
	model.addAttribute("lastEpisodes", lastEpisodes);
	model.addAttribute("nr_divs_with_ratings", lastEpisodes.size());
	if(podcast.getRating() == null) podcast.setRating(10f);
	model.addAttribute("roundedRatingScore", Math.round(podcast.getRating()));
	model.addAttribute("podcast", podcast);

	SitePreference currentSitePreference = SitePreferenceUtils.getCurrentSitePreference(httpRequest);
	if(currentSitePreference.isMobile() || currentSitePreference.isTablet()){
		return "m_podcastDetails";
	} else {
		return "podcastDetails";
	}
}</pre>

<p style="text-align: justify;">
  The podcast bean is supplied by backend services (line 8) and after that is added to the model (line 21) to be rendered in Tiles. The return value of the method &#8211; <code>podcastDetails</code> &#8211; is the same as the name for the Tiles definition that renders the page <code>&lt;definition name="podcastDetails" extends="defaultTemplate"&gt;</code>
</p>

<p class="note_normal">
  <strong>Note:</strong> To better understand the site preference part (lines 23-28) see my post <a title="Going mobile with Spring mobile and responsive web design" href="http://www.codingpedia.org/ama/going-mobile-with-spring-mobile-and-responsive-web-design/" target="_blank">Going mobile with Spring mobile and responsive web design</a>
</p>

<div id="social_logos_small">
  <p>
    The same mechanism is valid for filling the value of the <code>&lt;meta name="description"/&gt;</code> tag. Well, that&#8217;s it. Thanks for sharing.
  </p>

  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>
</div>

## Resources

  1. <a title="Spring MVC and Apache Tiles integration example" href="http://www.codingpedia.org/ama/spring-mvc-and-apache-tiles-integration-example/" target="_blank">Spring MVC and Apache tiles integration example</a>
  2. <a title="Apache Tiles Homepage" href="https://tiles.apache.org/" target="_blank">Apache Tiles homepage</a>
  3. <a title="Expression Language support" href="http://tiles.apache.org/framework/tutorial/advanced/el-support.html" target="_blank">Apache Tiles &#8211; Expression Language support</a>
  4. <a title="Site title and description - Google Webmaster tools help" href="https://support.google.com/webmasters/answer/35624?hl=en&ref_topic=2371375" target="_blank">Site title and description &#8211; Google Webmaster Tools help</a>
  5. <a title="Search engine optimization starter guide - Google" href="https://static.googleusercontent.com/media/www.google.com/en//webmasters/docs/search-engine-optimization-starter-guide.pdf" target="_blank">Search engine optimization starter guide &#8211; Google</a>
  6. <a title="[Video] Google & SEO Friendly Page Titles" href="http://www.seobook.com/video-google-seo-friendly-page-titles" target="_blank">[Video] Google & SEO Friendly Page Titles</a>
  7. <a title="WordPress SEO" href="yoast.com/articles/wordpress-seo/" target="_blank">WordPress SEO</a>

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
