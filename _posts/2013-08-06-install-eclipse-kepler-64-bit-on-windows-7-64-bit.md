---
id: 234
title: Install Eclipse Kepler 64-bit on Windows 7 64-bit
date: 2013-08-06T07:07:06+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=234
permalink: /ama/install-eclipse-kepler-64-bit-on-windows-7-64-bit/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
dsq_thread_id:
  - 1575185514
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
fsb_social_google:
  - 2
categories:
  - development
  - java
tags:
  - development
  - eclipse
  - jvm
  - tomcat
---
I recently switched to Eclipse Kepler, being very dissapointed in the Juno Version &#8211; sometimes it took a couple of seconds just to switch between open tabs, and that on a 16 GB RAM machine with a 8-core processor&#8230; Anyway, I will shortly present here how I have configured Eclipse for the further development of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org:</a>

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Download_Eclipse">Download Eclipse</a>
    </li>
    <li>
      <a href="#Launching_Eclipse">Launching Eclipse</a>
    </li>
    <li>
      <a href="#Install_Subversive_on_Eclipse">Install Subversive on Eclipse</a>
    </li>
    <li>
      <a href="#Import_your_local_repository_in_Eclipse">Import your local repository in Eclipse</a>
    </li>
    <li>
      <a href="#Sysdeo_Eclipse_Tomcat_Launcher_plugin">Sysdeo Eclipse Tomcat Launcher plugin</a>
    </li>
    <li>
      <a href="#Eclipse_Java_EE_Developer_Tools">Eclipse Java EE Developer Tools</a>
    </li>
    <li>
      <a href="#References">References</a>
    </li>
  </ul>
</div>

<!--more-->

### <span id="Download_Eclipse">Download Eclipse</span>

<p class="western" style="margin-bottom: 0cm;">
  Go to <a title="Eclipse downloads" href=" http://www.eclipse.org/downloads/index-developer.php" target="_blank">eclipse downloads</a> or google  &#8220;<em>download eclipse</em>&#8220;, select the version of your preference and save it to your local disk. In my case I chose the standard Keppler version for Windows 64 bit.
</p>

### <span id="Launching_Eclipse">Launching Eclipse</span>

Unzip the downloaded zip file in a folder of your preference. Before your start eclipse you will want to specify the JVM to use and tweak a few VM parametersin <a title="Eclipse.ini wiki" href="http://wiki.eclipse.org/Eclipse.ini" target="_blank"><em>eclipse.ini</em></a>  This is how my eclipse.ini looks like:

<pre>-startup
plugins/org.eclipse.equinox.launcher_1.3.0.v20130327-1440.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_64_1.1.200.v20130521-0416
-product
org.eclipse.epp.package.standard.product
--launcher.defaultAction
openFile
--launcher.XXMaxPermSize
256M
-showsplash
org.eclipse.platform
--launcher.XXMaxPermSize
256m
--launcher.defaultAction
openFile
--launcher.appendVmargs
-vm
C:\Java\jdk1.6.0_39\bin\javaw.exe
-vmargs
-Dosgi.requiredJavaVersion=1.6
-XX:MaxPermSize=512m
-Xms128m
-Xmx1024m</pre>

You can then launch eclipse by clicking on _eclipse.exe:_

<a href="{{site.url}}/wp-content/uploads/2013/08/2-launch-eclipse.png" target="_blank"><img class="alignnone size-medium wp-image-235" src="{{site.url}}/wp-content/uploads/2013/08/2-launch-eclipse-300x168.png" alt="2-launch-eclipse" width="300" height="168" srcset="{{site.url}}/wp-content/uploads/2013/08/2-launch-eclipse-300x168.png 300w, {{site.url}}/wp-content/uploads/2013/08/2-launch-eclipse-1024x576.png 1024w, {{site.url}}/wp-content/uploads/2013/08/2-launch-eclipse-624x351.png 624w, {{site.url}}/wp-content/uploads/2013/08/2-launch-eclipse.png 1500w" sizes="(max-width: 300px) 100vw, 300px" /></a>

