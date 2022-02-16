---
layout: post
title: Delete documents with mongoose examples
description: "Code snippets showing how to delete mongo documents with mongoose"
author: ama
permalink: /snippets/620c9bb7e102f148ceb9f6f0/delete-documents-with-mongoose-examples
published: true
snippetId: 620c9bb7e102f148ceb9f6f0
categories: [snippets]
tags: [mongodb, mongoose, javascript, codever-snippets]
---

To delete one entry you can use `findOneAndRemove` command - it issues a mongodb `findAndModify` remove command.
Finds a matching document, removes it, passing the found document (if any) to the callback.

```javascript
let deleteBookmarkById = async (userId, bookmarkId) => {
  const bookmark = await Bookmark.findOneAndRemove({
    _id: bookmarkId,
    userId: userId
  });

  if ( !bookmark ) {
    throw new NotFoundError('Bookmark NOT_FOUND with id: ' + bookmarkId);
  } else {
    return true;
  }
};
```

An alternative is to use the `deleteOne()` method which deletes the first document that matches `conditions` from the collection.
It returns an object with the property `deletedCount` indicating how many documents were deleted:

```javascript
let deleteBookmarkById = async (userId, bookmarkId) => {
  const response = await Bookmark.deleteOne({
    _id: bookmarkId,
    userId: userId
  });

  if ( response.deletedCount !== 1 ) {
    throw new NotFoundError('Bookmark NOT_FOUND with id: ' + bookmarkId);
  } else {
    return true;
  }
};
```

To delete multiple documents use the `deleteMany` function. This deletes all the documents that match the conditions specified in filter.
It returns an object with the property `deletedCount` containing the number of documents deleted.

```javascript
/**
 * Delete bookmarks of a user, identified by userId
 */
let deleteBookmarksByUserId = async (userId) => {
  await Bookmark.deleteMany({userId: userId});
  return true;
};
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://mongoosejs.com/docs/api/model.html" target="_blank" style="font-weight: lighter">
     https://mongoosejs.com/docs/api/model.html
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="620c9bb7e102f148ceb9f6f0" %}
