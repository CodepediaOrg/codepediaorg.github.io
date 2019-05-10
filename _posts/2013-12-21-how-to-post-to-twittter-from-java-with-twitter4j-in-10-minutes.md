---
id: 1048
title: How to post to Twittter from Java with Twitter4J in 10 minutes
date: 2013-12-21T17:57:50+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1048
permalink: /ama/how-to-post-to-twittter-from-java-with-twitter4j-in-10-minutes/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 7
dsq_thread_id:
  - 2065212106
fsb_social_google:
  - 6
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
tags:
  - post
  - social media
  - twitter
  - twitter4j
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#The_example">The example</a><ul>
        <li>
          <a href="#Configuration">Configuration</a>
        </li>
        <li>
          <a href="#The_test_method">The test method</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Appendix_Create_a_Twitter_application">(Appendix) Create a Twitter application</a><ul>
        <li>
          <a href="#Create_a_new_application">Create a new application</a>
        </li>
        <li>
          <a href="#Add_writing_permission">Add writing permission</a>
        </li>
        <li>
          <a href="#Generate_Access_Token_and_Access_Token_Secret">Generate Access Token and Access Token Secret</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Resources">Resources</a>
    </li>
  </ul>
</div>



I have just posted my first tweet from Java

<blockquote class="twitter-tweet" lang="en-gb">
  <p>
    &#8221; A Visit to Transylvania&#8221; by Euromaxx: Lifestyle Europe (DW) <a href="http://t.co/MJFDD4540y">http://t.co/MJFDD4540y</a>
  </p>

  <p>
    — Podcastpedia.org (@podcastpedia) <a href="https://twitter.com/podcastpedia/statuses/414285653900533760">December 21, 2013</a>
  </p>
</blockquote>

<p style="text-align: justify;">
  and I still can&#8217;t believe how easy and fast it was. The actual coding took like 2 minutes, whereby the remaining 8 minutes were needed to setup a Twitter application to be able to post in the first place. Not only that, but it worked from the first try, therefore I had to stop coding and start blogging about it. This was made possible by <a title="Twitter4J" href="http://twitter4j.org" target="_blank">Twitter4j </a>(thank you very much <a title="Yusuke Yamamoto github profile" href="https://github.com/yusuke" target="_blank">Yusuke</a>):
</p>

<p class="note_normal" style="text-align: justify;">
  &#8220;Twitter4J is an unofficial Java library for the <a href="https://dev.twitter.com/docs">Twitter API</a>. With Twitter4J, you can easily integrate your Java application with the Twitter service. Twitter4J is an unofficial library&#8221;
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>



<!--more-->

<h2 style="text-align: justify;">
  <span id="The_example">The example</span>
</h2>

The tweet you see was generated from a JUnit test method, so let&#8217;s have a look&#8230;

### <span id="Configuration">Configuration</span>

<p style="text-align: justify;">
  The first thing you need to do is add the <code>twitter4j-core-xxx.jar</code> to the project&#8217;s classpath. Because I use Maven, I added the following dependency to the <code>pom.xml</code> file:
</p>

<pre class="lang:default decode:true" title="Twitter4j Maven dependency">&lt;dependency&gt;
	 &lt;groupId&gt;org.twitter4j&lt;/groupId&gt;
	 &lt;artifactId&gt;twitter4j-core&lt;/artifactId&gt;
	 &lt;version&gt;[3.0,)&lt;/version&gt;
 &lt;/dependency&gt;</pre>

There are a number of properties available for configuring Twitter4J:

  * `oauth.consumerKey`
  * `oauth.consumerSecret`
  * `oauth.accessToken`
  * `oauth.accessTokenSecret`

<p class="note_normal">
  If you don&#8217;t have these yet, please see the second part of the post &#8211; <a href="appendix">Create a Twitter application </a>&#8211; to find out how to get them.
</p>

<p style="text-align: justify;">
  You can specify the properties via <code>twitter4j.properties</code> file, <code>ConfigurationBuilder</code> class or <code>System Property</code>. In the PodcastpediaAdmin project I chose to add a <code>twitter4.properties</code> file in the classpath under <code>src/test/resources</code>:
</p>

