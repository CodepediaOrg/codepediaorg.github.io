---
id: 841
title: 'SEO: Friendly URL construction with Spring MVC'
date: 2013-11-06T07:29:42+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=841
permalink: /ama/seo-friendly-url-construction-with-spring-mvc/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 1
dsq_thread_id:
  - 1942061724
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
  - seo
  - spring
tags:
  - construction
  - friendly
  - seo
  - spring
  - spring mvc
  - url
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#What_is_a_friendly_URL">What is a friendly URL?</a>
    </li>
    <li>
      <a href="#Friendly_URL_construction_with_Spring_MVC">Friendly URL construction with Spring MVC</a><ul>
        <li>
          <a href="#Model">Model</a>
        </li>
        <li>
          <a href="#View">View</a>
        </li>
        <li>
          <a href="#Controller">Controller</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Resources">Resources</a>
    </li>
  </ul>
</div>

## <span id="What_is_a_friendly_URL">What is a friendly URL?</span>

<p style="text-align: justify;">
  Since I started to really use the internet, about 13 years ago, I liked to bookmark useful and interesting links for later access. Of course my bookmarks list has been growing steadily ever since and it&#8217;s become difficult to just LOOK for some bookmark on a topic I remember I might have added to the list. So I do it the <em>Google</em>-way, I search for it by typing some relevant words. Besides tags and good titles, URLs that contain some of the words I look for, land at the top of the search results. The same is valid for search engines, who favour friendly or well constructed URLs.
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>
</p>

<!--more-->



But what is a **&#8220;friendly&#8221;** URL? I would say it&#8217;s

<li style="text-align: justify;">
  <strong>constructed logically</strong> so that is most intelligible to humans (when possible, readable words rather than long ID numbers). For example I find that a URL link like <a title="NPR: TED Radio Hour Podcast" href="https://github.com/Codingpedia/podcastpedia/podcasts/1183/NPR-TED-Radio-Hour-Podcast" target="_blank">https://github.com/Codingpedia/podcastpedia/<strong>podcasts</strong>/1183/<strong>NPR-TED-Radio-Hour-Podcast</strong></a> is much more appealing than something like this <a title="NPR: TED Radio Hour Podcast" href="https://github.com/Codingpedia/podcastpedia/podcasts/1183/NPR-TED-Radio-Hour-Podcast" target="_blank">www.podcastpedia.org/id-1183.html</a>. For one you can figure out just from the URL it&#8217;s about a podcast, and briefly what the podcast might be about.
</li>
<li style="text-align: justify;">
  <strong>not too complex</strong> and <strong>not cryptic</strong> &#8211; visitors may be intimidated by extremely long and cryptic URLs that contain few recognizable words. Some users might link to your page using the URL of that page as the anchor text. If your URL contains relevant words, this provides users and search engines with more information about the page than an ID or oddly named parameter would
</li>
<li style="text-align: justify;">
  <strong>flexible</strong> &#8211; allow for the possibility of a part of the URL being removed. For example, instead of using the breadcrumb links on the page, a user might drop off a part of the URL in the hopes of finding more general content &#8211; if the user tipped <a title="Podcasts ordered by publication date" href="https://github.com/Codingpedia/podcastpedia/podcasts" target="_blank">https://github.com/Codingpedia/podcastpedia/<strong>podcasts</strong></a>, he or she would get redirected to a list of all the podcasts available on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>. Or what if the user might be visiting <a title="Podcastpedia.org - Distillations" href="https://github.com/Codingpedia/podcastpedia/podcasts/742/Distillations" target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/742/Distillations</a>, but then enter just <a title="Podcastpedia.org - Distillations" href="https://github.com/Codingpedia/podcastpedia/podcasts/742/ " target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/742/ </a>into the browser&#8217;s address bar. Should she get a 404 or also the right page?
</li>
<li style="text-align: justify;">
  makint use of <strong>hyphens (-)</strong> &#8211; there&#8217;s a reason why Google recommends it &#8211; it&#8217;s easier to read in comparison with underscore (_) or white spaces which get encoded to %20.
</li>

<p dir="ltr" style="text-align: justify;" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  Lastly, remember that the URL to a document is displayed as part of a search result in Google, below the document&#8217;s title and snippet. Like the title and snippet, words in the URL on the search result appear in bold if they appear in the user&#8217;s query:
</p>

