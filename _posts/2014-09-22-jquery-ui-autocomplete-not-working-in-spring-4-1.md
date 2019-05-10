---
id: 1851
title: jQuery ui autocomplete not working in Spring 4.1
date: 2014-09-22T11:55:22+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1851
permalink: /ama/jquery-ui-autocomplete-not-working-in-spring-4-1/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:48:"https://github.com/podcastpedia/podcastpedia-web";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 3042247733
fsb_social_facebook:
  - 2
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
  - spring
  - troubleshooting
tags:
  - autocomplete
  - jQuery
  - jquery ui
  - spring
  - spring 4.1
  - troubleshooting
---
<p style="text-align: justify;">
  You may recall from my post <a title="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" href="http://www.codingpedia.org/ama/autocomplete-search-box-with-jquery-and-spring-mvc/" target="_blank">Autocomplete search box with jQuery and Spring MVC</a>, that I use <a title="http://jqueryui.com/autocomplete/" href="http://jqueryui.com/autocomplete/" target="_blank">jQuery ui autocomplete</a> to dynamically search for keywords on <a title="https://github.com/Codingpedia/podcastpedia/tags/all/0" href="https://github.com/Codingpedia/podcastpedia/tags/all/0" target="_blank">Podcastpedia.org</a>. I am now in the process of migrating the source base for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> to Spring 4. I decided to go with the latest version <code>4.1.0.RELEASE</code> and everything worked pretty smoothly until I got to test the auto-complete functionality presented in the post mentioned before.<!--more-->
</p>

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Problem">Problem</a>
    </li>
    <li>
      <a href="#Debugging">Debugging</a>
    </li>
    <li>
      <a href="#Solution">Solution</a>
    </li>
    <li>
      <a href="#Resources">Resources</a><ul>
        <li>
          <a href="#GitHub">GitHub</a>
        </li>
        <li>
          <a href="#Web">Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="Problem">Problem</span>

<p style="text-align: justify;">
  What was actually the problem? Well, as the title says it, it won&#8217;t work&#8230; Nothing happens when I type something in the auto complete input box, and I am not getting getting any error message.
</p>

<h2 style="text-align: justify;">
  <span id="Debugging">Debugging</span>
</h2>

The resource called the jQuery ajax `getJSON`:

<pre class="lang:js mark:9 decode:true" title="Autocomplete jquery ajax call">$(document).ready(function() {
	//attach autocomplete
    $("#tagQuery").autocomplete({    	
    	minLength: 1,
    	delay: 500,

    	//define callback to format results
        source: function (request, response) {
            $.getJSON("/tags/get_tag_list", request, function(result) {                
                response($.map(result, function(item) {                	
                    return {
                        // following property gets displayed in drop down
                        label: item.name + "(" + item.nrOfPodcasts + ")",
                        // following property gets entered in the textbox
                        value: item.name,
                        // following property is added for our own use
                        tag_url: "http://" + window.location.host + "/tags/" + item.tagId + "/" + item.name
                    }

                }));
        	});
    	},

    	//define select handler
    	select : function(event, ui) {
            if (ui.item) {       
            	event.preventDefault();
                $("#selected_tags span").append('&lt;a href=' + ui.item.tag_url + ' class="btn-metadata2" target="_blank"&gt;'+ ui.item.label +'&lt;/a&gt;');
                //$("#tagQuery").value = $("#tagQuery").defaultValue
                var defValue = $("#tagQuery").prop('defaultValue');
                $("#tagQuery").val(defValue);
                $("#tagQuery").blur();
                return false;
            }
    	}

    });

});</pre>

seemed to have been &#8220;rightfully&#8221; mapped by Spring, when looking in the starting log:

<pre class="lang:default mark:2 decode:true" title="Mapping successful for resource">4790 [main] INFO org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping
- Mapped "{[/tags/get_tag_list],methods=[GET],params=[],headers=[],consumes=[],produces=[],custom=[]}"
onto public java.util.List&lt;org.podcastpedia.common.domain.Tag&gt; org.podcastpedia.web.tags.TagController.getTagList(java.lang.String)</pre>

<p style="text-align: justify;">
  By using the Live HTTP Headers add-on from Firefox, I could establish that the GET call was sent from the JavaScript client&#8217;s side, but only to get a <code>HTTP/1.1 406 Not Acceptable</code> response:
</p>

<pre class="lang:default mark:1,3,6,15 decode:true" title="HTTP request successful">http://localhost:8080/tags/get_tag_list?term=jav

GET /tags/get_tag_list?term=jav HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:32.0) Gecko/20100101 Firefox/32.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate
X-Requested-With: XMLHttpRequest
Referer: http://localhost:8080/tags/all/0
Cookie: __atuvc=0%7C22%2C0%7C23%2C19%7C24%2C17%7C25%2C3%7C26; __atsa=sh=facebook%2Ccompact%2Cgoogle_plusone_share%2Ctwitter; __utma=1.487167226.1396700237.1399960777.1400006838.6; __utmz=1.1396700237.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); preferredLanguage=en; JSESSIONID=qp5yyqw9486jq668t9atdjtd; jwplayer.volume=24
Connection: keep-alive
If-None-Match: "0a2e9f5514fc2deefce2de6323e3a54ec"

