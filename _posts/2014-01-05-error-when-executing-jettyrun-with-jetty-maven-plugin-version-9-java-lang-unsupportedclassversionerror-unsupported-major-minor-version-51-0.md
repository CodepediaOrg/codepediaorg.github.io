---
id: 1061
title: 'Error when executing &#8220;jetty:run&#8221; with jetty-maven-plugin version 9 : java.lang.UnsupportedClassVersionError Unsupported major.minor version 51.0'
date: 2014-01-05T16:16:27+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1061
permalink: /ama/error-when-executing-jettyrun-with-jetty-maven-plugin-version-9-java-lang-unsupportedclassversionerror-unsupported-major-minor-version-51-0/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 2
dsq_thread_id:
  - 2097774742
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - troubleshooting
tags:
  - bug
  - eclipse
  - jdk
  - jetty
  - jetty-maven-plugin
  - maven
  - plugin
---
## Tools involved

  * JDK 1.6/1.7
  * Eclipse Kepler
  * Maven 3.1.1
  * m2e &#8211; Maven Integration for Eclipse plugin
  * Jetty 9
  * Windows 7 / 64 bit

<!--more-->

## Problem

When trying to execute execute the &#8220;jetty:run&#8221; maven goal from within Eclipse

<div id="attachment_1064" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/1-execute-jetty-run-from-eclipse.png"><img class="size-medium wp-image-1064" src="{{site.url}}/wp-content/uploads/2014/01/1-execute-jetty-run-from-eclipse-300x215.png" alt="Execute jetty:run from Eclipse" width="300" height="215" srcset="{{site.url}}/wp-content/uploads/2014/01/1-execute-jetty-run-from-eclipse-300x215.png 300w, {{site.url}}/wp-content/uploads/2014/01/1-execute-jetty-run-from-eclipse-1024x737.png 1024w, {{site.url}}/wp-content/uploads/2014/01/1-execute-jetty-run-from-eclipse.png 1289w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Execute jetty:run from Eclipse
  </p>
</div>

I am getting the following error:

<pre class="lang:java decode:true" title="Error trace jetty:run execution">Error injecting: org.eclipse.jetty.maven.plugin.JettyRunMojo
java.lang.TypeNotPresentException: Type org.eclipse.jetty.maven.plugin.JettyRunMojo not present
	at org.eclipse.sisu.space.URLClassSpace.loadClass(URLClassSpace.java:115)
	at org.eclipse.sisu.space.NamedClass.load(NamedClass.java:46)
	at org.eclipse.sisu.space.AbstractDeferredClass.get(AbstractDeferredClass.java:48)
	at com.google.inject.internal.ProviderInternalFactory.provision(ProviderInternalFactory.java:86)
	at com.google.inject.internal.InternalFactoryToInitializableAdapter.provision(InternalFactoryToInitializableAdapter.java:55)
	at com.google.inject.internal.ProviderInternalFactory$1.call(ProviderInternalFactory.java:70)
	at com.google.inject.internal.ProvisionListenerStackCallback$Provision.provision(ProvisionListenerStackCallback.java:100)
	at org.eclipse.sisu.plexus.PlexusLifecycleManager.onProvision(PlexusLifecycleManager.java:133)
	at com.google.inject.internal.ProvisionListenerStackCallback$Provision.provision(ProvisionListenerStackCallback.java:109)
	at com.google.inject.internal.ProvisionListenerStackCallback.provision(ProvisionListenerStackCallback.java:55)
	at com.google.inject.internal.ProviderInternalFactory.circularGet(ProviderInternalFactory.java:68)
	at com.google.inject.internal.InternalFactoryToInitializableAdapter.get(InternalFactoryToInitializableAdapter.java:47)
	at com.google.inject.internal.InjectorImpl$2$1.call(InjectorImpl.java:997)
	at com.google.inject.internal.InjectorImpl.callInContext(InjectorImpl.java:1047)
	at com.google.inject.internal.InjectorImpl$2.get(InjectorImpl.java:993)
	at com.google.inject.Scopes$1$1.get(Scopes.java:59)
	at org.eclipse.sisu.inject.LazyBeanEntry.getValue(LazyBeanEntry.java:82)
	at org.eclipse.sisu.plexus.LazyPlexusBean.getValue(LazyPlexusBean.java:51)
	at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:260)
	at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:252)
	at org.apache.maven.plugin.internal.DefaultMavenPluginManager.getConfiguredMojo(DefaultMavenPluginManager.java:459)
	at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:97)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:208)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:84)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:59)
	at org.apache.maven.lifecycle.internal.LifecycleStarter.singleThreadedBuild(LifecycleStarter.java:183)
	at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:161)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:317)
	at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:152)
	at org.apache.maven.cli.MavenCli.execute(MavenCli.java:555)
	at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:214)
	at org.apache.maven.cli.MavenCli.main(MavenCli.java:158)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:289)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:229)
	at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:415)
	at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:356)
	at org.codehaus.classworlds.Launcher.main(Launcher.java:46)
