---
layout: post
title: Verify empty object with Jest (extended)
description: "Verify empty object with Jest (extended) code snippet"
author: ama
permalink: /snippets/63eb2e2488deee44f7554290/verify-empty-object-with-jest-extended-
published: true
snippetId: 63eb2e2488deee44f7554290
categories: [snippets]
tags: [javascript, testing, jest, node.js, unit-testing, codever-snippets]
---

Install [**jest-extended**](https://jest-extended.jestcommunity.dev) (`npm install --save-dev jest-extended`)
and use the `toBeEmpty` assertion as in the following example:

```javascript
const { toBeEmpty } = require('jest-extended');
expect.extend({ toBeEmpty });

describe('setFulltextSearchTermsFilter', () => {
  test('returns filter without $text when fulltextSearchTerms is empty', () => {
    const fulltextSearchTerms = [];
    const filter = {};
    const searchInclude = 'any';
    expect(searchUtils.setFulltextSearchTermsFilter(fulltextSearchTerms, filter, searchInclude)).toBeEmpty();
  });
});
```

Use `.toBeEmpty` when checking if a `String` `''`, `Array` `[]`, `Object` `{}`, or `Iterable` is empty. Because `toBeEmpty` supports checking for emptiness of Iterables, you can use it to check whether a `Map`, or `Set` is empty, as well as checking that a generator yields no values.

**Project**: [`codever`](https://github.com/CodeverDotDev/codever) - **File**:  `search.utils.spec.js`

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://jest-extended.jestcommunity.dev/docs/matchers/tobeempty" target="_blank" style="font-weight: lighter">
     https://jest-extended.jestcommunity.dev/docs/matchers/tobeempty
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="63eb2e2488deee44f7554290" %}