HTTP/1.1 406 Not Acceptable
Content-Type: text/html; charset=ISO-8859-1
Cache-Control: must-revalidate,no-cache,no-store
Content-Length: 311
Server: Jetty(9.1.5.v20140505)
----------------------------------------------------------</pre>

meaning that the Spring mapping cannot deliver the `application/json` format present in the accept header:

<pre class="lang:default decode:true" title="HTTP request successful">Accept: application/json, text/javascript, */*; q=0.01</pre>

## <span id="Solution">Solution</span>

I remembered then, that when reading through the <a style="color: #ea9629;" title="https://github.com/spring-projects/spring-framework/wiki/Migrating-from-earlier-versions-of-the-spring-framework" href="https://github.com/spring-projects/spring-framework/wiki/Migrating-from-earlier-versions-of-the-spring-framework">Migrating from earlier versions of the Spring Framework</a> &#8211; Guide, the Jackson library was mentioned:

<p style="padding-left: 30px;">
  &#8220;Jackson 1.8.6 <em>(note: minimum 2.1 as of Spring Framework 4.1, with Jackson 2.2.2 or later recommended)</em>&#8220;
</p>

On a second look, I realized that, although I didn&#8217;t use the minimum version 1.8.6:

<pre class="lang:xhtml decode:true" title="Jackson dependencies previous to version 2">&lt;dependency&gt;
	&lt;groupId&gt;org.codehaus.jackson&lt;/groupId&gt;
	&lt;artifactId&gt;jackson-mapper-asl&lt;/artifactId&gt;
	&lt;version&gt;1.9.12&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.codehaus.jackson&lt;/groupId&gt;
	&lt;artifactId&gt;jackson-jaxrs&lt;/artifactId&gt;
	&lt;version&gt;1.9.12&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.codehaus.jackson&lt;/groupId&gt;
	&lt;artifactId&gt;jackson-core-asl&lt;/artifactId&gt;
	&lt;version&gt;1.9.12&lt;/version&gt;			
&lt;/dependency&gt;</pre>

, I had to upgrade the jackson libraries to a newer [2.1, *) version to work in Spring 4.1:

<pre class="lang:xhtml decode:true" title="jackson dependency with new group id">&lt;dependency&gt;
	&lt;groupId&gt;com.fasterxml.jackson.jaxrs&lt;/groupId&gt;
	&lt;artifactId&gt;jackson-jaxrs-base&lt;/artifactId&gt;
	&lt;version&gt;2.4.2&lt;/version&gt;
&lt;/dependency&gt;
</pre>

This dependency will download also the [jackson-core](http://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core "http://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core") and <a title="http://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind" href="http://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind" target="_blank">jackson-databind</a> artifacts for you.

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> the groupId has changed for newer versions of the Jackson libraries.  Learn more in <a title="http://wiki.fasterxml.com/JacksonUpgradeFrom19To20" href="http://wiki.fasterxml.com/JacksonUpgradeFrom19To20" target="_blank">Upgrading Jackson 1.9 to 2.0</a>
</p>

So, the short version of the blog post is _upgrade your jackson libraries to minimum of 2.1 to work with Spring 4.1 🙂_

## <span id="Resources">Resources</span>

### <span id="GitHub">GitHub</span>

  * <a title="https://github.com/podcastpedia/podcastpedia-web" href="https://github.com/podcastpedia/podcastpedia-web" target="_blank">podcastpedia-web (JSPs, java backend…)</a>

### <span id="Web">Web</span>

  * <a title="https://spring.io/blog/2014/01/30/migrating-from-spring-framework-3-2-to-4-0-1" href="https://spring.io/blog/2014/01/30/migrating-from-spring-framework-3-2-to-4-0-1" target="_blank">Migrating from Spring Framework 3.2 to 4.0.1</a>
  * [Migrating from earlier versions of the Spring Framework](https://github.com/spring-projects/spring-framework/wiki/Migrating-from-earlier-versions-of-the-spring-framework "https://github.com/spring-projects/spring-framework/wiki/Migrating-from-earlier-versions-of-the-spring-framework")
  * <a title="http://wiki.fasterxml.com/JacksonRelease20" href="http://wiki.fasterxml.com/JacksonRelease20" target="_blank">JacksonRelease20</a>
  * <a title="http://wiki.fasterxml.com/JacksonUpgradeFrom19To20" href="http://wiki.fasterxml.com/JacksonUpgradeFrom19To20" target="_blank">Upgrading Jackson 1.9 to 2.0</a>

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
