---
id: 585
title: Reading/Parsing RSS and Atom feeds in Java with Rome
date: 2013-09-21T14:09:59+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=585
permalink: /ama/reading-parsing-rss-and-atom-feeds-with-rome/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 7
dsq_thread_id:
  - 1783605737
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
tags:
  - atom
  - feed
  - java
  - rome
  - rss
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Maven_dependencies">Maven dependencies</a>
    </li>
    <li>
      <a href="#Building_a_SyndFeed_object">Building a SyndFeed object</a><ul>
        <li>
          <a href="#XmlReader">XmlReader</a>
        </li>
        <li>
          <a href="#InputSource">InputSource</a>
        </li>
        <li>
          <a href="#FileInputStream">FileInputStream</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Using_the_SyndFeed_interface">Using the SyndFeed interface</a>
    </li>
    <li>
      <a href="#Resources">Resources</a>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  As you might have already guessed, <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> is all about podcasts and podcasting is all about distributing audio or video content via <a title="Rss standard" href="http://en.wikipedia.org/wiki/Rss" target="_blank">RSS</a> or <a title="Atom, standard" href="http://en.wikipedia.org/wiki/Atom_%28standard%29" target="_blank">Atom</a>. This post will presents how  Atom and RSS podcast feeds are parsed and added to the directory, with the help of the Java project <a title="Rome project" href="https://rometools.jira.com/wiki/display/ROME/Home" target="_blank">Rome</a>.
</p>

## <span id="Maven_dependencies">Maven dependencies</span>

In order to use Rome in the Java project, you have to add `rome.jar` and `jdom.jar` to your classpath, or if you use Meven the following dependencies in the ``<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Maven_dependencies">Maven dependencies</a>
    </li>
    <li>
      <a href="#Building_a_SyndFeed_object">Building a SyndFeed object</a><ul>
        <li>
          <a href="#XmlReader">XmlReader</a>
        </li>
        <li>
          <a href="#InputSource">InputSource</a>
        </li>
        <li>
          <a href="#FileInputStream">FileInputStream</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Using_the_SyndFeed_interface">Using the SyndFeed interface</a>
    </li>
    <li>
      <a href="#Resources">Resources</a>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  As you might have already guessed, <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> is all about podcasts and podcasting is all about distributing audio or video content via <a title="Rss standard" href="http://en.wikipedia.org/wiki/Rss" target="_blank">RSS</a> or <a title="Atom, standard" href="http://en.wikipedia.org/wiki/Atom_%28standard%29" target="_blank">Atom</a>. This post will presents how  Atom and RSS podcast feeds are parsed and added to the directory, with the help of the Java project <a title="Rome project" href="https://rometools.jira.com/wiki/display/ROME/Home" target="_blank">Rome</a>.
</p>

## <span id="Maven_dependencies">Maven dependencies</span>

In order to use Rome in the Java project, you have to add `rome.jar` and `jdom.jar` to your classpath, or if you use Meven the following dependencies in the`` file:

<pre class="lang:default decode:true" title="Rome maven dependencies">&lt;dependency&gt;
    &lt;groupId&gt;rome&lt;/groupId&gt;
    &lt;artifactId&gt;rome&lt;/artifactId&gt;
    &lt;version&gt;1.0&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.jdom&lt;/groupId&gt;
    &lt;artifactId&gt;jdom&lt;/artifactId&gt;
    &lt;version&gt;1.1&lt;/version&gt;
&lt;/dependency&gt;</pre>

<!--more-->

## <span id="Building_a_SyndFeed_object">Building a SyndFeed object</span>

<p style="text-align: justify;">
  ROME represents syndication feeds (RSS and Atom) as instances of the <code>com.sun.syndication.synd.SyndFeed</code> interface. The <code>SyndFeed</code> interfaces and its properties follow the Java Bean patterns. The default implementations provided with ROME are all lightweight classes.
</p>

### <span id="XmlReader">XmlReader</span>

<p style="text-align: justify;">
  ROME includes parsers to process syndication feeds into <code>SyndFeed</code> instances. The <code>SyndFeedInput</code> class handles the parsers using the correct one, based on the syndication feed being processed. The developer does not need to worry about selecting the right parser for a syndication feed, the <code>SyndFeedInput</code> will take care of it by peeking at the syndication feed structure. All it takes to read a syndication feed using ROME are the following 2 lines of code:
</p>

<pre class="lang:java decode:true" title="Build SyndFeed with XmlReader">SyndFeedInput input = new SyndFeedInput();
SyndFeed feed = input.build(new XmlReader(feedUrl));</pre>

<p style="text-align: justify;">
  The first line creates a <code>SyndFeedInput</code> instance that will work with any syndication feed type (RSS and Atom versions). The second line instructs the <code>SyndFeedInput</code> to read the syndication feed from the char based input stream of a URL pointing to the feed. The <code>&lt;a title="Implementation of XmlReader" href="https://java.net/projects/rome/sources/svn/content/trunk/src/java/com/sun/syndication/io/XmlReader.java" target="_blank">XmlReader&lt;/a></code> is a character based Reader that resolves the encoding following the HTTP MIME types and XML rules for it. The <code>SyndFeedInput.build()</code> method returns a <code>SyndFeed</code> instance that can be easily processed.
</p>

### <span id="InputSource">InputSource</span>

<p style="text-align: justify;">
  Using the approach just mentioned works fine for most of the podcast feeds out there, but for some, <strong>mean</strong> exceptions like <code>"Content is not allowed in prolog"</code> or <code>"Invalid byte 2 of 3-byte UTF-8 sequence"</code> started to occur. To tackle these exceptions I replaced the <code>XmlReader</code> with <code>InputSource</code>, which solved most of the problems &#8211; thank you <a title=" Paŭlo Ebermann on Stackoverflow" href="http://stackoverflow.com/users/600500/paulo-ebermann" target="_blank">Paŭlo Ebermann</a> on StackOverflow for researching into this. The following code snippet presents how this is used to parse the feeds:
</p>

<pre class="lang:java mark:8-15 decode:true" title="Build SyndFeed from URL">public SyndFeed getSyndFeedForUrl(String url) throws MalformedURLException, IOException, IllegalArgumentException, FeedException {

	SyndFeed feed = null;
	InputStream is = null;

	try {

		URLConnection openConnection = new URL(url).openConnection();
		is = new URL(url).openConnection().getInputStream();
		if("gzip".equals(openConnection.getContentEncoding())){
			is = new GZIPInputStream(is);
		}
		InputSource source = new InputSource(is);
		SyndFeedInput input = new SyndFeedInput();
		feed = input.build(source);

	} catch (Exception e){
		LOG.error("Exception occured when building the feed object out of the url", e);
	} finally {
		if( is != null)	is.close();
	}

	return feed;
}</pre>