### <span id="Install_Subversive_on_Eclipse">Install Subversive on Eclipse</span>

For code versioning I currently use Subversion (SVN), and the easiest way to integrate it in Eclipse is by using the <a title="Subversive Eclipse project" href="http://www.eclipse.org/subversive/" target="_blank">Subversive </a>project. The following steps explain how to do that:

  * Run Eclipse and select **Help** > **Install New Software&#8230;** from the main menu.
  * On the dialog that appears, select a pre-configured simultaneous release update site in the **Work with** combo-box. For example, for the Keppler release, select the &#8220;Kepler &#8211; http://download.eclipse.org/releases/kepler&#8221; update site.
  * Wait a few seconds until the content of the selected update site is displayed under the combo-box.
  * Expand the **Collaboration** group and select the Subversive features that you would like to install. Certain Subversive features are required if you want to work with SVN, others are optional and offer some additional functionality. You can skip the optional features, if you wish.

    <a href="{{site.url}}/wp-content/uploads/2013/08/3-install-subversive.png" target="_blank"><img class="alignnone size-medium wp-image-239" src="{{site.url}}/wp-content/uploads/2013/08/3-install-subversive-300x277.png" alt="3-install-subversive" width="300" height="277" srcset="{{site.url}}/wp-content/uploads/2013/08/3-install-subversive-300x277.png 300w, {{site.url}}/wp-content/uploads/2013/08/3-install-subversive-1024x946.png 1024w, {{site.url}}/wp-content/uploads/2013/08/3-install-subversive-624x576.png 624w, {{site.url}}/wp-content/uploads/2013/08/3-install-subversive.png 1083w" sizes="(max-width: 300px) 100vw, 300px" /></a>
  * Accept the license and install the plugin and restart eclipse

### <span id="Import_your_local_repository_in_Eclipse">Import your local repository in Eclipse</span>

Go to **Window > Open Perspective > Other** and select **SVN Repository Exploring :**

<a href="{{site.url}}/wp-content/uploads/2013/08/4-select-svn-perspective.png" target="_blank"><img class="alignnone size-medium wp-image-241" src="{{site.url}}/wp-content/uploads/2013/08/4-select-svn-perspective-240x300.png" alt="4-select-svn-perspective" width="240" height="300" srcset="{{site.url}}/wp-content/uploads/2013/08/4-select-svn-perspective-240x300.png 240w, {{site.url}}/wp-content/uploads/2013/08/4-select-svn-perspective.png 355w" sizes="(max-width: 240px) 100vw, 240px" /></a>

<p class="western">
  When opening the perspective for the first time I got the following dialog and selected the corresponding connectors for my svn and windows version.
</p>

<p class="western">
  Install them, you might be warned to accept unsigned content, but that is OK and restart Eclipse:
</p>

<p class="western">
  <a href="{{site.url}}/wp-content/uploads/2013/08/5-subersive-svn-connectors.png" target="_blank"><img class="alignnone size-medium wp-image-242" src="{{site.url}}/wp-content/uploads/2013/08/5-subersive-svn-connectors-254x300.png" alt="5-subersive-svn-connectors" width="254" height="300" srcset="{{site.url}}/wp-content/uploads/2013/08/5-subersive-svn-connectors-254x300.png 254w, {{site.url}}/wp-content/uploads/2013/08/5-subersive-svn-connectors-624x735.png 624w, {{site.url}}/wp-content/uploads/2013/08/5-subersive-svn-connectors.png 632w" sizes="(max-width: 254px) 100vw, 254px" /></a>
</p>

<p class="western" style="margin-bottom: 0cm;">
  Now go again to the <strong>SVN Repository Exploring</strong> perspective, select <strong>New > Repository Location</strong> and give the URL of your local repository.
</p>

