---
layout: post
title: Compare date (no time) with current day in javascript
description: "Compare date (no time) with current day in javascript code snippet"
author: ama
permalink: /snippets/6398a6a53b43b9521ecfe361/compare-date-no-time-with-current-day-in-javascript
published: true
snippetId: 6398a6a53b43b9521ecfe361
categories: [snippets]
tags: [typescript, javascript, testing, date, datetime, jest, codever-snippets]
---

Get current date with `new Date()` and `setHours` to `0, 0, 0` and then you are ready to compare with the input date, which is a string in the `yyyy-MM-dd` format

```javascript
export const isLessThanToday = (input: string): boolean => { //format of input date is YYYY-MM-DD
    const today = new Date();
    today.setHours(0, 0, 0);

    return notEmpty(input) && new Date(input) < today;
};
```

To test that we can use the following jest test:

```javascript
    describe('isLessThanToday > ', () => {
        test.each([
            [null, false],
            [undefined, false],
            ['AXON', false],
            ['1900-01-01', true],
            ['2099-12-12', false], // TODO change this date when in 2099 :)
            [new Date().toISOString().slice(0, 10), false], //today
            [new Date(new Date().setDate(new Date().getDate() - 1)).toISOString().slice(0, 10), true], //yesterday
            [new Date(new Date().setDate(new Date().getDate() - 7)).toISOString().slice(0, 10), true], //one week ago
        ])('given input date %p, it should return %p', (input, expected) => {
            expect(isLessThanToday(input)).toEqual(expected);
        });
    });
```

See this [How to use jest test.each function ](https://www.codever.dev/snippets/639852fc3b43b9521ecfe046/details) to understand the usage of `test.each` function

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6398a6a53b43b9521ecfe361" %}