<div id="attachment_848" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/11/google-search-results-bold.png"><img class="size-medium wp-image-848" src="{{site.url}}/wp-content/uploads/2013/11/google-search-results-bold-300x193.png" alt="goole search results page highlighted" width="300" height="193" srcset="{{site.url}}/wp-content/uploads/2013/11/google-search-results-bold-300x193.png 300w, {{site.url}}/wp-content/uploads/2013/11/google-search-results-bold-624x403.png 624w, {{site.url}}/wp-content/uploads/2013/11/google-search-results-bold.png 1007w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    highlight of keywords in URLs
  </p>
</div>

<h2 dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  <span id="Friendly_URL_construction_with_Spring_MVC">Friendly URL construction with Spring MVC</span>
</h2>

<p dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  On <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, the URLs for podcast and episode pages are &#8220;friendly&#8221;-built. These are the core pages of the website and therefore should be highly optimized for search engines and easy to bookmark.
</p>

<p dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  <strong>Podcast URLs samples:</strong>
</p>

  * <a title="A History of the World in 100 Objects" href="https://github.com/Codingpedia/podcastpedia/podcasts/676/A-History-of-the-World-in-100-Objects" target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/676/A-History-of-the-World-in-100-Objects</a>
  * <a title="Quarks & Co - zum Mitnehmen" href="https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen" target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen</a>
  * <a title="NPR: TED Radio Hour Podcast" href="www.podcastpedia.org/podcasts/1183/NPR-TED-Radio-Hour-Podcast" target="_blank">www.podcastpedia.org/podcasts/1183/NPR-TED-Radio-Hour-Podcast</a>

<p dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  <strong>Episode URLs samples:</strong>
</p>

  * <a title="AHOW: 100 Solar-powered lamp and charger 22 Oct 2010 " href="https://github.com/Codingpedia/podcastpedia/podcasts/676/A-History-of-the-World-in-100-Objects/episodes/2/AHOW-100-Solar-powered-lamp-and-charger-22-Oct-2010" target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/676/A-History-of-the-World-in-100-Objects/episodes/2/AHOW-100-Solar-powered-lamp-and-charger-22-Oct-2010</a>
  * <a title="Quarks & Co: 08.10.2013, Auf Teilchenjagd " href="https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen/episodes/229/Quarks-Co-08-10-2013-Auf-Teilchenjagd" target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/1/Quarks-Co-zum-Mitnehmen/episodes/229/Quarks-Co-08-10-2013-Auf-Teilchenjagd</a>
  * <a title="Haves and Have Nots" href="https://github.com/Codingpedia/podcastpedia/podcasts/1183/NPR-TED-Radio-Hour-Podcast/episodes/63/Haves-and-Have-Nots" target="_blank">https://github.com/Codingpedia/podcastpedia/podcasts/1183/NPR-TED-Radio-Hour-Podcast/episodes/63/Haves-and-Have-Nots</a>

In the following sections I will present how an episode URL is constructed, as it is more complex:

<div id="attachment_850" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/11/episode-URL-construction.png"><img class="size-medium wp-image-850" src="{{site.url}}/wp-content/uploads/2013/11/episode-URL-construction-300x226.png" alt="Episode URL construction" width="300" height="226" srcset="{{site.url}}/wp-content/uploads/2013/11/episode-URL-construction-300x226.png 300w, {{site.url}}/wp-content/uploads/2013/11/episode-URL-construction-624x471.png 624w, {{site.url}}/wp-content/uploads/2013/11/episode-URL-construction.png 985w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Episode URL construction
  </p>
</div>

The podcast URL construction resembles the same process.

<h3 dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  <span id="Model">Model</span>
</h3>

As you&#8217;ve seen, the following elements are present in the URL of the episode:

  * podcast id
  * podcast title
  * episode id
  * episode title

Among other things, these are java bean properties of the `Episode` domain class. Object of this class type will get loaded in the `model`, and the via the `controller` be presented in the `view`:

<pre class="lang:default mark:14,17,23,26,29,32 decode:true" title="Code snippet Episode domain class">package org.podcastpedia.domain;

import java.io.Serializable;
import java.util.Date;

