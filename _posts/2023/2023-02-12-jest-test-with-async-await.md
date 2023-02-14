---
layout: post
title: How to run test.each tests in jest with async await
description: "Example showing how to run async await test with jest"
author: ama
permalink: /article/how-to-run-async-await-jest-test-each-example
published: true
categories: [article]
tags: [jest, testing, node.js, codever]
---

## Method to test

Given the `async`/`await` function to **find snippets** on [Codever](https://www.codever.dev)

```javascript
let findSnippets = async function (isPublic, userId, query, page, limit, searchInclude) {
  //split in text and tags
  const searchTermsAndTags = searchUtils.splitSearchQuery(query);
  const searchTerms = searchTermsAndTags.terms;
  const searchTags = searchTermsAndTags.tags;

  const {specialSearchTerms, fulltextSearchTerms} = searchUtils.extractFulltextAndSpecialSearchTerms(searchTerms);

  let filter = {}
  filter = searchUtils.setPublicOrPersonalFilter(isPublic, filter, userId);
  filter = searchUtils.setTagsToFilter(searchTags, filter);
  filter = searchUtils.setFulltextSearchTermsFilter(fulltextSearchTerms, filter, searchInclude);
  filter = searchUtils.setSpecialSearchTermsFilter(isPublic, userId, specialSearchTerms, filter);

  let snippets = await Snippet.find(
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

  return snippets;
}
```

I would like verify if the filter is correctly set for different scenarios.

 <!--more-->

## Test method

[test.each](https://jestjs.io/docs/api#testeachtablename-fn-timeout) is very practical in jest,
if you keep duplicating the same test with different data and is very appropriate here.

In the following example you can see the corresponding test method with jest, that mocks the MondoDB find call
to assert in the end the expected call, all in `async`/`await` style:

```javascript
const snippetsSearchService = require('./snippets-search.service');

const Snippet = require('../model/snippet');
jest.mock('../model/snippet', () => {
  return {
    find: jest.fn(() => ({
      sort: jest.fn(() => ({
        skip: jest.fn(() => ({
          limit: jest.fn(() => ({
            lean: jest.fn(() => ({
              exec: jest.fn(() => Promise.resolve([{_id: '123', name: 'Test Snippet'}]))
            }))
          }))
        }))
      }))
    }))
  };
});

describe('findSnippets to have been called with', () => {
  const input = {
    isPublic: false,
    userId: 'user1',
    page: 1,
    limit: 10,
    searchInclude: 'any'
  }

  test.each([
    [
      {
        ...input,
        query: 'codever testing'
      },
      {
        userId: input.userId,
        $text: {$search: 'codever testing'}
      }
    ],
    [
      {
        ...input,
        userId: undefined,
        isPublic: true,
        query: 'codever testing'
      },
      {
        public: true,
        $text: {$search: 'codever testing'}
      }
    ],
    [
      {
        ...input,
        searchInclude: undefined,
        query: 'codever testing'
      },
      {
        userId: input.userId,
        $text: {$search: '"codever" "testing"'}
      }
    ],
    [
      {
        ...input,
        searchInclude: undefined,
        query: 'codever testing -javascript'
      },
      {
        userId: input.userId,
        $text: {$search: '"codever" "testing" -javascript'}
      }
    ],
    [
      {
        ...input,
        query: '[javascript]'
      },
      {
        userId: input.userId,
        tags:
          {
            $all: ['javascript']
          }
      }
    ],
    [
      {
        ...input,
        query: '[javascript] jest testing'
      },
      {
        userId: input.userId,
        $text: {$search: 'jest testing'},
        tags:
          {
            $all: ['javascript']
          }
      }
    ],
    [
      {
        ...input,
        query: '[javascript] jest testing site:codever.dev'
      },
      {
        userId: input.userId,
        $text: {$search: 'jest testing'},
        tags:
          {
            $all: ['javascript']
          }
      }
    ],
  ])('given input params %p , should use filter %p ', async (input, expectedFilter) => {
    await snippetsSearchService.findSnippets(input.isPublic, input.userId, input.query, input.page, input.limit, input.searchInclude);
    expect(Snippet.find).toHaveBeenCalledWith(expectedFilter, {score: {$meta: 'textScore'}});
  });
});
```

{% include source-code-codever.html %}
