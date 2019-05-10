---
id: 283
title: 'How To: Enable compression and leverage browser caching with Apache Server'
date: 2013-08-10T20:00:17+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=283
permalink: /ama/how-to-enable-compression-and-leverage-browser-caching-with-apache-server/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 1
dsq_thread_id:
  - 1593164121
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - optimization
tags:
  - apache
  - deflate
  - GoDaddy
  - mobile
  - pagespeed
  - site optimization
---
Looking for ways to improve <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>&#8216; mobile user-friendliness, I have discovered Google&#8217;s <a title="PageSpeed Insights" href="http://developers.google.com/speed/pagespeed/insights/" target="_blank">PageSpeed Insights</a> . After analyzing the <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">home page</a>, a podcast&#8217;s page (<a title="The Naked Scientist Podcast" href="https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science" target="_blank">The Naked Scientists Podcast &#8211; Stripping Down Science</a>), an <a title="Episode page" href="https://github.com/Codingpedia/podcastpedia/podcasts/967/Leonardo-Evo-Solution-Die-Serie-zum-Darwin-Jahr/episodes/2/WDR-5-Leonardo-Evo-Solution-Die-Serie-im-Darwin-Jahr-Folge-12-Prima-Klima-vom-30-04-2009" target="_blank">episode page</a> and scoring an average of 70 out of 100 (even worse on mobile) two points needed urgent fixing:

### 1. Enabling compression

Compressing resources with gzip or deflate will reduce the number of bytes sent over the network at the expense of slightly increased CPU utilization. Smaller files also mean less bandwidth used and faster transfer time. At Google&#8217;s <a title="Enable Compression" href="https://developers.google.com/speed/docs/insights/EnableCompression" target="_blank">recommendation</a> I have enabled file compression on the Apache web server, for which you have to use the <a title="Mod_deflate module" href="http://httpd.apache.org/docs/current/mod/mod_deflate.html" target="_blank">mod_deflate</a> module.

#### 1.1 Enable mod_deflate on GoDaddy

To enable  <span style="background-color: #e6e6e6; padding: 0px 3px;">mod_deflate</span> on a GoDaddy Virtual Private Server account, you have to use _Easy Apache_ and the same procedure as in <a title="Troubleshooting wordpress on GoDaddy" href="http://www.codingpedia.org/ama/troubleshooting-wordpress-on-godaddy-vps-account/" target="_blank">Troubleshooting WordPress installation on GoDaddy VPS</a>. You need to check the <span style="background-color: #e6e6e6; padding: 0px 3px;">Deflate</span> option at step number _5. Exhaustive Options List_ and recompile the Apache Server.

[<img class="alignnone size-medium wp-image-288" src="{{site.url}}/wp-content/uploads/2013/08/easy_apache_deflate-300x126.png" alt="easy_apache_deflate" width="300" height="126" srcset="{{site.url}}/wp-content/uploads/2013/08/easy_apache_deflate-300x126.png 300w, {{site.url}}/wp-content/uploads/2013/08/easy_apache_deflate-1024x432.png 1024w, {{site.url}}/wp-content/uploads/2013/08/easy_apache_deflate-624x263.png 624w, {{site.url}}/wp-content/uploads/2013/08/easy_apache_deflate.png 1028w" sizes="(max-width: 300px) 100vw, 300px" />]({{site.url}}/wp-content/uploads/2013/08/easy_apache_deflate.png)

To verify the module <span style="background-color: #e6e6e6; padding: 0px 3px;">mod_deflate</span> is active run the following command on CentOS

<pre>httpd -M | grep deflate_module</pre>

<!--more-->

#### 1.2 Configure mod_deflate

After researching the web and the Apache website, the best solution for my needs came from <a title="How to enable gzip compression on Apache " href="http://howtounix.info/howto/Apache-gzip-compression-with-mod_deflate" target="_blank">HowToUnix.info</a> &#8211; it&#8217;s similar with Apache&#8217;s suggestion and it also verifies if the different modules are active. By adding the suggested configuration