public class Episode implements Serializable{

	/**
	 * automatic generated serialVersionUID
	 */
	private static final long serialVersionUID = -1957667986801174870L;

	/** identifies the podcast the episode belongs to */
	public Integer podcastId;

	/** episode id - unique identifier of a podcast's episode */
	public Integer episodeId;

	/** description of the episode */
	public String description;

	/** title of the episode */
	public String title;

	/** title of the podcast the episode belongs to */
	public String podcastTitle;

	/** episode's transformed title with hyphens to be added in the URL for SEO optimization */
	private String titleInUrl;

	/** episode's transformed title with hyphens to be added in the URL for SEO optimization */
	private String podcastTitleInUrl;

	..............................
}</pre>

The two properties `titleInUrl` and `podcastTitleInUrl` hold the &#8220;transformed&#8221; episode&#8217;s and respectively, the podcast&#8217;s title. In the transformation process, the spaces between words are replaced with hyphens (-) and if the length exceeds a certain limit (TITLE\_IN\_URL\_MAX\_LENGTH = 100), it will be shortened:

<pre class="lang:java decode:true" title="Transform title to contain hyphens">//build the title that appears in the URL when accessing a podcast from the main application
String titleInUrl = podcastTitle.trim().replaceAll("[^a-zA-Z0-9\\-\\s\\.]", "");
titleInUrl = titleInUrl.replaceAll("[\\-| |\\.]+", "-");
if(titleInUrl.length() &gt; TITLE_IN_URL_MAX_LENGTH){
	podcast.setTitleInUrl(titleInUrl.substring(0, TITLE_IN_URL_MAX_LENGTH));
} else {
	podcast.setTitleInUrl(titleInUrl);
}</pre>

<h3 dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  <span id="View">View</span>
</h3>

Let&#8217;s consider a use case, for example when <a title="Podcastpedia.org - advanced search" href="https://github.com/Codingpedia/podcastpedia/search/advanced_search" target="_blank">searching for episodes</a>, you get a list of results. This list of `Episode` objects is loaded in the `Model` via MyBatis (see my post <a title="MyBatis and Spring integration" href="http://www.codingpedia.org/ama/spring-mybatis-integration-example/" target="_blank">Integrate MyBatis with Spring</a> for that), and then via the `Controller` are presented in the `View`, which in this case is a `JSP` file. The user has the possibility to see the details of a specific episode by clicking on the episode&#8217;s URL, that is constructed in the following manner:

<pre class="lang:default mark:3,7 decode:true" title="JSP snippet - Episodes search results">&lt;div class="results_list"&gt;
	&lt;c:forEach items="${advancedSearchResult.episodes}" var="episode" varStatus="loop"&gt;
		&lt;c:url var="episodeUrl" value="/podcasts/${episode.podcastId}/${episode.podcastTitleInUrl}/episodes/${episode.episodeId}/${episode.titleInUrl}"/&gt;
	    &lt;div class="bg_color shadowy item_wrapper"&gt;
			....
	    	&lt;div class="metadata_desc"&gt;
				&lt;a href="${episodeUrl}"&gt; &lt;c:out value="${episode.title}"/&gt; &lt;/a&gt;
				&lt;div class="pub_date_media_type"&gt;
					&lt;div class="pub_date"&gt;
						&lt;fmt:formatDate pattern="yyyy-MM-dd" value="${episode.publicationDate}" /&gt;
						&lt;c:choose&gt;
							&lt;c:when test="${episode.isNew == 1}"&gt;
								&lt;span class="ep_is_new"&gt;&lt;spring:message code="new"/&gt;&lt;/span&gt;
							&lt;/c:when&gt;
						&lt;/c:choose&gt;
					&lt;/div&gt;
				&lt;/div&gt;
				&lt;div class="ep_desc"&gt;
					${fn:substring(episode.description,0,600)}
				&lt;/div&gt;
			&lt;/div&gt;
			.......
		&lt;/div&gt;
	&lt;/c:forEach&gt;
&lt;/div&gt;</pre>

<h3 dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  <span id="Controller">Controller</span>
</h3>