Caused by: java.lang.UnsupportedClassVersionError: org/eclipse/jetty/maven/plugin/JettyRunMojo : Unsupported major.minor version 51.0
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClassCond(ClassLoader.java:631)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:615)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:141)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:283)
	at java.net.URLClassLoader.access$000(URLClassLoader.java:58)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:197)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:190)
	at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClassFromSelf(ClassRealm.java:389)
	at org.codehaus.plexus.classworlds.strategy.SelfFirstStrategy.loadClass(SelfFirstStrategy.java:42)
	at org.codehaus.plexus.classworlds.realm.ClassRealm.unsynchronizedLoadClass(ClassRealm.java:259)
	at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass(ClassRealm.java:242)
	at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass(ClassRealm.java:227)
	at org.eclipse.sisu.space.URLClassSpace.loadClass(URLClassSpace.java:107)
	... 42 more</pre>

I have to mention I had just did an upgrade to JDK 1.7 (without removing the previous JDK 1.6). I thought I did everything right, so here are the steps I took after installing the JDK 1.7:

  1. set environemnt variables
      * add `JAVA_HOME` in environment variables and make it point to the new JDK  (e.g. _C:\Java\jdk1.7.0_45_)
      * add `%JAVA_HOME%/bin` to the `PATH` environment variable
  2. made sure Eclipse uses the right JDK:
    <div id="attachment_1065" style="width: 310px" class="wp-caption alignnone">
      <a href="{{site.url}}/wp-content/uploads/2014/01/2-choose-correct-JRE.png"><img class="size-medium wp-image-1065 " src="{{site.url}}/wp-content/uploads/2014/01/2-choose-correct-JRE-300x231.png" alt="Choose the correct JRE" width="300" height="231" srcset="{{site.url}}/wp-content/uploads/2014/01/2-choose-correct-JRE-300x231.png 300w, {{site.url}}/wp-content/uploads/2014/01/2-choose-correct-JRE.png 895w" sizes="(max-width: 300px) 100vw, 300px" /></a>

      <p class="wp-caption-text">
        Eclipse Menu -> Window -> Preferences -> Java &#8211; > Installed JREs
      </p>
    </div></li>

      * pointed to the correct JRE in `eclipse.ini` <pre class="lang:default mark:18,19 decode:true" title="eclipse.ini">-startup
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
C:/Java/jdk1.7.0_45/bin/javaw.exe
-vmargs
-Dosgi.requiredJavaVersion=1.6
-XX:MaxPermSize=512m
-Xms128m
-Xmx1024m</pre>

      * made sure the external Maven installation used &#8220;sees&#8221; the correct JDK by issuing &#8220;mvn &#8211;version&#8221; on the command line: <pre class="lang:default mark:3,4 decode:true" title="" mvn="" --version="" output="">Apache Maven 3.1.1 (0728685237757ffbf44136acec0402957f723d9a; 2013-09-17 17:22:22+0200)
Maven home: C:\Java\apache-maven-3.1.1
Java version: 1.7.0_45, vendor: Oracle Corporation
Java home: C:\Java\jdk1.7.0_45\jre
Default locale: en_US, platform encoding: Cp1252
OS name: "windows 7", version: "6.1", arch: "amd64", family: "windows" &lt;br&gt;</pre></ol>

    ## Solution

    The jetty plugin configuration seemed also to be correct in the `pom.xml` file:

    <pre class="lang:default decode:true" title="jetty-maven-plugin configuration in pom.xml">&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
