---
layout: post
title: How I migrated Codingpedia from Wordpress to Jekyll
description: "In this post, I will briefly present the steps I took to migrate Codingpedia.org from Wordpress to Jekyll"
author: ama
permalink: /ama/how-i-migrated-codingpedia-from-wordpress-to-jekyll.html
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

## Leaving Wordpress

To export Wordpress to Jekyll I installed the Jekyll Exporter plugin[^3], which with one click will convert all posts, pages, taxonomies, metadata, and settings to Markdown and YAML which can be dropped into Jekyll. Well for me was a real bummer at
the beginning because it did not work.  

[^3]: <https://wordpress.org/plugins/jekyll-exporter/>

It does export the permalinks, categories and tags. 

## Hello Jekyll

### Finding a theme

### Multiple authors
It was important because of our Coding Friend Program.

### Comments

### Feed

### Sitemap

### Benefits

- static website > speed > how could Github Pages host so many
- open source
- feels more suited for a Programming blog, of course Wordpress might have more Plugins, but there the more you add use them
the less performant your website becomes
- markdown > how to embed code in codingpedia
