---
id: 1501
title: How to add a GitHub ribbon to Podcastpedia.org
date: 2014-06-30T21:58:49+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1501
permalink: /ama/how-to-add-a-github-ribbon-to-podcastpedia-org/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:55:"https://github.com/podcastpedia/css-generator-sass-gulp";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 2807814421
fsb_social_facebook:
  - 1
fsb_social_google:
  - 6
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 1
fsb_social_pinterest:
  - 0
categories:
  - Podcastpedia.org
  - web design
tags:
  - css
  - css3
  - GitHub
  - open source
  - sass
  - scss
---
<p style="text-align: justify;">
  Since the source code for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> is now open <a title="https://github.com/podcastpedia/podcastpedia-web" href="https://github.com/podcastpedia/podcastpedia-web" target="_blank">on GitHub</a> for everyone to have look at and contribute to, I might as well add a ribbon on the website celebrating that. After a short research on the web, I stepped over Simon Whitaker&#8217;s project <strong><a style="color: #4183c4;" href="https://github.com/simonwhitaker/github-fork-ribbon-css">github-fork-ribbon-css</a> </strong>which I could easily integrate in  the website:
</p>

<p style="text-align: justify;">
  <a href="{{site.url}}/wp-content/uploads/2014/06/ribbon.png"><img class="alignnone size-full wp-image-1507" src="{{site.url}}/wp-content/uploads/2014/06/ribbon.png" alt="GitHub ribbon" width="574" height="175" srcset="{{site.url}}/wp-content/uploads/2014/06/ribbon.png 574w, {{site.url}}/wp-content/uploads/2014/06/ribbon-300x91.png 300w" sizes="(max-width: 574px) 100vw, 574px" /></a>
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>
</p>

<p style="text-align: justify;">
  In this post I&#8217;ll present the two simple steps required for that:
</p>

<p style="text-align: justify;">
  <!--more-->
</p>

<h3 style="text-align: justify;">
  1. Transform the css file into scss
</h3>

<p style="text-align: justify;">
  As you might recall from my blog post <a title="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" href="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" target="_blank">CSS Preprocessors – Introducing Sass to Podcastpedia.org</a>, I am generating the css files needed for Podcastpedia.org from Sass, so I transformed the css file needed for Github ribbon &#8211; <a title="https://github.com/simonwhitaker/github-fork-ribbon-css/blob/b3305a5555123a30502c1feb4ff23047030dc3f0/gh-fork-ribbon.css" href="https://github.com/simonwhitaker/github-fork-ribbon-css/blob/b3305a5555123a30502c1feb4ff23047030dc3f0/gh-fork-ribbon.css" target="_blank">gh-fork-ribbon.css</a>, into the following:
</p>

<pre class="lang:sass decode:true" title="scss version of the gh-fork-ribbon.css">/*!
 * "Fork me on GitHub" CSS ribbon v0.1.1 | MIT License
 * https://github.com/simonwhitaker/github-fork-ribbon-css
*/

/* Left will inherit from right (so we don't need to duplicate code) */
.github-fork-ribbon {
  /* The right and left classes determine the side we attach our banner to */
  position: absolute;
  /* Add a bit of padding to give some substance outside the "stitching" */
  padding: 2px 0;
  /* Set the base colour */
  background-color: #a00;
  /* Set a gradient: transparent black at the top to almost-transparent black at the bottom */
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.15));
  /* Add a drop shadow */
  box-shadow: 0 2px 3px 0 rgba(0, 0, 0, 0.5);
  /* Set the font */
  font: 700 13px "Helvetica Neue", Helvetica, Arial, sans-serif;
  z-index: 9999;
  pointer-events: auto;
  a {
    /* Set the text properties */
    color: #fff;
    text-decoration: none;
    text-shadow: 0 -1px rgba(0, 0, 0, 0.5);
    text-align: center;
    /* Set the geometry. If you fiddle with these you'll also need
       to tweak the top and right values in .github-fork-ribbon. */
    width: 200px;
    line-height: 20px;
    /* Set the layout properties */
    display: inline-block;
    padding: 2px 0;
    /* Add "stitching" effect */
    border-width: 1px 0;
    border-style: dotted;
    border-color: #fff;
    border-color: rgba(255, 255, 255, 0.7);
    &:hover {
      /* Set the text properties */
      color: #fff;
      text-decoration: none;
      text-shadow: 0 -1px rgba(0, 0, 0, 0.5);
      text-align: center;
      /* Set the geometry. If you fiddle with these you'll also need
         to tweak the top and right values in .github-fork-ribbon. */
      width: 200px;
      line-height: 20px;
      /* Set the layout properties */
      display: inline-block;
      padding: 2px 0;
      /* Add "stitching" effect */
      border-width: 1px 0;
      border-style: dotted;
      border-color: #fff;
      border-color: rgba(255, 255, 255, 0.7);
    }
  }
}