<pre style="padding-left: 30px;"><span class="tag">&lt;IfModule</span><span class="atn">mod_mime</span><span class="pln">.</span><span class="atn">c</span><span class="tag">&gt;</span><span class="pln">
 AddType application/x-javascript .js
 AddType text/css .css
</span><span class="tag">&lt;/IfModule&gt;
</span><span class="tag">&lt;IfModule</span><span class="atn">mod_deflate</span><span class="pln">.</span><span class="atn">c</span><span class="tag">&gt;
</span><b><span class="pln">  AddOutputFilterByType DEFLATE text/css application/x-javascript text/x-component text/html text/richtext image/svg+xml text/plain text/xsd text/xsl text/xml image/x-icon application/javascript
</span></b><span class="tag">&lt;IfModule</span><span class="atn">mod_setenvif</span><span class="pln">.</span><span class="atn">c</span><span class="tag">&gt;</span><span class="pln">
  BrowserMatch ^Mozilla/4 gzip-only-text/html
  BrowserMatch ^Mozilla/4\.0[678] no-gzip
  BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
 </span><span class="tag">&lt;/IfModule&gt;</span><span class="tag">&lt;IfModule</span><span class="atn">mod_headers</span><span class="pln">.</span><span class="atn">c</span><span class="tag">&gt;</span><span class="pln">
  Header append Vary User-Agent env=!dont-vary
 </span><span class="tag">&lt;/IfModule&gt;</span><span class="tag">&lt;/IfModule&gt;</span></pre>

to the virtual host configuration the html, css, xml and javascript files were compressed and an 10% score improvement on PageSpeed Insights was achieved for all the tested pages.

<p style="padding-left: 30px;">
  <em><strong>Note:</strong> The same problem held true for <a title="Codingpedia.org" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>, but for that I modified the <span style="background-color: #e6e6e6; padding: 0px 3px;">.htaccess</span> file from my root directory and got the same impact:</em>
</p>

<pre>####################
# GZIP COMPRESSION #
####################
SetOutputFilter DEFLATE
AddOutputFilterByType DEFLATE text/html text/css text/plain text/xml application/x-javascript application/x-httpd-php
BrowserMatch ^Mozilla/4 gzip-only-text/html
BrowserMatch ^Mozilla/4\.0[678] no-gzip
BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
BrowserMatch \bMSI[E] !no-gzip !gzip-only-text/html
SetEnvIfNoCase Request_URI \.(?:gif|jpe?g|png)$ no-gzip
Header append Vary User-Agent env=!dont-vary</pre>

### 2. Leveraging browser caching

The second point was enabling **browser caching** for static resources, which can save a user time if they visit your site more than once. You can do that by configuring the <span style="background-color: #e6e6e6; padding: 0px 3px;">Expires</span> and <span style="background-color: #e6e6e6; padding: 0px 3px;">Cache-control</span> HTTP headers with Apache web server.

First I checked if the <a title="Apache Module mod_expires" href="http://httpd.apache.org/docs/current/mod/mod_expires.html" target="_blank">mod_expires</a> and <a title="Apache Module mod_headers" href="http://httpd.apache.org/docs/current/mod/mod_headers.html" target="_blank">mod_headers</a> modules are active on Apache modules by issuing <span style="background-color: #e6e6e6; padding: 0px 5px;">httpd -M</span>

All I had to do next is adding the following to the virtual host configuration for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, and to _.htaccess_ for <a title="Codingpedia" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>:

<pre class="brush: plain; title: ; notranslate" title=""># Feed
  ExpiresByType application/atom+xml      "access plus 10 hours"
  ExpiresByType application/rss+xml       "access plus 10 hours"

# Favicon (cannot be renamed)
  ExpiresByType image/x-icon              "access plus 1 week"

# Media: images, video, audio
  ExpiresByType audio/ogg                 "access plus 1 month"
  ExpiresByType image/gif                 "access plus 1 month"
  ExpiresByType image/jpeg                "access plus 1 month"
  ExpiresByType image/png                 "access plus 1 month"
  ExpiresByType video/mp4                 "access plus 1 month"
  ExpiresByType video/ogg                 "access plus 1 month"
  ExpiresByType video/webm                "access plus 1 month"
  ExpiresByType application/x-shockwave-flash "access plus 1 month"

