---
layout: post
title: Concatenate strings and variable values in angular html template
description: "How to concatenate strings and variable values in angular html template code snippet"
author: ama
permalink: /snippets/5fdfae7b9de1f42bb232d0bd/how-to-concatenate-strings-and-variable-values-in-angular-html-template
published: true
snippetId: 5fdfae7b9de1f42bb232d0bd
categories: [snippets]
tags: [angular, html, html-template]
---

Just use the simple quotes `'` and `+` sign inside the double quotes of the html attribute value. See the value
of the `href` attribute below:

```html
<div class="float-right mt-1">
  <a
    *ngIf="snippet.public"
    type="button" class="btn btn-light btn-sm float-right"
    href="mailto:?subject={{'Code snippet: ' + snippet.title}}&body={{'https://www.bookmarks.dev/snippets/' + snippet._id + '/details'}}"
    title="Share link to snippet via Email">
    <i class="far fa-envelope"></i> Email
  </a>
</div>
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://github.com/BookmarksDev/bookmarks.dev" target="_blank" style="font-weight: lighter">
     https://github.com/BookmarksDev/bookmarks.dev
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fdfae7b9de1f42bb232d0bd" %}
