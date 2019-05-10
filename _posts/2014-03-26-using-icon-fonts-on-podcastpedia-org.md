---
id: 1274
title: Using icon fonts on Podcastpedia.org
date: 2014-03-26T07:43:22+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1274
permalink: /ama/using-icon-fonts-on-podcastpedia-org/
fsb_show_social:
  - 0
dsq_thread_id:
  - 2512346422
fsb_social_facebook:
  - 2
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - Podcastpedia.org
  - web design
tags:
  - css
  - css3
  - font
  - font icons
  - glyphicons
  - podcastpedia
  - responsive design
  - web design
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Icon_font_libraries">1. Icon font libraries</a>
    </li>
    <li>
      <a href="#2_How_to_use_Icon_Fonts">2. How to use Icon Fonts</a><ul>
        <li>
          <a href="#21_Font_formats">2.1. Font formats</a>
        </li>
        <li>
          <a href="#22_Pseudo-elements">2.2. Pseudo-elements</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_The_result">3. The result</a>
    </li>
    <li>
      <a href="#4_Conclusion">4. Conclusion</a>
    </li>
    <li>
      <a href="#5_RESOURCES">5. RESOURCES</a>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  Some time ago I discovered <a title="CSS Sprites" href="http://css-tricks.com/css-sprites/" target="_blank">CSS sprites </a>and I said to myself &#8220;what a cool thing, I must definitely use it on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, for all the existent social, flags and media icons&#8221;- by using CSS sprites you send only one HTTP request to get the bigger picture, instead of issuing individual HTTP requests for each icon.  When I finally rolled up my sleeves and built a CSS sprite for the icons, I had another revelation &#8211; I recalled having heard about icon fonts, which were supposed to be superior to using images as icons in every way (well maybe except the monochromatic part&#8230;).
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>

  <!--more-->
</p>

<h2 style="text-align: justify;">
  <span id="1_Icon_font_libraries">1. Icon font libraries</span>
</h2>

<p style="text-align: justify;">
  After an initial research on that topic, I&#8217;ve noticed there is a myriad of resources out there for icon fonts to choose from : <a title="http://fortawesome.github.io/Font-Awesome/" href="http://fortawesome.github.io/Font-Awesome/" target="_blank">Font Awesome</a>, <a title="http://glyphicons.com/" href="http://glyphicons.com/" target="_blank">Glyphicons</a>, <a title="http://icomoon.io/" href="http://icomoon.io/" target="_blank">IcoMoon</a> etc. For more of them you can check out out this <a title="http://css-tricks.com/flat-icons-icon-fonts/" href="http://css-tricks.com/flat-icons-icon-fonts/" target="_blank">compilation of icon fonts resources from css-tricks.com</a>.
</p>

<p style="text-align: justify;">
  For Podcastpedia I chose the ones from <a title="http://icomoon.io" href="http://icomoon.io" target="_blank">IcoMoon</a>, because I&#8217;ve heard of the website at a JavaScript/HTML 5 conference I attended in Munich and I really liked it after giving it an initial try. The IcoMoon app lets you build and use your own icon sets in different formats including SVG, icon font or simple PNG sprites (I have downloaded the font). You also get a <em>demo.html</em> file and a <em>style.css</em> demonstrating how you can use them.
</p>

<h2 style="text-align: justify;">
  <span id="2_How_to_use_Icon_Fonts">2. How to use Icon Fonts</span>
</h2>

<p style="text-align: justify;">
  I have included the parts of the generated <em>.css</em> from icomoon.io, into the style sheet file of Podcastpedia:
</p>

<pre class="lang:css decode:true" title="CSS snippet demonstrating use of icon fonts">/* icon fonts */
@font-face {
	font-family: 'podcastpedia';
	src:url('fonts/podcastpedia.eot?ezbxqx');
	src:url('fonts/podcastpedia.eot?#iefixezbxqx') format('embedded-opentype'),
		url('fonts/podcastpedia.woff?ezbxqx') format('woff'),
		url('fonts/podcastpedia.ttf?ezbxqx') format('truetype'),
		url('fonts/podcastpedia.svg?ezbxqx#podcastpedia') format('svg');
	font-weight: normal;
	font-style: normal;
}

