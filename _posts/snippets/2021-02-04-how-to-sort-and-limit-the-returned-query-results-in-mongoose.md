---
layout: post
title: How to sort and limit the returned query results in mongoose
description: "Code snippet showing how to How to sort and limit the returned query results in mongoose"
author: ama
permalink: /snippets/600319dfa4155b6d32a91ee0/how-to-sort-and-limit-the-returned-query-results-in-mongoose
published: true
snippetId: 600319dfa4155b6d32a91ee0
categories: [snippets]
tags: [javascript, mongoose, mongodb, sorting, node.js]
---

[**Project**: `codever`](https://github.com/codeverland/codever) - **File**:  `personal-bookmarks.service.js`

Use the `sort` and `limit` methods. In the following example we receive the latest 30 (`limit`) created bookmarks (sort **descending** by `createdAt` date) :

```javascript
let getLastCreatedBookmarks = async (userId) => {
  const bookmarks = await Bookmark.find({userId: userId})
    .sort({createdAt: -1}) // -1 for descending sort
    .limit(30);

  return bookmarks;
};
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.mongodb.com/manual/reference/method/cursor.sort/" target="_blank" style="font-weight: lighter">
     https://docs.mongodb.com/manual/reference/method/cursor.sort/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="600319dfa4155b6d32a91ee0" %}
