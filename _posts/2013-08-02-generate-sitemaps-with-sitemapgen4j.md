---
id: 166
title: Generate sitemaps with sitemapgen4j
date: 2013-08-02T07:29:34+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=166
permalink: /ama/generate-sitemaps-with-sitemapgen4j/
dsq_thread_id:
  - 1559068228
fsb_social_facebook:
  - 3
fsb_show_social:
  - 0
fsb_social_google:
  - 10
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - seo
tags:
  - seo
  - seo optimization
  - sitemap
  - sitemapgen4j
---
This post is about automatically generating sitemaps. I chose this topic, because it is fresh in my mind as I have recently started using sitemaps for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> After some research I came to the conclusion this would be a good thing &#8211; at the time of the posting Google had 3171 URLs indexed for the website (it has been live for 3 months now), whereby after generating sitemaps there were 87,818 URLs submitted. I am curios how many will get indexed after that&#8230;

So because I didn&#8217;t want to introduce over 80k URLs manually, I had to come up with an automated solution for that. Because Podcastpedia.org was developed with Java, it came easy to me to select <a title="SitemapGen4j is an XML sitemap generator written in Java." href="http://code.google.com/p/sitemapgen4j/" target="_blank">sitemapgen4j</a>

## Maven depedency

Check out the latest version <a title="Mvn repository" href="http://mvnrepository.com/artifact/com.google.code/sitemapgen4j" target="_blank">here</a>:

<div>
  <pre>	com.google.code
	sitemapgen4j
	1.0.1

</pre>
</div>

<!--more-->



The podcasts from Podcastpedia.org have an update frequency (DAILY, WEEKLY, MONTHLY, TERMINATED, UNKNOWN) associated, so it made sense to organize sub-sitemaps to make use of the _lastMod_ and _changeFreq_ properties accordingly. This way you can modify the _lastMod_ of the daily sitemap in the sitemap index without modifying the _lastMod_ of the monthly sitemap, and the Google bot doesn&#8217;t need to check the monthly sitemap everyday.

## Generation of sitemap

_Method : createSitemapForPodcastsWithFrequency &#8211; generates one sitemap file_

<pre class="brush: java; title: ; notranslate" title="">/**
	 * Creates sitemap for podcasts/episodes with update frequency
	 *
	 * @param  updateFrequency  update frequency of the podcasts
	 * @param  sitemapsDirectoryPath the location where the sitemap will be generated
	 */
	public void createSitemapForPodcastsWithFrequency(
			UpdateFrequencyType updateFrequency, String sitemapsDirectoryPath)  throws MalformedURLException {

		//number of URLs counted
		int nrOfURLs = 0;

		File targetDirectory = new File(sitemapsDirectoryPath);
		WebSitemapGenerator wsg = WebSitemapGenerator.builder("https://github.com/Codingpedia/podcastpedia", targetDirectory)
									.fileNamePrefix("sitemap_" + updateFrequency.toString()) // name of the generated sitemap
									.gzip(true) //recommended - as it decreases the file's size significantly
									.build();

		//reads reachable podcasts with episodes from Database with
		List podcasts = readDao.getPodcastsAndEpisodeWithUpdateFrequency(updateFrequency);
		for(Podcast podcast : podcasts) {
			String url = "https://github.com/Codingpedia/podcastpedia" + "/podcasts/" + podcast.getPodcastId() + "/" + podcast.getTitleInUrl();
			WebSitemapUrl wsmUrl = new WebSitemapUrl.Options(url)
		     							.lastMod(podcast.getPublicationDate()) // date of the last published episode
		     							.priority(0.9) //high priority just below the start page which has a default priority of 1 by default
		     							.changeFreq(changeFrequencyFromUpdateFrequency(updateFrequency))
		     							.build();
			wsg.addUrl(wsmUrl);
			nrOfURLs++;

			for(Episode episode : podcast.getEpisodes() ){
				url = "https://github.com/Codingpedia/podcastpedia" + "/podcasts/" + podcast.getPodcastId() + "/" + podcast.getTitleInUrl()
						+ "/episodes/" + episode.getEpisodeId() + "/" + episode.getTitleInUrl();

				//build websitemap url
				wsmUrl = new WebSitemapUrl.Options(url)
			     				.lastMod(episode.getPublicationDate()) //publication date of the episode
			     				.priority(0.8) //high priority but smaller than podcast priority
			     				.changeFreq(changeFrequencyFromUpdateFrequency(UpdateFrequencyType.TERMINATED)) //
			     				.build();
				wsg.addUrl(wsmUrl);
				nrOfURLs++;
			}
		}

		// One sitemap can contain a maximum of 50,000 URLs.
		if(nrOfURLs &lt;= 50000){
			wsg.write();
		} else {
			// in this case multiple files will be created and sitemap_index.xml file describing the files which will be ignored
			// workaround to resolve the issue described at http://code.google.com/p/sitemapgen4j/issues/attachmentText?id=8&aid=80003000&name=Admit_Single_Sitemap_in_Index.patch&token=p2CFJZ5OOE5utzZV1UuxnVzFJmE%3A1375266156989
			wsg.write();
			wsg.writeSitemapsWithIndex();
		}

	}
</pre>

The generated file contains URLs to podcasts and episodes, with changeFreq and lastMod set accordingly.

