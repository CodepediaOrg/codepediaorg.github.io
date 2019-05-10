---
id: 5
title: Troubleshooting WordPress installation on GoDaddy VPS
date: 2013-07-25T14:50:14+00:00
author: ama
layout: post
guid: http://www.codingpedia.org/?p=5
permalink: /ama/troubleshooting-wordpress-on-godaddy-vps-account/
dsq_thread_id:
  - 1533849201
fsb_social_facebook:
  - 3
fsb_show_social:
  - 0
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - troubleshooting
tags:
  - apache
  - GoDaddy
  - installation
  - mysql
  - php
  - tiles
  - wordpress
---
My first entry in the blogosphere is not &#8220;About us&#8221;, but about the problems I encountered trying to install WordPress on my GoDaddy Virtual Private Server (VPS).

<p class="western">
  For the installation I followed the steps from <a title="Codex Installing WordPress" href="http://codex.wordpress.org/Installing_WordPress">Installing WordPress</a>. Everything went fine until <em>Step 5: Run the Install Script</em>. After calling <tt>http://codingpedia.org/wp-admin/install.php</tt> , my excitement to have finished quickly finished the installation was ruined by the infamous
</p>

<p class="western" style="padding-left: 30px;">
  <em><strong>Internal Server Error </strong></em><br /> <em>The server encountered an internal error or misconfiguration and was unable to complete your request.</em><br /> <em>Please contact the server administrator, webmaster@codingpedia.org and inform them of the time the error occurred, and anything you might have done that may have caused the error.</em><br /> <em>More information about this error may be available in the server error log.</em>
</p>

<p class="western" style="padding-left: 30px;">
  <em>Additionally, a 404 Not Found error was encountered while trying to use an ErrorDocument to handle the request.</em>
</p>

<!--more-->

<p class="western">
  After spending a couple of hours researching the logs and the web it occured to me I had uploaded the files to my webserver via SSH and all the wordpress files had the owner and group set to <strong><em>root</em></strong>. My <strong>PHP 5 handler</strong> was set to <strong>suphp</strong>. After correcting the user and owner as the ones specified in my virtual host configuration for the domain:
</p>

<pre class="lang:default decode:true" title="Virtual Host configuration mod_suphp">ServerName codingpedia.org
...
UserDir enabled my_user
&lt;IfModule mod_suphp.c&gt;
	suPHP_UserGroup my_user my_user
&lt;/IfModule&gt;
...</pre>

, this problem was solved.

But when trying run the install script <tt>http://codingpedia.org/wp-admin/install.php</tt> again, I got another infamous HTTP error code &#8211; 403

<p style="padding-left: 30px;">
  <em><strong>Forbidden</strong> </em><br /> <em>You don&#8217;t have permission to access / on this server.</em><br /> <em>Additionally, a 403 Forbidden error was encountered while trying to use an ErrorDocument to handle the request.</em>
</p>

The cause was the same &#8211; uploading the files over SSH and not having the right file permissions. After <a title="Codex WordPress Changing File Permissions" href="http://codex.wordpress.org/Changing_File_Permissions" target="_blank">setting the proper file permissions</a> the problem was solved.

My third attempt to execute the install script ended also in failure &#8211; this time

<p style="padding-left: 30px;">
  <em>Your PHP installation appears to be missing the MySQL extension which is required by WordPress.</em>
</p>

Uncommenting the mysql extensions in _php.ini_ did not help alone

<pre class="brush: plain; title: ; notranslate" title="">...
extension=mysql.so
extension=mysqli.so
...
</pre>

I also had to recompile Apache with _EasyApache_ and check MySql options from _Exhaustive Options List_:

<p style="padding-left: 30px;">
  <a href="{{site.url}}/wp-content/uploads/2013/07/enable_mysql_easyapache.png"><img class="alignnone size-medium wp-image-28" src="{{site.url}}/wp-content/uploads/2013/07/enable_mysql_easyapache-300x132.png" alt="enable_mysql_easyapache" width="361" height="160" srcset="{{site.url}}/wp-content/uploads/2013/07/enable_mysql_easyapache-300x132.png 300w, {{site.url}}/wp-content/uploads/2013/07/enable_mysql_easyapache-1024x452.png 1024w, {{site.url}}/wp-content/uploads/2013/07/enable_mysql_easyapache-624x275.png 624w, {{site.url}}/wp-content/uploads/2013/07/enable_mysql_easyapache.png 1287w" sizes="(max-width: 361px) 100vw, 361px" /></a>
</p>

Lucky number four &#8211; installation was successful, but as you can see I am still using the default WordPress theme and didn&#8217;t have enough time to edit anything as I have a plane to catch in two hours. But I promise to come back soon with an &#8220;About us&#8221; page and more coding related posts&#8230;
