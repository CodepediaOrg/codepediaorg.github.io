---
id: 2208
title: 'Quick way to check if the REST API is alive &#8211; GET details from Manifest file'
date: 2015-01-15T20:48:56+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2208
permalink: /ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 7
fsb_social_google:
  - 6
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3423690803
categories:
  - Java EE
  - spring
tags:
  - api
  - manifest
  - maven
  - rest
  - troubleshoot
---
<p style="text-align: justify;">
  There might be cases when you want to quickly verify if your REST API, that is deployed either on dev, test or prod environments, is reachable altogether. A common way to do this is by building a generic resource that delivers for example the version of the deployed API. You can trigger a request to this resource manually or, even better, have a Jenkings/Hudson job, which runs a checkup job after deployment. In this post, I will present how to implement such a service that reads the implementation details from the application&#8217;s manifest file. The API verified, is the one developed in the <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a><!--more-->
</p>

<h2 style="text-align: justify;">
  Software used
</h2>

  1. Jersey JAX-RS implementation 2.14
  2. Spring 4.1.4
  3. Maven 3.1.1
  4. JDK 7

## REST resource

I have developed two REST resources reading from the Manifest file :

  * /manifest &#8211; returns the manifest&#8217;s main attributes as a key, value pairs
  * /manifest/implementation-details &#8211; returns only the implementation details from the manifest file

<pre class="lang:java mark:1 decode:true" title="Manifest REST resource">@Path("/manifest")
public class ManifestResource {

	@Autowired
	ManifestService manifestService;

	@GET
	@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
	public Response getManifestAttributes() throws FileNotFoundException, IOException{
		Attributes manifestAttributes = manifestService.getManifestAttributes();

		return Response.status(Response.Status.OK)
				.entity(manifestAttributes)
				.build();
	}

	@Path("/implementation-details")
	@GET
	@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
	public Response getVersion() throws FileNotFoundException, IOException{
		ImplementationDetails implementationVersion = manifestService.getImplementationVersion();

		return Response.status(Response.Status.OK)
				.entity(implementationVersion)
				.build();
	}

}</pre>

### Request

<pre class="lang:default mark:1 decode:true" title="GET request example - implementation details">GET http://localhost:8888/demo-rest-jersey-spring/manifest HTTP/1.1
Accept-Encoding: gzip,deflate
Accept: application/json
Host: localhost:8888
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)</pre>

### Response &#8211; 200 OK

<pre class="lang:default decode:true" title="Response in JSON format">{
   "Implementation-Title": "DemoRestWS",
   "Implementation-Version": "0.0.1-SNAPSHOT",
   "Implementation-Vendor-Id": "org.codingpedia",
   "Built-By": "ama",
   "Build-Jdk": "1.7.0_40",
   "Manifest-Version": "1.0",
   "Created-By": "Apache Maven 3.1.1",
   "Specification-Title": "DemoRestWS",
   "Specification-Version": "0.0.1-SNAPSHOT"
}</pre>

<p style="text-align: justify;">
  The returned values in case of success (HTTP Status 200 OK) contain different default data related to implementation and specification details. These are automatically generated  the Manifest file with Maven plugin, which I will present in the next section.
</p>

## Generate Manifest file with Maven

Since the demo application is a web application, I am using the Apache maven war plugin supported by the <a title="http://maven.apache.org/shared/maven-archiver/index.html" href="http://maven.apache.org/shared/maven-archiver/index.html" target="_blank">Apache Maven Archiver</a> to generate a Manifest file:

<pre class="lang:default decode:true" title="maven-war-plugin configuration">&lt;plugin&gt;
	&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
	&lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;
	&lt;version&gt;2.5&lt;/version&gt;
	&lt;configuration&gt;
		&lt;warName&gt;${project.artifactId}&lt;/warName&gt;
		&lt;archive&gt;
			&lt;manifest&gt;
				&lt;addDefaultImplementationEntries&gt;true&lt;/addDefaultImplementationEntries&gt;
				&lt;addDefaultSpecificationEntries&gt;true&lt;/addDefaultSpecificationEntries&gt;
			&lt;/manifest&gt;
		&lt;/archive&gt;					
	&lt;/configuration&gt;
	&lt;executions&gt;
		&lt;execution&gt;
			&lt;phase&gt;package&lt;/phase&gt;
			&lt;goals&gt;
				&lt;goal&gt;manifest&lt;/goal&gt;
			&lt;/goals&gt;
			&lt;inherited&gt;true&lt;/inherited&gt;
		&lt;/execution&gt;
	&lt;/executions&gt;				
&lt;/plugin&gt;</pre>

The addDefaultImplementationEntries and addDefaultSpecificationEntries will generate default implementation, respectively specification details, out of the project properties defined in the pom.xml file:

<pre class="lang:default decode:true " title="Default implementation details">Implementation-Title: ${project.name}
Implementation-Version: ${project.version}
Implementation-Vendor-Id: ${project.groupId}
Implementation-Vendor: ${project.organization.name}
Implementation-URL: ${project.url}</pre>

, respectively:

<pre class="lang:default decode:true" title="Default specifiation details">Specification-Title: ${project.name}
Specification-Version: ${project.version}
Specification-Vendor: ${project.organization.name}</pre>

See  <a title=" Apache Maven Archiver" href="http://maven.apache.org/shared/maven-archiver/index.html" target="_blank">Apache Maven Archiver</a> for further details.

<p style="text-align: justify;">
  Notice that in order to generate the Manifest.mf file also in the file system under webapp/META-INF, you need to bind the manifest goal to an execution phase (e.g. package):
</p>

<pre class="lang:default decode:true" title="Bind manifest goal to package phase">&lt;executions&gt;
	&lt;execution&gt;
		&lt;phase&gt;package&lt;/phase&gt;
		&lt;goals&gt;
			&lt;goal&gt;manifest&lt;/goal&gt;
		&lt;/goals&gt;
		&lt;inherited&gt;true&lt;/inherited&gt;
	&lt;/execution&gt;
&lt;/executions&gt;</pre>

## Read from Manifest file

Reading from the manifest file occurs in the injected ManifestService class:

<pre class="lang:java decode:true" title="ManifestService.java">public class ManifestService {

	@Autowired
	ServletContext context;

	Attributes getManifestAttributes() throws FileNotFoundException, IOException{
	    InputStream resourceAsStream = context.getResourceAsStream("/META-INF/MANIFEST.MF");
	    Manifest mf = new Manifest();
	    mf.read(resourceAsStream);
	    Attributes atts = mf.getMainAttributes();

	    return atts;	    		
	}

	ImplementationDetails getImplementationVersion() throws FileNotFoundException, IOException{
	    String appServerHome = context.getRealPath("/");
	    File manifestFile = new File(appServerHome, "META-INF/MANIFEST.MF");

	    Manifest mf = new Manifest();

	    mf.read(new FileInputStream(manifestFile));

	    Attributes atts = mf.getMainAttributes();
	    ImplementationDetails response = new ImplementationDetails();
	    response.setImplementationTitle(atts.getValue("Implementation-Title"));
	    response.setImplementationVersion(atts.getValue("Implementation-Version"));
	    response.setImplementationVendorId(atts.getValue("Implementation-Vendor-Id"));

	    return response;		
	}

}</pre>

To access the MANIFEST.MF file you need to inject the <a title="http://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html" href="http://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html" target="_blank">ServletContext</a>, and call one of its methods

  * <a title="http://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html#getResourceAsStream(java.lang.String)" href="http://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html#getResourceAsStream(java.lang.String)" target="_blank">SerlvetContext#getResourceAsStream()</a> &#8211; (the preferred way)
<li style="text-align: justify;">
  <a title="http://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html#getRealPath(java.lang.String)" href="http://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html#getRealPath(java.lang.String)" target="_blank">ServletContext#getRealPath()</a> &#8211; gets the real path corresponding to the given virtual path. The real path returned will be in a form appropriate to the computer and operating system on which the servlet container is running, including the proper path separators. Its biggest problem in this case, if you don&#8217;t deploy the .war exploded you won&#8217;t have access to the manifest file.
</li>

## Java EE version

In a JavaEE environment, you would have the ServletContext injected via the @Context annotation:

<pre class="lang:java mark:3 decode:true" title="Java EE implementation version">public class ManifestResource {

	@Context
	ServletContext context;

	@GET
	@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
	public Response getManifestAttributes() throws FileNotFoundException, IOException{
	    InputStream resourceAsStream = context.getResourceAsStream("/META-INF/MANIFEST.MF");
	    Manifest mf = new Manifest();
	    mf.read(resourceAsStream);
	    Attributes atts = mf.getMainAttributes();

		return Response.status(Response.Status.OK)
				.entity(atts)
				.build();	    		
	}
	...
}</pre>

Here you have &#8211; a quick way to verify that your REST api is reachable. If you have any suggestions please leave a comment below.

## Resources

  1. Apache Maven
      1. <a title="http://maven.apache.org/shared/maven-archiver/index.html" href="http://maven.apache.org/shared/maven-archiver/index.html" target="_blank">Apache Maven Archiver</a>
      2. <a title="http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings" href="http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings" target="_blank">Introduction to the Build Lifecycle#Built-in_Lifecycle_Bindings</a>
  2. Oracle docs &#8211; <a title="http://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html" href="http://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html" target="_blank">Working with Manifest Files: The Basics</a>
  3.  Stackoverflow
      1. <a title="http://stackoverflow.com/questions/2712970/how-to-get-maven-artifact-version-at-runtime" href="http://stackoverflow.com/questions/2712970/how-to-get-maven-artifact-version-at-runtime" target="_blank">How to get Maven Artifact version at runtime?</a>
      2. <a title="http://stackoverflow.com/questions/14760638/how-to-get-maven-project-version-from-java-method-as-like-at-pom" href="http://stackoverflow.com/questions/14760638/how-to-get-maven-project-version-from-java-method-as-like-at-pom" target="_blank">How to Get Maven Project Version From Java Method as Like at Pom</a>

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

<p style="text-align: center;">
  “Image courtesy of Stuart Miles/ FreeDigitalPhotos.net”
</p>