Before I checked out my projects, I wanted to be able to check them out as Maven projects. For that I use the <a title="Maven Integration to Eclipse" href="http://www.eclipse.org/m2e/" target="_blank">Maven Integration for Eclipse (m2e)</a> plugin. Another way to install a popular plugin is selecting in the menu **Help > Eclipse Marketplace > Popular**

<a href="{{site.url}}/wp-content/uploads/2013/08/6-m2e-plugin.png" target="_blank"><img class="alignnone size-medium wp-image-244" src="{{site.url}}/wp-content/uploads/2013/08/6-m2e-plugin-216x300.png" alt="6-m2e-plugin" width="216" height="300" srcset="{{site.url}}/wp-content/uploads/2013/08/6-m2e-plugin-216x300.png 216w, {{site.url}}/wp-content/uploads/2013/08/6-m2e-plugin-624x864.png 624w, {{site.url}}/wp-content/uploads/2013/08/6-m2e-plugin.png 629w" sizes="(max-width: 216px) 100vw, 216px" /></a>

Install the plugin and restart Eclipse. Now you can finally go the added svn perspective and checkout your projects:

<a href="{{site.url}}/wp-content/uploads/2013/08/7-check-out-project.png" target="_blank"><img class="alignnone size-medium wp-image-245" src="{{site.url}}/wp-content/uploads/2013/08/7-check-out-project-300x235.png" alt="7-check-out-project" width="300" height="235" srcset="{{site.url}}/wp-content/uploads/2013/08/7-check-out-project-300x235.png 300w, {{site.url}}/wp-content/uploads/2013/08/7-check-out-project-1024x803.png 1024w, {{site.url}}/wp-content/uploads/2013/08/7-check-out-project-624x489.png 624w, {{site.url}}/wp-content/uploads/2013/08/7-check-out-project.png 1032w" sizes="(max-width: 300px) 100vw, 300px" /></a>

Once that is done, you can convert them to Maven projects by right clicking on the project and selecting **Configure > Convert to Maven project :**

<a href="{{site.url}}/wp-content/uploads/2013/08/8-convert-maven-project.png" target="_blank"><img class="alignnone size-medium wp-image-246" src="{{site.url}}/wp-content/uploads/2013/08/8-convert-maven-project-300x289.png" alt="8-convert-maven-project" width="300" height="289" srcset="{{site.url}}/wp-content/uploads/2013/08/8-convert-maven-project-300x289.png 300w, {{site.url}}/wp-content/uploads/2013/08/8-convert-maven-project-624x603.png 624w, {{site.url}}/wp-content/uploads/2013/08/8-convert-maven-project.png 864w" sizes="(max-width: 300px) 100vw, 300px" /></a>

### <span id="Sysdeo_Eclipse_Tomcat_Launcher_plugin">Sysdeo Eclipse Tomcat Launcher plugin</span>