[class^="icon-"], [class*=" icon-"] {
	font-family: 'podcastpedia';
	speak: none;
	font-style: normal;
	font-weight: normal;
	font-variant: normal;
	text-transform: none;
	line-height: 1;

	/* Better Font Rendering =========== */
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

.icon-home:before {
	content: "\e600";
}</pre>

<p style="text-align: justify;">
  The <code>@font-face</code> rule allows you to load and use your &#8220;own&#8221; custom fonts, that might not be installed on the user&#8217;s computer. You have to include the font file on the web server(via the src:url font descriptor) , and it will be automatically downloaded  to the user when needed.
</p>

<p class="note_normal">
  <b>Note:</b> Internet Explorer 8 and earlier versions, do not support the <code>@font-face</code> rule.
</p>

### <span id="21_Font_formats">2.1. Font formats</span>

As you can see, based on the file extension, there are several font formats included:

**<a title="http://en.wikipedia.org/wiki/Embedded_OpenType" href="http://en.wikipedia.org/wiki/Embedded_OpenType" target="_blank">Embedded OpenType Fonts (EOT)<br /> </a>**EOT fonts are a compact form of OpenType fonts designed by Microsoft for use as embedded fonts on web pages.

<p style="text-align: justify;">
  <strong><a title="http://en.wikipedia.org/wiki/Web_Open_Font_Format" href="http://en.wikipedia.org/wiki/Web_Open_Font_Format" target="_blank">The Web Open Font Format (WOFF)<br /> </a></strong>WOFF is a font format for use in web pages. It was developed in 2009, and is now a W3C Recommendation. WOFF is essentially OpenType or TrueType with compression and additional metadata. The goal is to support font distribution from a server to a client over a network with bandwidth constraints.
</p>

<p style="text-align: justify;">
  <strong><a title="http://en.wikipedia.org/wiki/TrueType" href="http://en.wikipedia.org/wiki/TrueType" target="_blank">TrueType Fonts (TTF)</a><br /> </strong>TrueType is an outline font standard developed by Apple and Microsoft in the late 1980s as a competitor to Adobe&#8217;s Type 1 fonts used in PostScript. It has become the most common format for fonts on both the Mac OS and Microsoft Windows operating systems.
</p>

<p style="text-align: justify;">
  <strong><a title="http://www.w3.org/TR/SVG11/fonts.html#Introduction" href="http://www.w3.org/TR/SVG11/fonts.html#Introduction" target="_blank">SVG Fonts/Shapes</a><br /> </strong>SVG fonts allow SVG to be used as glyphs when displaying text. The SVG 1.1 specification define a font module that allows the creation of fonts within an SVG document. You can also apply CSS to SVG documents, and the <code>@font-face</code> rule can be applied to text in SVG documents.
</p>

<p class="note_normal">
  <strong>Note:</strong> The Web Open Font Format (WOFF) is a W3C recommendation&#8230;
</p>

<h3 style="text-align: justify;">
  <span id="22_Pseudo-elements">2.2. Pseudo-elements</span>
</h3>

<p style="text-align: justify;">
  A very nice way to include the icon fonts in a web page is by the using  the CSS <a title="https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements" href="https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements" target="_blank">pseudo-elements</a> <code>:before</code> and <code>:after</code>, which can be used to insert some content before, respectively after, the content of an element.
</p>

For example the _broadcast icon_ _font_ in the logo[<img class="alignnone size-large wp-image-1286" src="{{site.url}}/wp-content/uploads/2014/03/Logo-broadcast-1024x111.png" alt="Broadcast font icon" width="604" height="65" srcset="{{site.url}}/wp-content/uploads/2014/03/Logo-broadcast-1024x111.png 1024w, {{site.url}}/wp-content/uploads/2014/03/Logo-broadcast-300x32.png 300w, {{site.url}}/wp-content/uploads/2014/03/Logo-broadcast.png 1102w" sizes="(max-width: 604px) 100vw, 604px" />]({{site.url}}/wp-content/uploads/2014/03/Logo-broadcast.png)

is inserted with the help of the pseudo element `:after`. This is applied to the logo hyperlink element <a>, which has a class of icon-podcast :

<pre class="lang:default decode:true" title="HTML snippet - Podcastpedia.org logo">&lt;a href="/" class="icon-podcast"&gt;&lt;span id="logo_title"&gt;Podcastpedia.org&lt;/span&gt;&lt;/a&gt;</pre>

<pre class="lang:css decode:true" title=":after pseudo-element example">.icon-podcast:after {
	content: "\e609";
	color: #DF7401;
}</pre>

I use the same procedure (could be `:before` ) to insert the other icon fonts present on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>

## <span id="3_The_result"><span style="font-family: Bitter, Georgia, serif; font-size: 30px; line-height: 1.3; text-align: justify;">3. The result</span></span>

<span style="line-height: 1.5;">Please visit the website </span><a style="line-height: 1.5;" title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org </a><span style="line-height: 1.5;">to see the icon fonts in action. They are present in menu, social icons, feed icon etc. :<a href="{{site.url}}/wp-content/uploads/2014/03/font-icons-podcastpedia.png"><img class="alignnone size-large wp-image-1283" src="{{site.url}}/wp-content/uploads/2014/03/font-icons-podcastpedia-1024x558.png" alt="Font icons on Podcastpedia.org" width="604" height="329" srcset="{{site.url}}/wp-content/uploads/2014/03/font-icons-podcastpedia-1024x558.png 1024w, {{site.url}}/wp-content/uploads/2014/03/font-icons-podcastpedia-300x163.png 300w, {{site.url}}/wp-content/uploads/2014/03/font-icons-podcastpedia.png 1075w" sizes="(max-width: 604px) 100vw, 604px" /></a></span>

<p class="note_normal" style="text-align: justify;">
  <span style="line-height: 1.5;"><strong> Note:</strong> Not only the looks of the webiste got better, but there was also an increase of performance results on <a title="https://developers.google.com/speed/pagespeed/insights/" href="https://developers.google.com/speed/pagespeed/insights/" target="_blank">Google PageSpeed Insights</a>. </span>
</p>

## <span id="4_Conclusion">4. Conclusion</span>

As a conclusion I would like to mention I am very happy right now with the icon fonts and I would like to mention some of the benefits of using icon fonts :

  * they scale well (big advantage in <a title="Responsive web design" href="http://en.wikipedia.org/wiki/Responsive_web_design" target="_blank"><em>responsive design</em></a> )
  * you can easily change their color
  * you can easily add shadows to their shapes
  * can have transparent knockouts
  * don&#8217;t require individual HTTP requests
  * have good support in browsers
  * you can do all the other stuff image based icons can do, like change opacity, rotation etc.

Thank you for sharing and connecting with us.



<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="5_RESOURCES">5. RESOURCES</span>

  * <a title="icomoon.io" href="http://icomoon.io" target="_blank">http://icomoon.io/ &#8211; Custom Built and Crisp Icon Fonts, Done Right</a>
  * <a title="http://en.wikipedia.org/wiki/Glyph" href="http://en.wikipedia.org/wiki/Glyph" target="_blank">Wikipedia &#8211; Glyph</a>
  * <a title="http://www.vanseodesign.com/web-design/icon-fonts/" href="http://www.vanseodesign.com/web-design/icon-fonts/" target="_blank">Why And How To Use Icon Fonts</a>
  * <a title="http://css-tricks.com/flat-icons-icon-fonts/" href="http://css-tricks.com/flat-icons-icon-fonts/" target="_blank">The Big List of Flat Icons & Icon Fonts</a>
  * <a title="http://www.w3schools.com/css/css3_fonts.asp" href="http://www.w3schools.com/css/css3_fonts.asp" target="_blank">CSS3 Web Fonts</a>
  * <a title="http://en.wikipedia.org/wiki/Web_Open_Font_Format" href="http://en.wikipedia.org/wiki/Web_Open_Font_Format" target="_blank">Web Open Font Format &#8211; WOFF </a>

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
