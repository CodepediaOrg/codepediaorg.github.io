---
layout: post
title: Override nested child element with spread operator in javascript
description: "Override nested child element with spread operator in javascript code snippet"
author: ama
permalink: /snippets/6401910a88deee44f755a5ba/override-nested-child-element-with-spread-operator-in-javascript
published: true
snippetId: 6401910a88deee44f755a5ba
categories: [snippets]
tags: [javascript, codever-snippets]
---

For the following `req` object there is a need to override the `stackoverflowId` attribute in a jest test

```javascript
const req = {
  body: {
    _id: '123',
    name: 'Codever',
    location: 'https://www.codever.dev',
    language: 'en',
    description: 'This is a test bookmark',
    tags: ['test'],
    public: true,
    stackoverflowQuestionId: null
  },
  params: {
    userId: '456',
  },
};
```

**Project**: `codever` - **File**:  `bookmark-request.mapper.test.js`

Here is how to achieve that with the spread operator

```javascript
      {...req,
        body : {
        ...req.body,
          stackoverflowQuestionId: 123456
        }
      }
```

The complete function where this is used is shown below:

```javascript
 test.each([
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
        name: 'Codever',
        location: 'https://www.codever.dev',
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
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6401910a88deee44f755a5ba" %}
