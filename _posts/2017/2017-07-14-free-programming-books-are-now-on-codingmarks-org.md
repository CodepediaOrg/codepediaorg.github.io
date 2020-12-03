---
layout: post
title: Free Programming Books are now on Bookmarks.dev
description: "We're happy to announce that we've reached and surpassed our goal of 1 Mb bookmarks, by importing the books
from the Free-Programming-Books project"
author: ama
permalink: /ama/free-programming-books-are-now-on-bookmarks-dev
published: true
categories: [article]
tags:
    - bookmarks.dev
    - books
    - resources
---

I am really happy to announce that we've reached and surpassed our [goal of 1Mb of #DevBookmarks](https://www.youtube.com/watch?v=bj22xbE5ZiY&feature=youtu.be).
This was possible by importing the free programming books from the [Free-Programming-Books project](https://github.com/EbookFoundation/free-programming-books), which is one
 of the most starred repository on GitHub.

 You can look for all imported free books by typing `[free-programming-books]` (_**tag** filtering_) in the search bar, or by provide it as query parameter:
  [https://www.bookmarks.dev/search?q=Bfree-programming-books%5D](https://www.bookmarks.dev/?q=%5Bfree-programming-books%5D).
  Either way you should get a result similar to the following:

  <figure>
  	<img src="http://www.codepedia.org/images/posts/free-programming-books/search-results.png" alt="">
  	<figcaption>Results when search for free-programming-books tag </figcaption>
  </figure>

<!--more-->

 Of course you might want to filter more, and not scroll through 2700+ results, so you can either  add another search term `nodejs`, for example:

   <figure>
   	<img src="http://www.codepedia.org/images/posts/free-programming-books/search-results-nodejs.png" alt="">
   	<figcaption>Results when search for free-programming-books tag plus nodejs term</figcaption>
   </figure>

 Or use **Filter by language** functionality and select **Spanish** for example:

   <figure>
   	<img src="http://www.codepedia.org/images/posts/free-programming-books/search-results-spanish.png" alt="">
   	<figcaption>Results when search for free-programming-books tag in Spanish</figcaption>
   </figure>

 Or you can combine both - you get the idea... You can see this in action by watching the following videeo:
 <iframe width="560" height="315" src="https://www.youtube.com/embed/CLj1Iv3LQZk" frameborder="0" allowfullscreen></iframe>

 <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" />
     If you wonder how I've done this, weill I have written a <a href="https://github.com/BookmarksDev/bookmarks-free-programming-books-importer" target="_blank">simple Java importer</a>,
     to help me with the import.
 </p>