Apache Tomcat is the used server for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, both in development and production. For the development environment in Eclipse I really like the [Sysdeo Eclipse Tomcat Launcher plugin.](http://www.eclipsetotale.com/tomcatPlugin.html) Download the tomcatPluginV33.zip file, unzip it under Eclipse_Home/dropins and restart Eclipse. You should now have the Tomcat buttons in your menu<a href="{{site.url}}/wp-content/uploads/2013/08/10-tomcat-plugin-menu-buttons.png" target="_blank"><img class="alignnone size-medium wp-image-249" src="{{site.url}}/wp-content/uploads/2013/08/10-tomcat-plugin-menu-buttons-300x114.png" alt="10-tomcat-plugin-menu-buttons" width="300" height="114" srcset="{{site.url}}/wp-content/uploads/2013/08/10-tomcat-plugin-menu-buttons-300x114.png 300w, {{site.url}}/wp-content/uploads/2013/08/10-tomcat-plugin-menu-buttons-624x238.png 624w, {{site.url}}/wp-content/uploads/2013/08/10-tomcat-plugin-menu-buttons.png 797w" sizes="(max-width: 300px) 100vw, 300px" /></a>

Before you can start Tomcat from Eclipse go to **Window > Tomcat** and set tomcat home, and other parameters if you might need:

<a href="{{site.url}}/wp-content/uploads/2013/08/11-tomcat-home-configuration.png" target="_blank"><img class="alignnone size-medium wp-image-250" src="{{site.url}}/wp-content/uploads/2013/08/11-tomcat-home-configuration-300x219.png" alt="11-tomcat-home-configuration" width="300" height="219" srcset="{{site.url}}/wp-content/uploads/2013/08/11-tomcat-home-configuration-300x219.png 300w, {{site.url}}/wp-content/uploads/2013/08/11-tomcat-home-configuration-624x456.png 624w, {{site.url}}/wp-content/uploads/2013/08/11-tomcat-home-configuration.png 919w" sizes="(max-width: 300px) 100vw, 300px" /></a>

Now click on the cat icon to start Tomcat.

### <span id="Eclipse_Java_EE_Developer_Tools">Eclipse Java EE Developer Tools</span>

For JSP and XML editing I use the Eclipse JAVA EE Developer Tools. The installation way is the same over **Help** > **Install New Software&#8230;**

<a href="{{site.url}}/wp-content/uploads/2013/08/12-install-java-ee-developer-tools.png" target="_blank"><img class="alignnone size-medium wp-image-254" src="{{site.url}}/wp-content/uploads/2013/08/12-install-java-ee-developer-tools-300x290.png" alt="12-install-java-ee-developer-tools" width="300" height="290" srcset="{{site.url}}/wp-content/uploads/2013/08/12-install-java-ee-developer-tools-300x290.png 300w, {{site.url}}/wp-content/uploads/2013/08/12-install-java-ee-developer-tools-1024x991.png 1024w, {{site.url}}/wp-content/uploads/2013/08/12-install-java-ee-developer-tools-624x604.png 624w, {{site.url}}/wp-content/uploads/2013/08/12-install-java-ee-developer-tools.png 1084w" sizes="(max-width: 300px) 100vw, 300px" /></a>

That&#8217;s it. You might also want to install database access from Eclipse. Maybe is a matter of habit, but I prefer MySql Workbench for that&#8230;

If you liked this, please show your support by <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">helping us</a> with <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>

We promise to only share high quality podcasts and episodes.

<h3 style="margin: 1.714285714rem 0px; padding: 0px; border: 0px; font-size: 1.142857143rem; vertical-align: baseline; clear: both; line-height: 1.846153846; color: #444444; font-family: 'Open Sans', Helvetica, Arial, sans-serif; font-style: normal; font-variant: normal; letter-spacing: normal; orphans: auto; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffffff;">
  <span id="References">References</span>
</h3>

<ul style="margin: 0px 0px 1.714285714rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline; list-style: disc outside; line-height: 24px; color: #444444; font-family: 'Open Sans', Helvetica, Arial, sans-serif; font-style: normal; font-variant: normal; font-weight: normal; letter-spacing: normal; orphans: auto; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffffff;">
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a title="Eclipse.org" href="http://www.eclipse.org" target="_blank">http://www.eclipse.org</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a href="http://www.eclipse.org/downloads/">http://www.eclipse.org/downloads/</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a href="http://wiki.eclipse.org/Eclipse.ini">http://wiki.eclipse.org/Eclipse.ini</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a href="http://eclipse.org/m2e/download/">http://eclipse.org/m2e/download/</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a href="http://www.eclipse.org/subversive/">http://www.eclipse.org/subversive/</a>
  </li>
  <li style="margin: 0px 0px 0px 2.571428571rem; padding: 0px; border: 0px; font-size: 14px; vertical-align: baseline;">
    <a href="http://www.eclipsetotale.com/tomcatPlugin.html">http://www.eclipsetotale.com/tomcatPlugin.html</a>
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
