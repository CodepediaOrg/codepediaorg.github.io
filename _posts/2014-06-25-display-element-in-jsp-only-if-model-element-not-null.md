---
id: 1604
title: Display element in JSP only if model element not null
date: 2014-06-25T17:45:49+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1604
permalink: /ama/display-element-in-jsp-only-if-model-element-not-null/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 0
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2794470377
categories:
  - web design
tags:
  - html
  - jsp
  - jstl
format: aside
---
**How to display element in JSP only if model element not null**

To do that you can use the <c:_if> or <c:choose>_ tags to make conditional rendering in Java Server Pages (JSP) using the JSP Standard Tag Library (JSTL) like in the following code snippet:

<pre class="lang:xhtml decode:true crayon-selected" title="Example ">&lt;div&gt;
	&lt;c:if test="${not empty podcast.twitterPage}"&gt;
		&lt;a href="${podcast.twitterPage}" target="_blank" class="icon-twitter-producer producer-social"&gt;&lt;/a&gt;
	&lt;/c:if&gt; 			 		
	&lt;c:if test="${not empty podcast.fbPage}"&gt;
		&lt;a href="${podcast.fbPage}" target="_blank" class="icon-facebook-producer producer-social"&gt;&lt;/a&gt;
	&lt;/c:if&gt; 			 		
	&lt;c:if test="${not empty podcast.gplusPage}"&gt;
		&lt;a href="${podcast.gplusPage}" target="_blank" class="icon-google-plus-producer producer-social"&gt;&lt;/a&gt;
	&lt;/c:if&gt; 
&lt;/div&gt;</pre>

<span style="line-height: 1.5;"><a title="https://github.com/podcastpedia/podcastpedia-web" href="https://github.com/podcastpedia/podcastpedia-web" target="_blank">Podcastpedia.org on GitHub</a></span>