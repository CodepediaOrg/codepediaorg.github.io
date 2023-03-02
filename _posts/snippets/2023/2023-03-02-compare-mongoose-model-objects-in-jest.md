---
layout: post
title: Compare mongoose model objects in jest excluding _id
description: "Compare mongoose model objects in jest code snippet"
author: ama
permalink: /snippets/63ff7adb1b5b8945055f4e59/compare-mongoose-model-objects-in-jest
published: true
snippetId: 63ff7adb1b5b8945055f4e59
categories: [snippets]
tags: [javascript, mongoose, testing, jest, codever-snippets, mongodb]
---

The schema model object in Mongoose provides an `_id` that is of type `ObjectId`. If you are not interested in
comparing value of this attribute when you compare it compare objects in jest, you can exclude it
by calling the `toObject` method of the mongoose model and set the `_id` object to nothing via the spread operator,
something similar to the following:

```
expect({...resultBookmark.toObject(), _id: {}}).toEqual({...expectedBookmark.toObject(), _id: {}})
```

The complete testing method with the setup is shown in the snippet bellow,
where expected bookmark model should match the mapped bookmark from the given request:

```javascript
const showdown = require('showdown');
const Bookmark = require('../../model/bookmark');
const bookmarkRequestMapper = require('./bookmark-request.mapper');

jest.mock('showdown', () => {
  const makeHtml = jest.fn(() => '<p>This is a test bookmark</p>');
  return {
    Converter: jest.fn().mockImplementation(() => ({makeHtml})),
  };
});

describe('toBookmark', () => {
  const req = {
    body: {
      _id: '123',
      name: 'Test Bookmark',
      location: 'https://example.com',
      language: 'en',
      description: 'This is a test bookmark',
      tags: ['test'],
      public: true,
    },
    params: {
      userId: '456',
    },
  };

  beforeEach(() => {
    showdown.Converter.mockClear();
    showdown.Converter().makeHtml.mockClear();
    req.body.descriptionHtml = undefined;
    req.body.youtubeVideoId = undefined;
    req.body.stackoverflowQuestionId = undefined;
  });

  it('should use descriptionHtml if it is provided', () => {
    req.body.descriptionHtml = '<p>This is a test bookmark</p>';

    const expectedBookmark = new Bookmark({
      _id: '123',
      name: 'Test Bookmark',
      location: 'https://example.com',
      language: 'en',
      description: 'This is a test bookmark',
      descriptionHtml: '<p>This is a test bookmark</p>',
      tags: ['test'],
      public: true,
      userId: '456',
      likeCount: 0,
      youtubeVideoId: null,
      stackoverflowQuestionId: null,
    });

    const resultBookmark = bookmarkRequestMapper.toBookmark(req);

    expect({...resultBookmark.toObject(), _id: {}}).toEqual({...expectedBookmark.toObject(), _id: {}})
    expect(showdown.Converter().makeHtml).not.toHaveBeenCalled();
  });
});
```

**Project**: [`codever`](https://github.com/CodeverDotDev/codever) - **File**:  `bookmark-request.mapper.test.js`

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="63ff7adb1b5b8945055f4e59" %}
