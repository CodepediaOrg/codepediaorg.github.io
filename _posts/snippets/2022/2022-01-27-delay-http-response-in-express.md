---
layout: post
title: Delay http response in Express
description: "Delay http response in Express code snippet"
author: ama
permalink: /snippets/60a25b99d8923017316598bd/delay-http-response-in-express
published: true
snippetId: 60a25b99d8923017316598bd
categories: [snippets]
tags: [node.js, expressjs, http, javascript, codever-snippets]
---

**Project**: [`Codever`](https://github.com/codeverland/codever)

Enclose the response in `setTimeout`

```javascript
router.get('/', async (request, response) => {

  const {page, limit} = PaginationQueryParamsHelper.getPageAndLimit(request);
  const bookmarks = await PublicBookmarksService.getLatestPublicBookmarks(page, limit);

  setTimeout((() => {
    return response.send(bookmarks);
  }), 2000)
});
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/API/setTimeout" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/API/setTimeout
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60a25b99d8923017316598bd" %}
