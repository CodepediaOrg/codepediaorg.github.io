---
layout: post
title: An easy way  to set test name in jest repetitive tests (test.each)
description: "An easy way  to set test name in jest repetitive tests (test.each) code snippet"
author: ama
permalink: /snippets/64004a8a88deee44f7559f76/an-easy-way-to-set-test-name-in-jest-repetitive-each-test
published: true
snippetId: 64004a8a88deee44f7559f76
categories: [snippets]
tags: [javascript, testing, jest, node.js, codever-snippets]
---

One easy way to display a custom name for each `test.each` test in jest,
is to place the text name in the first element of each array of the input table for the method
and use it as name in the `test.each` method - `('%s', (testname, req, expectedBookmark) `, as in the following snippet:

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

  test.each([
    [
      'should return a new bookmark',
      req,
      new Bookmark({
        _id: 123,
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
      })
    ],
    [
      'should set youtubeVideoId if it is provided',
      {...req,
        body : {
        ...req.body,
          youtubeVideoId: 'abcd1234'
        }
      },
      new Bookmark({
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
        youtubeVideoId: 'abcd1234',
        stackoverflowQuestionId: null,
      })
    ],
    [
      'should set stackoverflowQuestionId if it is provided',
      {...req,
        body : {
        ...req.body,
          stackoverflowQuestionId: 123456
        }
      },
      new Bookmark({
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
        stackoverflowQuestionId: 123456,
      })
    ],
  ])('%s', (testname, req, expectedBookmark) => {
    const resultBookmark = bookmarkRequestMapper.toBookmark(req);

    expect({...resultBookmark.toObject(), _id: {}}).toEqual({...expectedBookmark.toObject(), _id: {}});
    expect(showdown.Converter).toHaveBeenCalledTimes(1);
    expect(showdown.Converter().makeHtml).toHaveBeenCalledTimes(1);
    expect(showdown.Converter().makeHtml).toHaveBeenCalledWith('This is a test bookmark');
  });
});
```

**Project**: `codever` - **File**:  `bookmark-request.mapper.test.js`

Of course you are free to display other data where appropiate...

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://jestjs.io/docs/api#testeachtablename-fn-timeout" target="_blank" style="font-weight: lighter">
     https://jestjs.io/docs/api#testeachtablename-fn-timeout
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="64004a8a88deee44f7559f76" %}
