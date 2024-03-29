---
layout: post
title: Access elements filtered by pipe in angular html template
description: "Access elements filtered by pipe in angular html template code snippet"
author: ama
permalink: /snippets/63806e79944cbd1faacdda84/access-elements-filtered-by-pipe-in-angular-html-template
published: true
snippetId: 63806e79944cbd1faacdda84
categories: [snippets]
tags: [angular, html, html-template, filter, angular-pipe, codever-snippets]
---

To access the filtered results of the pipe just define it as a variable via the `as` keyword

```
*ngFor="let bookmark of bookmarks | bookmarkFilter: filterText as filteredBookmarks"
```

In the full example the `bookmarkFilter` pipe filters the list of bookmarks based on some text search.
Further I need to check whether the filtered results contain only one element `[showMoreText] = "filteredBookmarks.length === 1 "`,
to then display the full content of the bookmark:

```html
<div class="mt-3" *ngFor="let bookmark of bookmarks | bookmarkFilter: filterText as filteredBookmarks">
    <app-bookmark-list-element
        [showMoreText] = "filteredBookmarks.length === 1 || bookmarks.length === 1"
        [bookmark]="bookmark"
        [userData$]="userData$"
        [queryText]="queryText"
        [filterText]="filterText"
        [isSearchResultsPage]="isSearchResultsPage"
    >
    </app-bookmark-list-element>
</div>
```

See it in action at [www.codever.dev](https://www.codever.dev):

![Copy-to-clipboard-demo](/images/posts/2022-11-25-get-filtered-results-from-angular-pipe/filter-bookmarks-results-expanding.gif)

<hr>
The source code for [Codever](https://www.codever.dev) is available on [Github](https://github.com/CodeverDotDev/codever)
<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="63806e79944cbd1faacdda84" %}
