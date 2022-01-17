---
layout: post
title: Mongo create full text index example
description: "Mongo create full text index example code snippet"
author: ama
permalink: /snippets/5f0a9035fedd4b056f5e0774/mongo-create-full-text-index-example
published: true
snippetId: 5f0a9035fedd4b056f5e0774
categories: [snippets]
tags: [mongodb, indexing, full-text-search, sorting, javascript, codever-snippets]
---

When you are searching your code snippets on [Codever](https://www.codever.land), depending on the search terms
you use, relevant results are displayed. Behind this there is a MongoDb full text search index. Let's see how it is
created and used.

Full text search is supported in Mongo by using a text index.
 **Text** indexes can include any field whose value is a string or an array of string elements,
  to which you can give weights. For a text index, the weight of an indexed field denotes the significance of the field
   relative to the other indexed fields in terms of the text search score.

```javascript
db.snippets.createIndex(
  {
    title: "text",
    tags: "text",
    "codeSnippets.comment": "text",
    "codeSnippets.code": "text",
    sourceUrl: "text"
  },
  {
    weights: {
      title: 8,
      tags: 13,
      "codeSnippets.comment": 3,
      "codeSnippets.code": 1,
      sourceUrl: 1
    },
    name: "full_text_search",
    default_language: "none",
    language_override: "none"
  }
);
```

For each indexed field in the document, MongoDB multiplies the number of matches by the weight and sums the results.
 Using this sum, MongoDB then calculates the score for the document.
  You then can use the  [$meta](https://docs.mongodb.com/manual/reference/operator/aggregation/meta/) operator for details
   on returning and sorting by text scores, as in the snippet below:

```javascript
let getPublicBookmarksForSearchedTerms = async function (nonSpecialSearchTerms, page, limit, sort, specialSearchFilters, searchInclude) {

  let filter = {
    public: true
  }

  if ( nonSpecialSearchTerms.length > 0 ) {
    if(searchInclude === 'any') {
      filter.$text = {$search: nonSpecialSearchTerms.join(' ')}
    } else {
      filter.$text = {$search: bookmarksSearchHelper.generateFullSearchText(nonSpecialSearchTerms)};
    }
  }

  addSpecialSearchFiltersToMongoFilter(specialSearchFilters, filter);

  let sortBy = {};
  if ( sort === 'newest' ) {
    sortBy.createdAt = -1;
  } else {
    sortBy.score = {$meta: "textScore"}
  }

  let bookmarks = await Bookmark.find(
    filter,
    {
      score: {$meta: "textScore"}
    }
  )
    .sort(sortBy)
    .skip((page - 1) * limit)
    .limit(limit)
    .lean()
    .exec();

  return bookmarks;
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.mongodb.com/manual/core/index-text/" target="_blank" style="font-weight: lighter">
     https://docs.mongodb.com/manual/core/index-text/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5f0a9035fedd4b056f5e0774" %}
