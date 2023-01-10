---
layout: post
title: Mongo full text search example
description: "Code snippet show how to do a full text search in Mongo"
author: ama
permalink: /snippets/60046e4554aac471e0799da7/mongo-full-text-search-example
published: true
snippetId: 60046e4554aac471e0799da7
categories: [snippets]
tags: [mongodb, full-text-search]
---

[**Project**: `codever`](https://github.com/CodeverDotDev/codever)  - **File**:  `personal-bookmarks-search.service.js`

To perform a text search query on the `text` index of a collection, you need to use the `$text` operator.
 In the example below the results are sorted in order of relevance score,
  which you must explicitly project to the `$meta` **textScore** field and sort on it:

```javascript
let getPersonalBookmarksForSearchedTerms = async function (nonSpecialSearchTerms, page, limit, userId, specialSearchFilters, searchInclude) {

  let filter = {userId: userId};
  if ( nonSpecialSearchTerms.length > 0 ) {
    if ( searchInclude === 'any' ) {
      filter.$text = {$search: nonSpecialSearchTerms.join(' ')}
    } else {
      filter.$text = {$search: bookmarksSearchHelper.generateFullSearchText(nonSpecialSearchTerms)};
    }
  }
  addSpecialSearchFiltersToMongoFilter(specialSearchFilters, filter);

  let bookmarks = await Bookmark.find(
    filter,
    {
      score: {$meta: "textScore"}
    }
  )
    .sort({score: {$meta: "textScore"}})
    .skip((page - 1) * limit)
    .limit(limit)
    .lean()
    .exec();

  return bookmarks;
}
```

By default, the full text search in Mongo looks for any of the terms, so a query of "java exception" will look for the bookmarks
 that container **java** or **exception**. You can force an **AND** search by using the terms in double quotes,
  so the query text would be `"java" "exception"`. This is what the following `generateFullSearchText` function does for terms
   that you do not want excluded (To exclude a word in mongo, you can prepend a “-” character):

```javascript
let generateFullSearchText = function (nonSpecialSearchTerms) {
  let termsQuery = '';
  nonSpecialSearchTerms.forEach(searchTerm => {
    if ( searchTerm.startsWith('-') ) {
      termsQuery += ' ' + searchTerm;
    } else { //wrap it in quotes to make it a default AND in search
      termsQuery += ' "' + searchTerm.substring(0, searchTerm.length) + '"';
    }
  });

  return termsQuery.trim();
};
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.mongodb.com/manual/text-search/" target="_blank" style="font-weight: lighter">
     https://docs.mongodb.com/manual/text-search/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60046e4554aac471e0799da7" %}
