---
layout: post
title: Configure Jest for a Node.js backend project
description: "Install and run first test in Node.js backend project (Codever)"
author: ama
permalink: /article/configure-jest-for-nodejs-backend-project
published: true
categories: [article]
tags: [jest, testing, node.js, codever]
---

The API supporting [Codever](https://www.codever.dev) is written in Node.js/ExpressJs, and I've decided
to use Jest as the testing framework for unit testing. This blog post presents the steps required to install and run the first
unit test in Jest for this backend.

 <!--more-->

## Install Jest

### Install npm dependency
Using `npm` run the following command

```
npm install --save-dev jest
```

This will install `jest` in the `devDependencies` in `package.json` of the backend project

### Write a jest configuration file

In the root of the backend project create a `jest.config.js` configuration file with the following content:

```javascript
module.exports = {
  testEnvironment: 'node',
  testMatch: ['**/*.spec.js'],
};
```

> This tells the Jest runner to run all the files in the project ending in `spec.js`

## Run first tests in Jest

In `package.json` under the `scripts` property define a `test` task for running jest tests - `"test": "jest --config ./jest.config.js"`:

```
  "scripts": {
    "start": "node ./bin/www",
    "debug": "nodemon --inspect ./bin/www --watch src --watch docs/openapi/openapi.yaml",
    "test": "jest --config ./jest.config.js"
  },
```

With this configuration with `npm run` you can execute the script for running jest tests - `npm run test`

But for that we need to first write a Jest test.
I will choose some `utils` methods used in searching bookmarks, snippets or notes to write first jest tests.

Let's take the following method for example, from `search.utils.js`

```javascript
let includeFulltextSearchTermsInFilter = function (fulltextSearchTerms, filter, searchInclude) {
  let newFilter = {...filter};
  if ( fulltextSearchTerms.length > 0 ) {
    let searchText = '';
    if ( searchInclude === 'any' ) {
      searchText = {$search: fulltextSearchTerms.join(' ')}
    } else {
      searchText = {$search: generateFullSearchText(fulltextSearchTerms)};
    }

    newFilter.$text = searchText;
  }
  return newFilter;
}
```

> This method returns a new filter containing also the fulltext search depending on the OR or AND selection, but
> the best way to understand it, is looking at its corresponding test:

```javascript
const searchUtils = require('./search.utils');

describe('includeFulltextSearchTermsInFilter', () => {
    test('returns filter with $text when fulltextSearchTerms is not empty', () => {
        const fulltextSearchTerms = ['testing jest'];
        const filter = {};
        const searchInclude = 'any';
        const expected = {
            ...filter,
            $text: {$search: fulltextSearchTerms.join(' ')}
        };
        expect(searchUtils.includeFulltextSearchTermsInFilter(fulltextSearchTerms, filter, searchInclude)).toEqual(expected);
    });

    test('returns filter without $text when fulltextSearchTerms is empty', () => {
        const fulltextSearchTerms = [];
        const filter = {};
        const searchInclude = 'any';
        expect(searchUtils.includeFulltextSearchTermsInFilter(fulltextSearchTerms, filter, searchInclude)).toBe(undefined);
    });
});
```

Now every time I run `npm run test` this test will also be executed. Easy peasy.

{% include source-code-codever.html %}
