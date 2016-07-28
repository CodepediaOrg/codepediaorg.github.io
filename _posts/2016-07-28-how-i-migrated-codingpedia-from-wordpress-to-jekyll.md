---
layout: post
title: How I migrated Codingpedia from Wordpress to Jekyll
description: "In this post, I will briefly present the steps I took to migrate Codingpedia.org from Wordpress to Jekyll"
author: ama
permalink: /ama/how-i-migrated-codingpedia-from-wordpress-to-jekyll/
categories: [jekyll]
tags: [jekyll, wordpress, migration, codingpedia]
---

In this post, I will briefly present the steps I took to migrate Codingpedia.org from Wordpress to Jekyll.

## Why?

Well, I was experiencing performance pains with the virtual private server from GoDaddy, where I used to
host [Podcastpedia.org](https://www.podcastpedia.org) and [Codingpedia.org](http://www.codingpedia.org). Codingpedia was
developed initially with Wordpress[^1], which I still think is a great tool, if you want to quickly start a blog and don't
have programming experience, but it kinda requires a LAMP[^2] stack, and it's a performance killer from a number of visitors forward. Of course you can always add more hardware to support the website, but that has a limit too. Limited was also the
budget I was ready to spend on hosting, by today's standards.

[^1]: <https://wordpress.org/>
[^2]: <https://en.wikipedia.org/wiki/LAMP_(software_bundle)>

<!--more-->

## Leaving Wordpress

To export Wordpress to Jekyll I installed the Jekyll Exporter plugin[^3], which with one click will convert all posts, pages, taxonomies, metadata, and settings to Markdown and YAML which can be dropped into Jekyll. Well for me was a real bummer at
the beginning because it did not work.  

[^3]: <https://wordpress.org/plugins/jekyll-exporter/>

It does export the permalinks, categories and tags.

## Hello Jekyll

**So what is Jekyll, exactly?**

"Jekyll is a simple, blog-aware, static site generator. It takes a template directory containing raw text files in various formats, runs it through a converter (like Markdown) and our Liquid renderer, and spits out a complete, ready-to-publish static website suitable for serving with your favorite web server. Jekyll also happens to be the engine behind GitHub Pages, which means you can use Jekyll to host your project’s page, blog, or website from GitHub’s servers for free."[^4]

[^4]: <https://jekyllrb.com/docs/home/>

### Hosting

I decided to host the website on GitHub Pages[^4], because is currently free, hosted directly from the
[GitHub Repository](https://github.com/Codingpedia/codingpedia.github.io). Just edit, push, and your changes are live.

> It matches perfectly our idea of [sharing coding knowledge](http://www.codingpedia.org/about/#Mission)

[^5]: <https://github.com/jekyll/jekyll/wiki/themes>

### Finding a theme

The first task was to find a proper Jekyll Theme[^5] - I was looking for one
that was simple, neat, responsive, open-source and had good post samples for code, images and video embedding. After some research I found [Neo-HPSTR Jekyll Theme](https://github.com/aron-bordin/neo-hpstr-jekyll-theme), thank you [Aron](https://github.com/aron-bordin), that fulfils all of the requirements and on top of that has nice social sharing features.

[^6]: <https://github.com/jekyll/jekyll/wiki/themes>

### Code embedding

One concern that I had, was what would happen with all the code from the posts, because you know it is a programming blog...


### Multiple authors
It was important because of our Coding Friend Program.

### Comments

Because I already had a [Disqus](http://disqus.com) account, migrating the comments was straightforward - I just changed `disqus_shortname` in `_config.yml` to the Disqus *codingpedia* . By default comments appear on all post and pages. To disable commenting on a post or page, add the following to its YAML Front Matter:

{% highlight yaml %}
comments: false
{% endhighlight %}

> Implementation is based on [Jekyll Installation Instructions](https://help.disqus.com/customer/portal/articles/472138-jekyll-installation-instructions) from Disqus

### Feed

It uses the theme's own [Atom implementation](https://github.com/Codingpedia/codingpedia.github.io/blob/master/feed.xml), which I adjusted for multiple authors. Don't forget to subscribe to it [here](http://www.codingpedia.org/feed.xml).

> Another possibility would have been to use Atom (RSS) feeds for GitHub Pages[^6], which can automatically create an
 [Atom feed](https://en.wikipedia.org/wiki/Atom_%28standard%29) for the Jekyll blog deployed on GitHub Pages.

[^7]: <https://help.github.com/articles/atom-rss-feeds-for-github-pages/>

### Sitemap

Unlike custom feed generation, I used Sitemaps for GitHub Pages[^7], which can automatically create [sitemaps](https://support.google.com/webmasters/answer/156184?hl=en) for the blog

[^8]: <https://help.github.com/articles/atom-rss-feeds-for-github-pages/>

> If you decide you want to create your own sitemaps, a good starting point is [Building a Better Sitemap.xml with Jekyll](http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/)

### Benefits

So far I am glad I did this move. Here are a summary of the benefits I reaped so far:
* it feels more suited for a programming blog
* the generated static website is light years faster than what I had before
* free hosting; edit, git push and is live via GitHub Pages
* open source which is a perfect match with our mission
* uses markdown[^8] which is very easy to read and write, especially for a programming blog
* provides native SASS support[^10]

[^9]: <https://daringfireball.net/projects/markdown/>
[^10]: <https://jekyllrb.com/docs/assets/>

I hope you've found this useful and after you will have migrated to Jekyll, join our [Coding Friend Program](http://www.codingpedia.org/friends/) so we can easier republish your articles or, even better, do it yourself with a [pull request](https://github.com/Codingpedia/codingpedia.github.io/blob/master/CONTRIBUTING_POST.md).

## References