.................................................
	&lt;build&gt;
		&lt;plugins&gt;
			&lt;plugin&gt;
			  &lt;groupId&gt;org.eclipse.jetty&lt;/groupId&gt;
			  &lt;artifactId&gt;jetty-maven-plugin&lt;/artifactId&gt;
			  &lt;version&gt;${jetty.version}&lt;/version&gt;
			    &lt;configuration&gt;
				     &lt;jettyConfig&gt;${project.basedir}/src/main/resources/config/jetty9.xml&lt;/jettyConfig&gt;
				     &lt;stopKey&gt;STOP&lt;/stopKey&gt;
				     &lt;stopPort&gt;9999&lt;/stopPort&gt;
				     &lt;scanIntervalSeconds&gt;5&lt;/scanIntervalSeconds&gt;
					 &lt;contextXml&gt;${project.basedir}/src/test/resources/jetty-context.xml&lt;/contextXml&gt;
				     &lt;webAppConfig&gt;
				     	&lt;contextPath&gt;/${project.artifactId}-${project.version}&lt;/contextPath&gt;
				     &lt;/webAppConfig&gt;
			    &lt;/configuration&gt;
			    &lt;dependencies&gt;
					&lt;dependency&gt;
						&lt;groupId&gt;mysql&lt;/groupId&gt;
						&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
						&lt;version&gt;5.1.27&lt;/version&gt;
					&lt;/dependency&gt;
			    &lt;/dependencies&gt;
			&lt;/plugin&gt;
		&lt;/plugins&gt;
	&lt;/build&gt;
&lt;/project&gt;</pre>

    Then what was the problem? Well, thanks to <a title="java.lang.UnsupportedClassVersionError Unsupported major.minor version 51.0 [duplicate]" href="http://stackoverflow.com/questions/11239086/java-lang-unsupportedclassversionerror-unsupported-major-minor-version-51-0" target="_blank">Stackoverflow</a> I found out the error happens because of a higher JDK during compile time and lower JDK during runtime. But I thought I compiled and executed everything with Java 7&#8230;

    When checking the JDK compliance for the project:

    <div id="attachment_1071" style="width: 304px" class="wp-caption alignnone">
      <a href="{{site.url}}/wp-content/uploads/2014/01/3-check-project-jdk-compliance.png"><img class="size-medium wp-image-1071" src="{{site.url}}/wp-content/uploads/2014/01/3-check-project-jdk-compliance-294x300.png" alt="Check project JDK compliance" width="294" height="300" srcset="{{site.url}}/wp-content/uploads/2014/01/3-check-project-jdk-compliance-294x300.png 294w, {{site.url}}/wp-content/uploads/2014/01/3-check-project-jdk-compliance.png 741w" sizes="(max-width: 294px) 100vw, 294px" /></a>

      <p class="wp-caption-text">
        Project -> Properties -> Java Compiler
      </p>
    </div>

    the default was on 1.5, which should be no problem for Java 7&#8230;

    After looking again in the console it struck me:

    <div id="attachment_1072" style="width: 310px" class="wp-caption alignnone">
      <a href="{{site.url}}/wp-content/uploads/2014/01/4-console-output.png"><img class="size-medium wp-image-1072" src="{{site.url}}/wp-content/uploads/2014/01/4-console-output-300x177.png" alt="Console output" width="300" height="177" srcset="{{site.url}}/wp-content/uploads/2014/01/4-console-output-300x177.png 300w, {{site.url}}/wp-content/uploads/2014/01/4-console-output-1024x604.png 1024w" sizes="(max-width: 300px) 100vw, 300px" /></a>

      <p class="wp-caption-text">
        Console output
      </p>
    </div>

    &#8230;it was indeed using the older JDK (1.6) at execution time. This was caused by using an Alternate JRE (which was set to `jdk1.6.0_39`), in the setup of the **Run Configuration**:

    <div id="attachment_1073" style="width: 310px" class="wp-caption alignnone">
      <a href="{{site.url}}/wp-content/uploads/2014/01/5-run-configuration-JRE.png"><img class="size-medium wp-image-1073" src="{{site.url}}/wp-content/uploads/2014/01/5-run-configuration-JRE-300x215.png" alt="run configuration JRE" width="300" height="215" srcset="{{site.url}}/wp-content/uploads/2014/01/5-run-configuration-JRE-300x215.png 300w, {{site.url}}/wp-content/uploads/2014/01/5-run-configuration-JRE-1024x737.png 1024w, {{site.url}}/wp-content/uploads/2014/01/5-run-configuration-JRE.png 1289w" sizes="(max-width: 300px) 100vw, 300px" /></a>

      <p class="wp-caption-text">
        Eclipse Menu -> Run -> Run configurations
      </p>
    </div>

    After selecting the &#8220;Workspace default JRE (jdk1.7.0_45)&#8221; everything worked as expected.

    I hope you could learn something from this as I did.

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
