---
layout: post
title: Mongoose query pagination example
description: "Code snippet example showing how to use a query pagination in MongoDb and Mongoose"
author: ama
permalink: /snippets/6003251fe6eb82224c98e96b/mongoose-query-pagination-example
published: true
snippetId: 6003251fe6eb82224c98e96b
categories: [snippets]
tags: [mongoose, mongodb, pagination, async-await, javascript]
---

[**Project**: `bookmarks.dev`](https://github.com/codeverland/codever) - **File**:  `personal-bookmarks-search.service.js`

Use the mongo cursor `skip` method with `offset` in combination with the [`limit`](https://docs.mongodb.com/manual/reference/method/cursor.limit/) method:

```javascript
let getPersonalBookmarksForSearchFilter = async function (filter, pageNumber, nPerPage) {

  let bookmarks = await Bookmark.find(
    filter,
    {
      score: {$meta: "textScore"}
    }
  )
    .sort({score: {$meta: "textScore"}})
    .skip(pageNumber > 0 ? ((pageNumber - 1) * nPerPage) : 0)
    .limit(nPerPage)
    .lean()
    .exec();

  return bookmarks;
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.mongodb.com/manual/reference/method/cursor.skip/" target="_blank" style="font-weight: lighter">
     https://docs.mongodb.com/manual/reference/method/cursor.skip/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6003251fe6eb82224c98e96b" %}