Snippet from the generated <a title="sitemap for monthly updated podcasts" href="https://github.com/Codingpedia/podcastpedia/sitemap_MONTHLY.xml.gz" target="_blank">sitemap_MONTHLY.xml</a>:

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" &gt;
  &lt;url&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/podcasts/581/heise-Developer-SoftwareArchitekTOUR-Podcast&lt;/loc&gt;
    &lt;lastmod&gt;2013-07-05T17:01+02:00&lt;/lastmod&gt;
    &lt;changefreq&gt;monthly&lt;/changefreq&gt;
    &lt;priority&gt;0.9&lt;/priority&gt;
  &lt;/url&gt;
  &lt;url&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/podcasts/581/heise-Developer-SoftwareArchitekTOUR-Podcast/episodes/130/Episode-40-Mobile-Multiplattform-Anwendungen-am-Beispiel-von-jQuery-Mobile&lt;/loc&gt;
    &lt;lastmod&gt;2013-07-05T17:01+02:00&lt;/lastmod&gt;
    &lt;changefreq&gt;never&lt;/changefreq&gt;
    &lt;priority&gt;0.8&lt;/priority&gt;
  &lt;/url&gt;
  &lt;url&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/podcasts/581/heise-Developer-SoftwareArchitekTOUR-Podcast/episodes/90/Episode-39-Entwicklung-fr-Embedded-Systeme-mit-mbeddr&lt;/loc&gt;
    &lt;lastmod&gt;2013-03-11T15:40+01:00&lt;/lastmod&gt;
    &lt;changefreq&gt;never&lt;/changefreq&gt;
    &lt;priority&gt;0.8&lt;/priority&gt;
  &lt;/url&gt;
  .....
&lt;/urlset&gt;
</pre>

## Generation of sitemap index

After sitemaps are generated for all update frequencies, a sitemap index is generated to list all the sitemaps. This file will be submitted in the Google Webmaster Toolos.

_Method : createSitemapIndexFile_

<pre class="brush: java; highlight: [18]; title: ; notranslate" title="">/**
	 * Creates a sitemap index from all the files from the specified directory excluding the test files and sitemap_index.xml files
	 *
	 * @param  sitemapsDirectoryPath the location where the sitemap index will be generated
	 */
	public void createSitemapIndexFile(String sitemapsDirectoryPath) throws MalformedURLException {

		File targetDirectory = new File(sitemapsDirectoryPath);
		// generate sitemap index for foo + bar grgrg
		File outFile = new File(sitemapsDirectoryPath + "/sitemap_index.xml");
		SitemapIndexGenerator sig = new SitemapIndexGenerator("https://github.com/Codingpedia/podcastpedia", outFile);

		//get all the files from the specified directory
		File[] files = targetDirectory.listFiles();
		for(int i=0; i &lt; files.length; i++){
			boolean isNotSitemapIndexFile = !files[i].getName().startsWith("sitemap_index") || !files[i].getName().startsWith("test");
			if(isNotSitemapIndexFile){
				SitemapIndexUrl sitemapIndexUrl = new SitemapIndexUrl("https://github.com/Codingpedia/podcastpedia/" + files[i].getName(), new Date(files[i].lastModified()));
				sig.addUrl(sitemapIndexUrl);
			}

		}
		sig.write();
	}
</pre>

The process is quite simple &#8211; the method looks in the folder where the sitemaps files were created and generates a sitemaps index with these files setting the _lastmod_ value to the time each file had been last modified (line 18).

Et voilà <a title="Sitemap index Podcastpedia.org" href="https://github.com/Codingpedia/podcastpedia/sitemap_index.xml" target="_blank">sitemap_index.xml</a>:

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"&gt;
  &lt;sitemap&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/sitemap_DAILY.xml.gz&lt;/loc&gt;
    &lt;lastmod&gt;2013-08-01T07:24:38.450+02:00&lt;/lastmod&gt;
  &lt;/sitemap&gt;
  &lt;sitemap&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/sitemap_MONTHLY.xml.gz&lt;/loc&gt;
    &lt;lastmod&gt;2013-08-01T07:25:01.347+02:00&lt;/lastmod&gt;
  &lt;/sitemap&gt;
  &lt;sitemap&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/sitemap_TERMINATED.xml.gz&lt;/loc&gt;
    &lt;lastmod&gt;2013-08-01T07:25:10.392+02:00&lt;/lastmod&gt;
  &lt;/sitemap&gt;
  &lt;sitemap&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/sitemap_UNKNOWN.xml.gz&lt;/loc&gt;
    &lt;lastmod&gt;2013-08-01T07:26:33.067+02:00&lt;/lastmod&gt;
  &lt;/sitemap&gt;
  &lt;sitemap&gt;
    &lt;loc&gt;https://github.com/Codingpedia/podcastpedia/sitemap_WEEKLY.xml.gz&lt;/loc&gt;
    &lt;lastmod&gt;2013-08-01T07:24:53.957+02:00&lt;/lastmod&gt;
  &lt;/sitemap&gt;
&lt;/sitemapindex&gt;
</pre>

If you liked this, please show your support by <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">helping us</a> with <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>

We promise to only share high quality podcasts and episodes.

### Source code

  * [SitemapService.zip]({{site.url}}/wp-content/uploads/2013/08/SitemapService.zip) &#8211; the archive contains the interface and class implementation for the methods described in the post

### References

  * <a href="http://code.google.com/p/sitemapgen4j/" target="_blank">http://code.google.com/p/sitemapgen4j/</a>
  * <a href="https://support.google.com/webmasters/answer/156184?hl=en" target="_blank">https://support.google.com/webmasters/answer/156184?hl=en</a>
  * <a href="http://www.sitemaps.org/" target="_blank">http://www.sitemaps.org/</a>

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