<p style="text-align: justify;">
  Note the line <code>if("gzip".equals(openConnection.getContentEncoding())</code> &#8211; this was needed because some web sites use <a title="Wikipedia - Gzip" href="http://en.wikipedia.org/wiki/Gzip" target="_blank">gzip</a> to compress the files, and althogh in the browsers you might not recognize this (they decompress the files automatically), if you have to decompress it programatically in your code.
</p>

### <span id="FileInputStream">FileInputStream</span>

<p style="text-align: justify;">
  If for some reason (<code>"Content is not allowed in prolog"</code> or <code>"Invalid byte 2 of 3-byte UTF-8 sequence" etc.</code>),  you cannot parse the feed from the online like via its URL, you can store to a local file, process it and modify the encoding for your needs (very easy with <a title="Notepad++" href="http://notepad-plus-plus.org/" target="_blank">Notepad++  </a>for example ) and parse it from there :
</p>

<pre class="lang:java decode:true" title="Build SyndFeed from local file">public SyndFeed getSyndFeedFromLocalFile(String filePath)
		throws MalformedURLException, IOException,
		IllegalArgumentException, FeedException {

	SyndFeed feed = null;
	FileInputStream fis = null;
	try {
		fis = new FileInputStream(filePath);
		InputSource source = new InputSource(fis);
		SyndFeedInput input = new SyndFeedInput();
		feed = input.build(source);
	} finally {
		fis.close();
	}

	return feed;
}</pre>

## <span id="Using_the_SyndFeed_interface">Using the SyndFeed interface</span>

<p style="text-align: justify;">
  Once the <code>SyndFeed</code> instance is created, it is used to extract the metadata of the podcast(like title, description, author, copyright etc.):
</p>

<pre class="lang:java decode:true" title="Build podcast metadata from SyndFeed">@SuppressWarnings("unchecked")
public void setPodcastFeedAttributes(Podcast podcast,  boolean feedPropertyHasBeenSet) throws Exception {

	SyndFeed syndFeed = podcast.getPodcastFeed()

	if(syndFeed!=null){
		//set DESCRIPTION for podcast - used in search
		if(syndFeed.getDescription()!=null
				&& !syndFeed.getDescription().equals("")){
			String description = syndFeed.getDescription();
			//out of description remove tags if any exist and store also short description
			String descWithoutTabs = description.replaceAll("\\&lt;[^&gt;]*&gt;", "");
			if(descWithoutTabs.length() &gt; MAX_LENGTH_DESCRIPTION) {
				podcast.setDescription(descWithoutTabs.substring(0, MAX_LENGTH_DESCRIPTION));
			} else {
				podcast.setDescription(descWithoutTabs);
			}
		} else {
			podcast.setDescription("NO DESCRIPTION AVAILABLE for FEED");
		}

		//set TITLE - used in search
		String podcastTitle = syndFeed.getTitle();
		podcast.setTitle(podcastTitle);

		//set author
		podcast.setAuthor(syndFeed.getAuthor());

		//set COPYRIGHT
		podcast.setCopyright(syndFeed.getCopyright());

		//set LINK
		podcast.setLink(syndFeed.getLink());

		//set url link of the podcast's image when selecting the podcast in the main application - mostly used through 		SyndImage podcastImage = syndFeed.getImage();
		if(null!= podcastImage){
			if(podcastImage.getUrl() != null){
				podcast.setUrlOfImageToDisplay(podcastImage.getUrl());
			} else if (podcastImage.getLink() != null){
				podcast.setUrlOfImageToDisplay(podcastImage.getLink());
			} else {
				podcast.setUrlOfImageToDisplay(configBean.get("NO_IMAGE_LOCAL_URL"));
			}
		} else {
			podcast.setUrlOfImageToDisplay(configBean.get("NO_IMAGE_LOCAL_URL"));
		}

		podcast.setPublicationDate(null);//default value is null, if cannot be set

		//set url media link of the last episode - this is used when generating the ATOM and RSS feeds from the Start page for example
		for(SyndEntryImpl entry: (List)syndFeed.getEntries()){
			//get the list of enclosures
			List enclosures = (List) entry.getEnclosures();

			if(null != enclosures){
				//if in the enclosure list is a media type (either audio or video), this will set as the link of the episode
				for(SyndEnclosureImpl enclosure : enclosures){
					if(null!= enclosure){
						podcast.setLastEpisodeMediaUrl(enclosure.getUrl());
						break;
					}
				}
			}

			if(entry.getPublishedDate() == null){
				LOG.warn("PodURL[" + podcast.getUrl() + "] - " + "COULD NOT SET publication date for podcast, default date 08.01.1983 will be used " );
			} else {
				podcast.setPublicationDate(entry.getPublishedDate());
			}
			//first episode in the list is last episode - normally (are there any exceptions?? TODO -investigate)
			break;
		}
	}
}</pre>

<p style="text-align: justify;">
  Well, that&#8217;s all folks. Many thanks to the Rome creators and contributers, to the open source communities, to Google, Stackoverflow and to all the great people out there.
</p>

Thanks for sharing and connecting with us

<div class="social_logo_small">
  <a href="https://plus.google.com/+CodingpediaOrg" target="_blank"><img style="float: left;" src="{{site.url}}/wp-content/uploads/2013/10/g_plus.png" alt="Google plus" /></a>
</div>

<div class="social_logo_small">
  <a href="https://twitter.com/codingpedia" target="_blank"><img class="social_logo" src="{{site.url}}/wp-content/uploads/2013/10/twitter.png" alt="Twitter" /></a>
</div>

<div class="social_logo_small">
  <a href="https://www.facebook.com/Codingpedia" target="_blank"><img class="social_logo" src="{{site.url}}/wp-content/uploads/2013/10/fb.png" alt="Facebook" /></a>
</div>

<div class="social_logo_small">
  <a href="mailto:contact@codingpedia.org" target="_blank"><img class="social_logo" src="{{site.url}}/wp-content/uploads/2013/10/email.png" alt="Email" /></a>
</div>

<div class="clear">
</div>

<p class="note_normal">
  Don&#8217;t forget to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you might find it really interesting. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

## <span id="Resources">Resources</span>

  1. <a title="Rome project" href="https://rometools.jira.com/wiki/display/ROME/Home" target="_blank">Rome project</a>
  2. <a title="Read and print feed example" href="https://rometools.jira.com/wiki/display/ROME/Rss+and+atOM+utilitiEs+%28ROME%29+v0.5+Tutorial,+Using+ROME+to+read+a+syndication+feed" target="_blank">Reads and prints any RSS/Atom feed type</a>
  3. <a title="Problem with charset and rome - Stackoverflow" href="http://stackoverflow.com/questions/5365270/problem-with-charset-and-rome-rss-atom-feeds" target="_blank">Problem with charset and rome &#8211; Stackoverflow</a>
  4. <a title="JAVA: Resolving org.xml.sax.SAXParseException: Content is not allowed in prolog" href="http://mark.koli.ch/2009/02/resolving-orgxmlsaxsaxparseexception-content-is-not-allowed-in-prolog.html" target="_blank">JAVA: Resolving org.xml.sax.SAXParseException: Content is not allowed in prolog</a>
  5. <a title="Stackoverflow resource" href="http://stackoverflow.com/questions/20148840/getting-strange-characters-when-trying-to-read-utf-8-document-from-url" target="_blank">Stackoverflow &#8211; Getting strange characters when trying to read UTF-8 document from URL</a>

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


P.S. The stack trace of the mean &#8220;Content is not allowed in prolog&#8221;-error is listed bellow:

<pre class="lang:java decode:true" title="Content not allowed in prolog stack trace">2013-09-19 06:23:43,529 ERROR [org.podcastpedia.admin.service.impl.UpdateServiceImpl:?] -
com.sun.syndication.io.ParsingFeedException: Invalid XML: Error on line 1: Content is not allowed in prolog.
	at com.sun.syndication.io.WireFeedInput.build(WireFeedInput.java:226)
	at com.sun.syndication.io.SyndFeedInput.build(SyndFeedInput.java:136)
	at org.podcastpedia.admin.service.utils.impl.UtilsImpl.getSyndFeedForUrl(UtilsImpl.java:552)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.getSyndFeedForUpdate(UpdateServiceImpl.java:472)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.getNewEpisodes(UpdateServiceImpl.java:389)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.updatePodcastById(UpdateServiceImpl.java:221)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.updatePodcastsFromRange(UpdateServiceImpl.java:607)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:317)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:183)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:150)
	at org.springframework.aop.interceptor.AsyncExecutionInterceptor$1.call(AsyncExecutionInterceptor.java:89)
	at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:303)
	at java.util.concurrent.FutureTask.run(FutureTask.java:138)
	at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:895)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:918)
	at java.lang.Thread.run(Thread.java:662)
