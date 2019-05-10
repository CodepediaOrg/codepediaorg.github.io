---
layout: post
title: Sharing coding bookmarks
description: "This posts presents how www.codingmarks.org came to life."
author: ama
permalink: /ama/sharing-coding-bookmarks
published: true
categories: [codingpedia]
tags: [codingmarks, coding bookmarks]
image:
  feature: 2017-02-11-coding-bookmarks-post-feature-image.png
---

Whenever I look through coding bookmarks list in my browser, I get a little bit of discomfort... Why? Because
for some of the "categories" like angular or java ee, the list doesn't fit into my screen anymore, and I use a 2K resolution... Despite trying to neatly order my bookmarks alphabetically,
categorize them in folders, put tags and description on them, I ended up with one big mess.
You might say, so what, why not use the bookmarks manager's search function? Well, first is that awkward keystroke combination or navigation through the menu you need to remember :) and
 then there is browser dependency, privacy concerns, complexity. I can add to that the "just throw it there" careless behaviour.

Why I don't just rely on the Google/search engine? Well,

* much of the really good links I discovered were referenced from other links, which may have come up initially through a search engine
* I like having them persisted, since I put some energy in finding and using them in the first place
* privacy concerns - maybe I won't bother when Google will be able to read my mind, or does it already?
* but the most important thing could prove to be the feedback of public bookmarks; visitors (hopefully not robots), who will be able to vote up
or down a link (feature I will start working on soon)...

<p class="note_normal">
    So, bottom line - I will have bookmarks for useful dev resources, but it will only be <b>one</b> link in my browser.
</p>

<!--more-->

The first thing I tried, was to create a text file with bookmarks in the form of a [README](https://github.com/Codingpedia/dev-resources) on Github, approach taken by others[^1] too.
I must say it looks nice, you can **CTRL+F**-it in the browser, but it's such a hassle to maintain without a GUI[^2].

[^1]: <https://github.com/vhf/free-programming-books>
[^2]: <http://www.linfo.org/gui.html>

Then, why not make a GUI? It was just the perfect project to get into the MEAN[^3] stack world, something I have been interested in for a while now.

[^3]: <https://en.wikipedia.org/wiki/MEAN_(software_bundle)>

## Chossing the website name

It was also pretty clear I needed a website, to have my bookmarks accessible from everywhere. One of the hardest things for a website is picking the right name... A couple of ideas came up - **dev-resources.org**, **coding-bookmarks.org**, **dev-bookmarks.org**,
 but none of those felt quite right. Then I remembered the right thing to do, which is to place a request on the subconscious for it. The next morning, bam - [https://www.codingmarks.org](www.codingmarks.org)
 It shares both the philosophy of [our mission](http://www.codingpedia.org/about/) and gave me a good title - **Sharing Coding Bookmarks**.

## Metadata

When I add a new bookmark, I am requested to provide the following metadata:

* **title/name** - usually title of the link
* **location** - usually a URL of website
* **tags** - keywords; I try to stick to a maximum of five per bookmark
* **notes/description** - relevant message
* **data** - TBD; quite relevant with coding links

### The tags vs categories dilemma

That got easily solved: I use only tags. No more questions like: should I make this category, but feels more like a tag?

## The search

When the number of bookmarks grows, finding the right bookmark should be as easy and accurate as possible.

There are two search options provided:

* search public bookmarks
* search personal/private bookmarks

The search will look through titles, URLs, tags and notes/description of bookmarks.

> You can search for multiple terms by adding the **"+"** sign between them.

## Public bookmarks

On the [landing page](https://www.codingmarks.org) of the website, you can see the public bookmarks. These are private bookmarks, that users chose to share so that others can benefit. I try to make all my personal bookmarks public,
because it forces me to really filter the ones worth adding and to dwell a bit on the right metadata.

I guess this is pretty at the moment. Feel like sharing or improving it?

{% include source-code-codingmarks.html %}

## References