[<img class="alignnone size-medium wp-image-1052" src="{{site.url}}/wp-content/uploads/2013/12/twitter4j-properties-186x300.png" alt="twitter4j-properties" width="186" height="300" srcset="{{site.url}}/wp-content/uploads/2013/12/twitter4j-properties-186x300.png 186w, {{site.url}}/wp-content/uploads/2013/12/twitter4j-properties.png 352w" sizes="(max-width: 186px) 100vw, 186px" />]({{site.url}}/wp-content/uploads/2013/12/twitter4j-properties.png)

<pre class="lang:default decode:true" title="twitter4j.properties">debug=true
oauth.consumerKey=*********************
oauth.consumerSecret=******************************************
oauth.accessToken=**************************************************
oauth.accessTokenSecret=******************************************</pre>

### <span id="The_test_method">The test method</span>

<pre class="lang:java mark:17-20 decode:true" title="Test method - post to Twitter">package org.podcastpedia.admin.general;

import org.apache.log4j.Logger;
import org.junit.Test;

import twitter4j.Status;
import twitter4j.Twitter;
import twitter4j.TwitterException;
import twitter4j.TwitterFactory;

public class TestTwitterPosting {

	private static Logger LOG = Logger.getLogger(TestTwitterPosting.class);

	@Test
	public void testPostingToTwitter() throws TwitterException{
		Twitter twitter = TwitterFactory.getSingleton();
		String message="\"A Visit to Transylvania\" by Euromaxx: Lifestyle Europe (DW) \n http://bit.ly/1cHB7MH";
		Status status = twitter.updateStatus(message);
		LOG.debug("Successfully updated status to " + status.getText());
	}

}</pre>

<p style="text-align: justify;">
  The code is fairly simple. I have just created a <code>Twitter</code> instance from <code>TwitterFactory</code>, and called the method <code>updateStatus</code> with the message I wanted posted on Twitter.
</p>

I ran the test method and in the log console I got:

<pre class="lang:default decode:true" title="Log result">2013-12-21 07:46:16,461 DEBUG [org.podcastpedia.admin.general.TestTwitterPosting:20] - &lt;Successfully updated status to " A Visit to Transylvania" by Euromaxx: Lifestyle Europe (DW)
 http://t.co/MJFDD4540y&gt;</pre>

I couldn&#8217;t believe my eyes so I went to check Twitter, and there it was, the tweet shown at the start of the post.

<p style="text-align: justify;">
  The functionality I need for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a> is pretty much reflected in this test method &#8211; the goal is to post tweets for new podcasts directly from the insert-podcasts batch process. So, in production the code will be pretty similar to the one above.
</p>

## <span id="Appendix_Create_a_Twitter_application">(Appendix) Create a Twitter application</span>

### <span id="Create_a_new_application">Create a new application</span>

<p style="text-align: justify;">
  Of course you need to have a <a title="Twitter homepage" href="https://twitter.com/" target="_blank">Twitter</a> account. You have to log in with into the <a title="Twitter Developer" href="https://dev.twitter.com/" target="_blank">Twitter Developer Site </a>and create a new application. Click on &#8220;Create a new application&#8221; -> fill in the form, agree to the Terms and Conditions, answer the CAPTCHA -> and in the end click &#8220;Create your Twitter application&#8221;:
</p>

<div id="attachment_1053" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/12/create-twitter-application.png"><img class="size-medium wp-image-1053" src="{{site.url}}/wp-content/uploads/2013/12/create-twitter-application-300x208.png" alt="Create new Twitter application" width="300" height="208" srcset="{{site.url}}/wp-content/uploads/2013/12/create-twitter-application-300x208.png 300w, {{site.url}}/wp-content/uploads/2013/12/create-twitter-application.png 990w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Create new Twitter application
  </p>
</div>

### <span id="Add_writing_permission">Add writing permission</span>

Once you&#8217;ve done that you will be redirected to your application under <a title="My applications - Twitter" href="https://dev.twitter.com/apps" target="_blank">My applications</a>. In the &#8220;Details&#8221; tab you have the &#8220;OAuth settings&#8221; section:

<div id="attachment_1054" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/12/OAuth-settings.png"><img class="size-medium wp-image-1054" src="{{site.url}}/wp-content/uploads/2013/12/OAuth-settings-300x171.png" alt="OAuth settings" width="300" height="171" srcset="{{site.url}}/wp-content/uploads/2013/12/OAuth-settings-300x171.png 300w, {{site.url}}/wp-content/uploads/2013/12/OAuth-settings.png 970w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Home->My applications->Details:OAuth settings
  </p>
</div>

<p style="text-align: justify;">
  Notice here the value of the <code>Access level</code> attribute, which is set to <code>Read-only</code>. If you want to post on the wall you need to give grant the app writing permission too. So go to &#8220;Settings&#8221; tab, select &#8220;Read and Write&#8221; or &#8220;Read, Write and Access direct messages&#8221; for &#8220;Access&#8221;. When you are ready click on &#8220;Update this Twitter application&#8217;s settings&#8221;:
</p>

<div id="attachment_1055" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/12/write-permission.png"><img class="size-medium wp-image-1055" src="{{site.url}}/wp-content/uploads/2013/12/write-permission-300x234.png" alt="Grant write access " width="300" height="234" srcset="{{site.url}}/wp-content/uploads/2013/12/write-permission-300x234.png 300w, {{site.url}}/wp-content/uploads/2013/12/write-permission.png 836w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Grant write access
  </p>
</div>

The new value will be reflected on the &#8220;Details&#8221; page:

[<img class="alignnone size-medium wp-image-1056" src="{{site.url}}/wp-content/uploads/2013/12/write-permission-reflect-300x165.png" alt="write-permission-reflect" width="300" height="165" srcset="{{site.url}}/wp-content/uploads/2013/12/write-permission-reflect-300x165.png 300w, {{site.url}}/wp-content/uploads/2013/12/write-permission-reflect.png 847w" sizes="(max-width: 300px) 100vw, 300px" />]({{site.url}}/wp-content/uploads/2013/12/write-permission-reflect.png)

<h3 style="text-align: justify;">
  <span id="Generate_Access_Token_and_Access_Token_Secret">Generate Access Token and Access Token Secret</span>
</h3>

<p style="text-align: justify;">
  On the &#8220;Details&#8221; page notice also the <code>Consumer key</code> and <code>Consumer secret</code> values. What we still miss are the <code>Access Token</code> and <code>Access Token Secret</code> values. You can generate them by clicking on the &#8220;Create my access token&#8221; button at the bottom of the &#8220;Details&#8221; page. Once you do that, the generated values will be reflected on the bottom of the &#8220;Details&#8221; page:
</p>

[<img class="alignnone size-medium wp-image-1057" src="{{site.url}}/wp-content/uploads/2013/12/OAuth-tokens-generated-300x193.png" alt="OAuth-tokens-generated" width="300" height="193" srcset="{{site.url}}/wp-content/uploads/2013/12/OAuth-tokens-generated-300x193.png 300w, {{site.url}}/wp-content/uploads/2013/12/OAuth-tokens-generated.png 869w" sizes="(max-width: 300px) 100vw, 300px" />]({{site.url}}/wp-content/uploads/2013/12/OAuth-tokens-generated.png)

You have now all the parameters needed to configure Twitter4J and post on Twitter. I hope you could learn something from this as I did.

<p class="note_normal">
  Don&#8217;t forget to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you&#8217;ll find for sure other interesting podcasts and episodes. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="Resources">Resources</span>

  * <a title="Twitter4J" href="http://twitter4j.org/en/index.html" target="_blank">http://twitter4j.org/en/index.html</a>
  * <a title="Twitter4J - code examples" href="http://twitter4j.org/en/code-examples.html" target="_blank">http://twitter4j.org/en/code-examples.html</a>
  * <a title="Twitter4J - configuration" href="http://twitter4j.org/en/configuration.html" target="_blank">http://twitter4j.org/en/configuration.html</a>
  * <a title="How to post a Tweet in Java using Twitter REST API and Twitter4J Library " href="http://codeoftheday.blogspot.de/2013/07/how-to-post-tweet-in-java-using-twitter.html" target="_blank">http://codeoftheday.blogspot.de/2013/07/how-to-post-tweet-in-java-using-twitter.html</a>
  * <a title="OAuth" href="http://oauth.net/" target="_blank">OAuth</a>

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