# HTC files  (css3pie)
  ExpiresByType text/x-component          "access plus 1 month"

# Webfonts
  ExpiresByType application/vnd.ms-fontobject "access plus 1 month"
  ExpiresByType application/x-font-ttf    "access plus 1 month"
  ExpiresByType application/x-font-woff   "access plus 1 month"
  ExpiresByType font/opentype             "access plus 1 month"
  ExpiresByType image/svg+xml             "access plus 1 month"

# CSS and JavaScript
  ExpiresByType application/javascript    "access plus 1 week"
  ExpiresByType text/css                  "access plus 1 week"
  ExpiresByType application/x-javascript  "access plus 1 week"

&lt;/IfModule&gt;
&lt;IfModule mod_headers.c&gt;
        &lt;FilesMatch "\\.(ico|jpg|jpeg|png|gif|swf)$"&gt;
                Header set Cache-Control "max-age=2678400, public"
        &lt;/FilesMatch&gt;
        &lt;FilesMatch "\\.(css)$"&gt;
                Header set Cache-Control "max-age=604800, public"
        &lt;/FilesMatch&gt;
        &lt;FilesMatch "\\.(js)$"&gt;
                Header set Cache-Control "max-age=604800, private"
        &lt;/FilesMatch&gt;
        &lt;FilesMatch "\\.(x?html?|php)$"&gt;
                Header set Cache-Control "max-age=60, private, must-revalidate"
        &lt;/FilesMatch&gt;
                Header unset ETag
                Header unset Last-Modified
&lt;/IfModule&gt;
</pre>

This basically sets expiration dates for the different types of files. The result was another 10% improvement on PageSpeed Insights score.

Well, that&#8217;s it&#8230; Two small steps, I had no clue about until now, that boosted both the website&#8217;s performance and the PageSpeed Insights scores on Mobile and Desktop.

If you notice any room for improvement, please <a href="mailto:contact@codingepdia.org?Subject=Apache%20optimization" target="_top">contact us</a> or leave a message.

If you liked this, please show your support by <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">helping us</a> with <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>

We promise to only share high quality podcasts and episodes.

<h3 style="margin: 1.714285714rem 0px; padding: 0px; border: 0px; font-size: 1.142857143rem; vertical-align: baseline; clear: both; line-height: 1.846153846; color: #444444; font-family: 'Open Sans', Helvetica, Arial, sans-serif; font-style: normal; font-variant: normal; letter-spacing: normal; orphans: auto; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffffff;">
  References
</h3>

<ul style="margin: 0px 0px 1.714285714rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline; list-style: disc outside; line-height: 24px; color: #444444; font-family: 'Open Sans', Helvetica, Arial, sans-serif; font-style: normal; font-variant: normal; font-weight: normal; letter-spacing: normal; orphans: auto; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffffff;">
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="Apache.org" href="http://www.apache.org" target="_blank">http://www.apache.org</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="PageSpeed Insights" href="http://developers.google.com/speed/pagespeed/insights/" target="_blank">PageSpeed Insights</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="How to enable gzip compression on Apache " href="http://howtounix.info/howto/Apache-gzip-compression-with-mod_deflate" target="_blank">http://howtounix.info/howto/Apache-gzip-compression-with-mod_deflate</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="Stackoverflow resource" href="http://stackoverflow.com/questions/2676744/set-http-caching-expiration-recommended-by-google-pagespeed" target="_blank">http://stackoverflow.com/questions/2676744/set-http-caching-expiration-recommended-by-google-pagespeed</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="Stackoverflow resource" href="http://stackoverflow.com/questions/2932890/http-cache-control-max-age-must-revalidate" target="_blank">http://stackoverflow.com/questions/2932890/http-cache-control-max-age-must-revalidate</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="Better Explained - How To Optimize Your Site With HTTP Caching" href="http://betterexplained.com/articles/how-to-optimize-your-site-with-http-caching/http://" target="_blank">http://betterexplained.com/articles/how-to-optimize-your-site-with-http-caching/</a>
  </li>
</ul>

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
