---
layout: post
title: Efficient management of dev bookmarks and code snippets saves me two workweeks a year
description: "Finding an already visited link or that piece of code you know it works and used before,
can be a tedious job and sometimes even frustrating.
 It normally takes me at least 30 seconds to look for such a resource the traditional way, and I do that at least 40 times a day.
  With bookmarks.dev it takes about 10 seconds, so I get a time saving of around 40 * 20 / 60 = 13.33 minutes a day.
   Over a year's time this translates to around 80+ hours of saved time."
author: ama
permalink: /ama/efficient-management-of-my-dev-bookmarks-and-code-snippets-saves-me-two-workweeks-a-year
published: true
categories: [productivity]
tags: [bookmarks, efficiency, bookmarksdev]
---

## Motivation
Finding the desired link again or that working piece of code can be sometimes a tedious job. It can even become frustrating
when you don't find it in a reasonable amount of time, because you know it's there somewhere, you've seen it before...
After a struggle of about two or more minutes you finally find it.

 ![Nailed it](/images/posts/2019-02-10-saving-time-with-codingmarks/nailed-it.gif)

> For code snippets it might take even longer depending on your project portfolio's size. **I had sometimes the impression
> I spent more time looking for stuff than actually programming**...

This time you bookmark the resource, and say to yourself - "_from now on I will be
more disciplined and consciously use bookmarks and a code snippet manager_".

So far, so good, but soon the bookmarks toolbar won't fit into your 4k wide monitor anymore, and you might lose more
 time browsing through them... Because you bookmarked it, right?

<figure>
  <img src="{{site.url}}/images/posts/2019-02-10-saving-time-with-codingmarks/or-didnt-i.gif" alt="Or didn't you?"/>
  <figcaption>Or didn't you?</figcaption>
</figure>

<!--more-->

> Gist retrieval can also be optimized IMHO

Eventually you say "_F*ck that, Google knows everything about me anyway, so I'll just google it when I need it_". It usually works,
 all the privacy issues taken into account ;), but what about your company's intranet links or those dozens of intern wiki articles?
  Do you really want to use the intranet search or confluence search? I sure as hell don't.

## Light at the end of your toolbar - [Bookmars.dev](https://www.bookmarks.dev)
Well, out of these frustrations I have created [www.bookmarks.dev](https://www.bookmarks.dev) to help me manage more efficiently
 both my programming and work related bookmarks plus reusable code snippets (**like 80+ hours plus a year more efficiently...**)

### How can you achieve that?

Well my computations are simple. It normally takes 30 seconds or more to look for a link the "traditional" way, and I do that
at least 20 times a day. Code snippets retrieval might take even longer depending on the size of your project portfolio.
On [Bookmarks.dev](https://www.bookmarks.dev/howto) it takes me about 10 seconds, so I get a time saving of around 40 * 20 / 60 = 13.33 minutes a day.
Over a year's time this translates to around __80+ hours of saved time__.

> Sparing frustrations, that is priceless...

I have made it very easy to
* **Save** bookmarks and code snippets via
  * Automatic completion of location, title and description (and tags for youtube videos and stackoverflow questions)
    * [Bookmarklets](https://www.bookmarks.dev/howto/bookmarklets)
    * [Chromium extension](https://chrome.google.com/webstore/detail/save-url-to-bookmarksdev/diofdblfhjbpgackifolmboaiccmebjb) - [source code](https://github.com/BookmarksDev/bookmarks.dev-chrome-extension)
  * [IntelliJ Plugin](https://plugins.jetbrains.com/plugin/14456-save-to-bookmarks-dev) - [source code](https://github.com/BookmarksDev/bookmarks.dev-intellij-plugin)
  * [Visual Studio Extension - please help](https://github.com/BookmarksDev/bookmarks.dev/issues/35)

* **Search & retrieve**
  * Tags
  * Full-text search
  * History, Read Later, Pinned & more

See it in action in the following video:
<iframe width="560" height="315" src="https://www.youtube.com/embed/f9GpTtFVD9Q" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Best, give it a try yourself. When it becomes part of dev tools
productivity toolbox, please share it so other can save time too.

 <span class="highlight-yellow">Beware there is no 30 days money back guarantee, because everything is free and open-source.</span>

  {% include source-code-bookmarks.dev.html %}