Caused by: org.jdom.input.JDOMParseException: Error on line 1: Content is not allowed in prolog.
	at org.jdom.input.SAXBuilder.build(SAXBuilder.java:468)
	at com.sun.syndication.io.WireFeedInput.build(WireFeedInput.java:222)
	... 19 more
Caused by: org.xml.sax.SAXParseException: Content is not allowed in prolog.
	at org.apache.xerces.util.ErrorHandlerWrapper.createSAXParseException(Unknown Source)
	at org.apache.xerces.util.ErrorHandlerWrapper.fatalError(Unknown Source)
	at org.apache.xerces.impl.XMLErrorReporter.reportError(Unknown Source)
	at org.apache.xerces.impl.XMLErrorReporter.reportError(Unknown Source)
	at org.apache.xerces.impl.XMLErrorReporter.reportError(Unknown Source)
	at org.apache.xerces.impl.XMLScanner.reportFatalError(Unknown Source)
	at org.apache.xerces.impl.XMLDocumentScannerImpl$PrologDispatcher.dispatch(Unknown Source)
	at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl.scanDocument(Unknown Source)
	at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
	at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
	at org.apache.xerces.parsers.XMLParser.parse(Unknown Source)
	at org.apache.xerces.parsers.AbstractSAXParser.parse(Unknown Source)
	at org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser.parse(Unknown Source)
	at org.jdom.input.SAXBuilder.build(SAXBuilder.java:453)
	... 20 more</pre>
