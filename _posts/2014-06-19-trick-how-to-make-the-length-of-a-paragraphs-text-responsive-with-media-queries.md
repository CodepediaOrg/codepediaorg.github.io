---
id: 1516
title: 'Trick &#8211; how to make the length of a paragraph&#8217;s text responsive with media queries'
date: 2014-06-19T14:27:08+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1516
permalink: /ama/trick-how-to-make-the-length-of-a-paragraphs-text-responsive-with-media-queries/
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:31:"https://github.com/podcastpedia";s:11:"ribbon-type";i:10;}'
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
  - 2777914499
categories:
  - 'tips &amp; tricks'
  - web design
tags:
  - css
  - css3
  - jsp
  - jstl
  - media queries
  - responsive
  - sass
  - scss
  - tip
  - tips and tricks
  - trick
  - web design
---
<p style="text-align: justify;">
  In this post I will present you a simple trick about how to make the length of the text shown in a paragraph responsive. The example presented is written in Java Server Pages (JSP) and makes use of JavaServer Pages Standard Tag Library (JSTL), but you can use the same trick with other technologies and media queries as you will find out in the coming paragraphs.<!--more-->
</p>

<h2 style="text-align: justify;">
  The need
</h2>

<p style="text-align: justify;">
  All of the podcasts and episodes present on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, have a description, whose length varies between 0 and 1000 characters (more than that is not stored in the database). To keep the length of that text shown to some extent responsive meaning not to get too long for small devices, I&#8217;ve decided that for screen with resolutions smaller than 720px, the length should not be more than 300 characters and for bigger resolutions should be limited to 600 characters (you know with podcasts you shouldn&#8217;t tell your story in text anyway&#8230;)
</p>

<h2 style="text-align: justify;">
  The trick
</h2>

<p style="text-align: justify;">
  How did I achieve that?<br /> Very simple:
</p>

  1. include in the html/JSP code the same information twice
  2. use the <a title="http://docs.oracle.com/javaee/5/jstl/1.1/docs/tlddocs/fn/substring.fn.html" href="http://docs.oracle.com/javaee/5/jstl/1.1/docs/tlddocs/fn/substring.fn.html" target="_blank">fn:substring()</a> function to get different subsets of the string(one time 300, and the other time 600); I am sure there are similar functionalities offered by other languages/frameworks&#8230;
  3. use media queries to show and hide the paragraphs at the same time based on the screen&#8217;s width

### HTML/JSP part

<p style="text-align: justify;">
  This is how it looks in JSP:
</p>

<pre class="lang:xhtml decode:true" title="Doubling the information in HTML">....................
&lt;div class="ep_desc"&gt;
	${fn:substring(episode.description,0,300)}
&lt;/div&gt;
&lt;div class="ep_desc_bigger"&gt;
	${fn:substring(episode.description,0,600)}
&lt;/div&gt;
....................</pre>

<p class="note_code">
  <strong>Code tip:</strong> You can find the source code for the JSP file on GitHub &#8211; <a title="https://github.com/podcastpedia/podcastpedia-web/blob/be94ceaeca9b0545602371b5f8862d3b5f397261/src/main/webapp/WEB-INF/jsp/episode/episode_details_m.jsp" href="https://github.com/podcastpedia/podcastpedia-web/blob/be94ceaeca9b0545602371b5f8862d3b5f397261/src/main/webapp/WEB-INF/jsp/episode/episode_details_m.jsp" target="_blank">episode_details_m.jsp</a>
</p>

### CSS part

The CSS code supporting this is the following:

<pre class="lang:css decode:true" title="Media queries in CSS">@media screen and (min-width: 720px) {
    .ep_desc {
		display: none;
	 }
}

@media screen and (max-width: 719px) {
    .ep_desc_bigger {
		display: none;
	 }
}</pre>

<p class="note_code">
  <strong>Code tip:</strong> As you might recall from the post <a title="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" href="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" target="_blank">CSS Preprocessors – Introducing Sass to Podcastpedia.org</a>, the CSS for Podcastpedia.org gets generated from SASS with Gulp.
</p>

Well, that&#8217;s it. If you have a better proposal to achieve this please leave a comment!!! 10x

## Resources

### <span id="41_Source_code">Source code</span>

  * #### GitHub

      * Repositories
          * <a style="color: #bc360a;" title="https://github.com/podcastpedia/css-generator-sass-gulp" href="https://github.com/podcastpedia/css-generator-sass-gulp" target="_blank">https://github.com/podcastpedia/css-generator-sass-gulp</a>
          * <a title="https://github.com/podcastpedia/podcastpedia-web" href="https://github.com/podcastpedia/podcastpedia-web" target="_blank">podcastpedia-web (JSPs, java backend&#8230;)</a>
      * Relevant files

### <span id="42_Web">Web</span>

  1. [Oracle &#8211; JavaServer Pages Standard Tag Library](http://www.oracle.com/technetwork/java/index-jsp-135995.html "http://www.oracle.com/technetwork/java/index-jsp-135995.html")

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

Featured image courtesy of 2nix / FreeDigitalPhotos.net
