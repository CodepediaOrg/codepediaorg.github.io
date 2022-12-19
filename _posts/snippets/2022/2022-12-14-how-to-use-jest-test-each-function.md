---
layout: post
title: How to use jest test.each function
description: "How to use jest test.each function code snippet"
author: ama
permalink: /snippets/639852fc3b43b9521ecfe046/how-to-use-jest-test-each-function
published: true
snippetId: 639852fc3b43b9521ecfe046
categories: [snippets]
tags: [typescript, jest, testing, codever-snippets]
---

Let's test the following `isInfiniteDate` function, which checks whether the **given** date is "infinite" in the given context:

```typescript
export const isInfiniteDate = (input: string): boolean => {
    const maximumDatePossible = '9999-12-31';
    return new Date(input).getTime() === new Date(maximumDatePossible).getTime();
};
```

For sure, we want to test different dates with the same expected result, either `true` or `false``

To avoid "duplication" of same test with different data, you can use `test.each(table)(name, fn, timeout)` function,
to which you can pass an `Array` of Arrays with the arguments that are passed into the test `fn` for each row.

```typescript
describe('isInfiniteDate > ', () => {
    test.each([
        [null, false],
        [undefined, false],
        ['AXON', false],
        ['2021-31-31', false],
        ['2021-12-12', false],
        ['9999-12-31', true],
    ])('given input date %p , it should return %p ', (input, expected) => {
        expect(isInfiniteDate(input)).toEqual(expected);
    });
});
```

- `name` is the `String` title of the test block -  See the [referenced link](https://jestjs.io/docs/api#testeachtablename-fn-timeout) for the different formatting options
- optionally, you can provide a timeout (in milliseconds) for specifying how long to wait for each row before aborting. The default timeout is 5 seconds.

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://jestjs.io/docs/api#testeachtablename-fn-timeout" target="_blank" style="font-weight: lighter">
     https://jestjs.io/docs/api#testeachtablename-fn-timeout
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="639852fc3b43b9521ecfe046" %}