<p dir="ltr" data-font-name="g_font_505_0" data-canvas-width="322.6415370788574">
  This is perhaps the most interesting part in the &#8220;friendly&#8221; URL construction. Let&#8217;s have a look at the <code>EpisodeController</code>, which handles episode &#8220;friendly&#8221; URLs:
</p>

<pre class="lang:java mark:16,32,33,34 decode:true" title="EpisodeController code snippet">package org.podcastpedia.controllers;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
.................

/**
 * Annotation-driven controller that handles requests to display episodes
 * and episode archive pages.
 *
 * @author ama
 */
@Controller
@RequestMapping("/podcasts")
public class EpisodeController {
	@Autowired
	private EpisodeService episodeService;
    ............
	/**
	* Controller method for episode page.
	*
	* @param podcastId
	* @param episodeId
	* @param show_other_episodes
	* @param model
	* @param httpRequest
	* @return
	* @throws BusinessException
	*/
	@RequestMapping(value="{podcastId}/*/episodes/{episodeId}/*", method=RequestMethod.GET)
	public String getEpisodeDetails(@PathVariable("podcastId") int podcastId,
									@PathVariable("episodeId") int episodeId,
									@RequestParam(value="show_other_episodes", required=false) Boolean show_other_episodes,
									ModelMap model,
									HttpServletRequest httpRequest) throws BusinessException {

		LOG.debug("------ getEpisodeDetails : Received request to show details for episode id " + episodeId
				+ " of the podcast id " + podcastId  + " ------");

		EpisodeWrapper episodeDetails = episodeService.getEpisodeDetails(podcastId, episodeId);
        .............
		SitePreference currentSitePreference = SitePreferenceUtils.getCurrentSitePreference(httpRequest);
		if(currentSitePreference.isMobile() || currentSitePreference.isTablet()){
			return "m_episodeDetails_def";
		} else {
			return "episodeDetails_def";
		}
	}
    ..............
}</pre>

<p style="text-align: justify;">
  The <code>@RequestMapping</code> annotation is used for mapping web requests onto specific handler classes and/or handler methods. A <code>@RequestMapping</code> on the class level is not required. Without it, all paths are simply absolute, and not relative. The <code>@PathVariable</code> indicates that the annotated method argument is bound to the URI template variable specified by the value in parenthesis.
</p>

So if you add the values of the `@RequestMapping` at the class level (line 16) &#8211; `/podcasts` &#8211; and the `@RequestMapping` annotation at the method level (line=32) &#8211; `{podcastId}/*/episodes/{episodeId}/*` &#8211; you get `/podcasts/{podcastId}/*/episodes/{episodeId}/*`, which reproduce exactly the &#8220;friendly&#8221; episode URLs as mentioned before. The star (*) in the URL structure means it can be filled with any text. I chose to fill that with the titles of the podcast and of the episode respectively.

<p style="text-align: justify; padding-left: 30px;">
  <em><strong>Note:</strong> The SitePreference part (lines 44-49) routes the visitor to the desktop or mobile version, depending on her device or selected preferences. See my post <a title="Going mobile with Spring mobile and responsive web design" href="http://www.codingpedia.org/ama/going-mobile-with-spring-mobile-and-responsive-web-design/" target="_blank">Going mobile with Spring mobile and responsive web design</a> for more details.</em>
</p>

<p style="text-align: justify;">
  Well, you see it&#8217;s pretty easy to build friendly URLs with Spring MVC 3.x. This is just one measure to improve your website visibility for search engines. But stay tuned, more posts on this topic will follow.
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>
</p>

## <span id="Resources">Resources</span>

  1. <a title="URL structure" href="https://support.google.com/webmasters/answer/76329" target="_blank">Google Webmaster Tools &#8211; URL Structure</a>
  2. [Search-engine-optimization-starter-guide](https://static.googleusercontent.com/external_content/untrusted_dlcp/www.google.com/en//webmasters/docs/search-engine-optimization-starter-guide.pdf "https://static.googleusercontent.com/external_content/untrusted_dlcp/www.google.com/en//webmasters/docs/search-engine-optimization-starter-guide.pdf")

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

Adrian&#8217;s favorite Spring and Java books

<div class="amazon_book">
</div>

<div class="amazon_book">
</div>

<div class="amazon_book">
</div>

<div class="amazon_book">
</div>

<div class="clear">
</div>
