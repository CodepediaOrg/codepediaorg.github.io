---
layout: post
title: Use test.each in jest to avoid repetitive tests
description: "This blog post makes a case for using jest test.each wherever possible, by refactoring an existing test, thus
making the code more concise and maintainable and reduce code duplication"
author: ama
permalink: /article/how-to-use-test-each-in-jest-to-avoid-repetitive-tests
published: true
categories: [article]
tags: [jest, testing, node.js, codever]
---

Jest is a popular JavaScript testing framework that provides an intuitive and powerful way to write automated tests for your codebase.
One of the features that make Jest stand out is its [`test.each`](https://jestjs.io/docs/api#testeachtablename-fn-timeout) function,
which enables you to write more concise and readable tests by parameterizing test cases.

With `test.each`, you can define an array of test cases and run the same test function against each test case
with the input arguments substituted. This feature is particularly useful when you need to test a function
with a large number of input combinations, making your test code more maintainable and less verbose.

In this blog post, you'll see an example of how I refactored several repetitive tests to use `test.each` in Jest
and simplify my test code.

<!--more-->

## The method under test

The method under test is `setSpecialSearchTermsFilter`, which extends the document search filter with special
ones you can set on [Codever](https://www.codever.dev):

```javascript
let setSpecialSearchTermsFilter = function (docType, isPublic, userId, specialSearchFilters, filter) {
  let newFilter = {...filter};

  //one is not entitled to see private bookmarks of another user
  if ( specialSearchFilters.userId && (isPublic || specialSearchFilters.userId === userId) ) {
    newFilter.userId = specialSearchFilters.userId;
  }

  if ( specialSearchFilters.privateOnly && !isPublic ) { //
    newFilter.public = false;
  }

  if ( specialSearchFilters.site ) {
    if(docType === DocType.BOOKMARK) {
      newFilter.location = new RegExp(specialSearchFilters.site, 'i');
    } else if (docType === DocType.SNIPPET) {
      newFilter.sourceUrl = new RegExp(specialSearchFilters.site, 'i');//TODO when performance becomes an issue extract domains from URLs and make a direct comparison with the domain
    } else {
      throw new Error(`${docType} is not supported as document type`)
    }
  }

  return newFilter;
};
```

## Before

In the snippet below you can see the tests before using jest `test.each`

```javascript
describe('setSpecialSearchTermsFilter', () => {
  it('should set the userId filter when specialSearchTerms.userId is present and isPublic is true', () => {
    const filter = {};
    const specialSearchTerms = {userId: '123'};
    const result = searchUtils.setSpecialSearchTermsFilter(DocType.SNIPPET, true, '123', specialSearchTerms, filter);
    expect(result).toEqual({userId: '123'});
  });

  it('should set the userId filter when specialSearchTerms.userId is present and matches the userId', () => {
    const filter = {};
    const specialSearchTerms = {userId: '123'};
    const result = searchUtils.setSpecialSearchTermsFilter(DocType.SNIPPET, false, '123', specialSearchTerms, filter);
    expect(result).toEqual({userId: '123'});
  });

  it('should not set the userId filter when specialSearchTerms.userId is present and does not match the userId', () => {
    const filter = {};
    const specialSearchTerms = {userId: '456'};
    const result = searchUtils.setSpecialSearchTermsFilter(DocType.SNIPPET, false, '123', specialSearchTerms, filter);
    expect(result).toEqual({});
  });

  it('should set the public filter to false when specialSearchTerms.privateOnly is present', () => {
    const filter = {};
    const specialSearchTerms = {privateOnly: true};
    const result = searchUtils.setSpecialSearchTermsFilter(DocType.SNIPPET, false, '123', specialSearchTerms, filter);
    expect(result).toEqual({public: false});
  });

  it('should set the sourceUrl filter when specialSearchTerms.site is present for snippets', () => {
    const filter = {};
    const specialSearchTerms = {site: 'example.com'};
    const result = searchUtils.setSpecialSearchTermsFilter(DocType.SNIPPET, false, '123', specialSearchTerms, filter);
    expect(result).toEqual({sourceUrl: /example.com/i});
  });

  it('should set the location filter when specialSearchTerms.site is present for bookmarks', () => {
    const filter = {};
    const specialSearchTerms = {site: 'example.com'};
    const result = searchUtils.setSpecialSearchTermsFilter(DocType.BOOKMARK, false, '123', specialSearchTerms, filter);
    expect(result).toEqual({location: /example.com/i});
  });

  it('should set the location filter when specialSearchTerms.site is present for bookmarks', () => {
    const filter = {};
    const specialSearchTerms = {site: 'example.com'};
    expect(() => searchUtils.setSpecialSearchTermsFilter('unknown', false, '123', specialSearchTerms, filter)).toThrow(Error);
  });
});
```

## After

By using `test.each` the test become more concise and more rapid to read

```javascript
describe('setSpecialSearchTermsFilter', () => {
  describe('valid calls', () => {
    test.each([
      [{}, DocType.SNIPPET, {userId: '123'}, true, '123', {userId: '123'}],
      [{}, DocType.SNIPPET, {userId: '123'}, false, '123', {userId: '123'}],
      [{}, DocType.SNIPPET, {userId: '456'}, false, '123', {}],
      [{}, DocType.SNIPPET, {privateOnly: true}, false, '123', {public: false}],
      [{}, DocType.SNIPPET, {site: 'example.com'}, false, '123', {sourceUrl: /example.com/i}],
      [{}, DocType.BOOKMARK, {site: 'example.com'}, false, '123', {location: /example.com/i}]
    ])('should set the filter correctly', (filter, docType, specialSearchTerms, isPublic, userId, expected) => {
      const result = searchUtils.setSpecialSearchTermsFilter(docType, isPublic, userId, specialSearchTerms, filter);
      expect(result).toEqual(expected);
    });
  });

  it('should throw error when document type not known', () => {
    const filter = {};
    const specialSearchTerms = {site: 'example.com'};
    expect(() => searchUtils.setSpecialSearchTermsFilter('unknown', false, '123', specialSearchTerms, filter)).toThrow(Error);
  });
});
```

Let me explain a bit the signature of  `test.each(table)(name, fn, timeout)`:
- `test.each(table)` is a Jest function that allows you to define a table of input data to use for parameterized tests.
- `table` is an array of arrays, where each sub-array represents a set of input arguments for a test case.
Each set of input arguments is used to run the same test function multiple times, with the input arguments substituted.
- `name` is a string that describes the test case. It should be unique and descriptive enough to identify the test case in the test results.
- `fn` is the test function that takes the input arguments from the `table`
and performs assertions to check if the function under test behaves as expected with those inputs.
- `timeout` is an optional parameter that specifies the timeout for the test case. If the test function takes longer than the specified timeout to complete, Jest will mark the test as failed.

### Test names

Hold on, regarding the `name` you said it should be unique and descriptive to identify the test case in the test results...
You are right and the [`test.each` docs](https://jestjs.io/docs/api#testeachtablename-fn-timeout) offers several possibilities
when setting the name, but one of my favorite ways so far where I have the most control is with the following refactoring
of the test:

```javascript
describe('setSpecialSearchTermsFilter', () => {
  describe('set the filter correctly', () => {
    test.each([
      [
        'should set the userId filter when specialSearchTerms.userId is present and isPublic is true',
        {}, DocType.SNIPPET, {userId: '123'}, true, '123', {userId: '123'}
      ],
      [
        'should set the userId filter when specialSearchTerms.userId is present and matches the userId',
        {}, DocType.SNIPPET, {userId: '123'}, false, '123', {userId: '123'}
      ],
      [
        'should not set the userId filter when specialSearchTerms.userId is present and does not match the userId',
        {}, DocType.SNIPPET, {userId: '456'}, false, '123', {}
      ],
      [
        'should set the public filter to false when specialSearchTerms.privateOnly is present',
        {}, DocType.SNIPPET, {privateOnly: true}, false, '123', {public: false}
      ],
      [
        'should set the sourceUrl filter when specialSearchTerms.site is present for snippets',
        {}, DocType.SNIPPET, {site: 'example.com'}, false, '123', {sourceUrl: /example.com/i}
      ],
      [
        'should set the location filter when specialSearchTerms.site is present for bookmarks',
        {}, DocType.BOOKMARK, {site: 'example.com'}, false, '123', {location: /example.com/i}
      ]
    ])('%s', (testName, filter, docType, specialSearchTerms, isPublic, userId, expected) => {
      const result = searchUtils.setSpecialSearchTermsFilter(docType, isPublic, userId, specialSearchTerms, filter);
      expect(result).toEqual(expected);
    });
  });

  it('should throw error when document type not known', () => {
    const filter = {};
    const specialSearchTerms = {site: 'example.com'};
    expect(() => searchUtils.setSpecialSearchTermsFilter('unknown', false, '123', specialSearchTerms, filter)).toThrow(Error);
  });
});
```

In this refactored test suite, each test case has a unique name (`testName`) which is passed as an argument to the `test.each` function.
The name of the test case is interpolated into the string %s in the template string, which is used as the first argument to test.each.

This way, Jest can generate unique test names for each test case, and if a test case fails, it will be easier to identify which specific test case failed.

> Name setting of jest test was also covered in the other blog post - [An easy way  to set test name in jest repetitive tests (test.each)]({% post_url 2023-03-03-an-easy-way-to-set-test-name-in-jest-repetitive-test-test-each %})

## Conclusion

I hope you could grasp through this blog post that by using `test.each`, you can write concise and maintainable test code,
reduce code duplication, and make your test suite easier to read and understand.

In conclusion, if you're not already using parameterized tests in your Jest test suite,
`test.each` is a great way to get started. It's a powerful tool that can help you write better tests and catch more bugs in your code.

We use a lot of jest `test.each` tests in backend that supports [Codever](https://www.codever.dev). Check out the project
and see for yourself 👇

{% include source-code-codever.html %}