.github-fork-ribbon-wrapper {
  width: 150px;
  height: 150px;
  position: absolute;
  overflow: hidden;
  top: 0;
  z-index: 9999; //should really be on top of everything
  pointer-events: none;
  @include until-mq(1340px){
    display: none;
  }
  &.fixed {
    position: fixed;
  }
  &.left {
    left: 0;
  }
  &.right {
    right: 0;
  }
  &.left-bottom {
    position: fixed;
    top: inherit;
    bottom: 0;
    left: 0;
  }
  &.right-bottom {
    position: fixed;
    top: inherit;
    bottom: 0;
    right: 0;
  }
}

$rotation-angle: 45deg;
.github-fork-ribbon-wrapper {
  &.right .github-fork-ribbon {
    top: 42px;
    right: -43px;
    transform: rotate($rotation-angle);
  }
  &.left .github-fork-ribbon {
    top: 42px;
    left: -43px;
    transform: rotate(-$rotation-angle);
  }
  &.left-bottom .github-fork-ribbon {
    top: 80px;
    left: -43px;
    transform: rotate($rotation-angle);
  }
  &.right-bottom .github-fork-ribbon {
    top: 80px;
    right: -43px;
    transform: rotate(-$rotation-angle);
  }
}</pre>

<p style="text-align: justify;">
  In Podcastpedia&#8217;s case the ribbon will be displayed only on big monitors with resolutions bigger than 1340px&#8230; This shouldn&#8217;t pose a problem for developers as they should use at least screens with full HD resolutions.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> By using the <em>gulp-autoprefixer</em> plugin, there was no need to replicate the CSS vendor prefixes present in the original .css file. See my post <a title="http://www.codingpedia.org/ama/how-to-use-gulp-to-generate-css-from-sass-scss/" href="http://www.codingpedia.org/ama/how-to-use-gulp-to-generate-css-from-sass-scss/" target="_blank">How to use Gulp to generate CSS from Sass/scss</a>  for more details about that.
</p>

### 2. Add code snippet after the body tag

Following the instrunctions from GitHub, the second step was to add the highlighted html code snippet after the body tag:

<pre class="lang:xhtml mark:2-7 decode:true" title="Top left ribbon html snippet">&lt;body id="&lt;tiles:insertAttribute name="body-id" /&gt;"&gt;
	&lt;!-- TOP LEFT RIBBON: START COPYING HERE --&gt;
	&lt;div class="github-fork-ribbon-wrapper left"&gt;
		&lt;div class="github-fork-ribbon"&gt;
			&lt;a href="https://github.com/podcastpedia/podcastpedia-web" target="_blank"&gt;Fork me on GitHub&lt;/a&gt;
		&lt;/div&gt;
	&lt;/div&gt;
	...............
&lt;/body&gt;</pre>

Well, it cannot get simpler than that&#8230; So thanks again <a title="https://github.com/simonwhitaker" href="https://github.com/simonwhitaker" target="_blank">Simon</a> for this nice peace of work.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## Resources

### GitHub

  * <a title="https://github.com/simonwhitaker/github-fork-ribbon-css" href="https://github.com/simonwhitaker/github-fork-ribbon-css" target="_blank">simonwhiteker/github-fork-ribbon-css</a>

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
