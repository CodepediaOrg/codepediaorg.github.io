---
layout: post
title: Calculate life span and age in years in javascript/typescript
description: "Calculate life span and age in years in javascript/typescript code snippet"
author: ama
permalink: /snippets/6426a0931cc7ed0f73e54e83/calculate-life-span-and-age-in-years-in-javascript-typescript
published: true
snippetId: 6426a0931cc7ed0f73e54e83
categories: [snippets]
tags: [typescript, javascript, date, testing, jest, mocking, spy, codever-snippets]
---

Given `birthDate` and day of death as strings the following method calculates the lifespan in years.
To calculate then the age of the person we call the same `calculateLifeSpan` method with the day of death set to today

```javascript
export const calculateLifeSpan = (birthdateString: string, deathDayString: string): number => {
    const birthdate = new Date(birthdateString);
    const dayOfDeath = new Date(deathDayString);
    let age = dayOfDeath.getFullYear() - birthdate.getFullYear();
    const monthDifference = dayOfDeath.getMonth() - birthdate.getMonth();

    if (monthDifference < 0 || (monthDifference === 0 && dayOfDeath.getDate() < birthdate.getDate())) {
        age--;
    }

    return age;
}

export const calculateAge = (birthDateStr: string): number => {
    return calculateLifeSpan(birthDateStr, new Date().toISOString().slice(0, 10))
}
```

> *Note* - you could as well accept the inputs directly as `Date` objects
> and wouldn't need then to transform them to dates with [`new Date()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date) constructor

The following test suite tests the `calculateLifeDuration` function, which takes two dates as strings and calculates the duration in years between them.
It also uses test.each to run multiple tests with different start and end dates and expected durations.

```javascript
describe('calculateLifeDuration', () => {
  test.each([
    ['2021-01-01', '2021-12-31', 0],
    ['1980-01-01', '2023-03-31', 43],
    ['1995-05-15', '2023-03-31', 27],
    ['2010-10-10', '2023-03-31', 12],
  ])('calculates duration correctly between %s and %s', (startDate, endDate, expectedDuration) => {
    expect(calculateLifeDuration(startDate, endDate)).toEqual(expectedDuration);
  });
});
```

The next test suite tests the `calculateAge` function, which takes a birthdate as a string
and calculates the age in years by calling the `calculateLifeDuration` function with the birthdate and the current date.
To mock today's date in the calculateAge tests, we can use Jest's `jest.spyOn` method
to replace the `toISOString` method of the `Date` object with a mocked version that always returns a fixed date.
This way, the calculateAge function will always use the same date when calculating the age, regardless of when the test is run.

```javascript
describe('calculateAge', () => {
  const fixedDate = new Date('2022-03-31T00:00:00.000Z');

  beforeEach(() => {
    jest.spyOn(Date.prototype, 'toISOString').mockReturnValue(fixedDate.toISOString());
  });

  afterEach(() => {
    jest.restoreAllMocks();
  });

  test.each([
    ['1990-01-01', 32],
    ['1980-05-15', 41],
    ['2005-12-31', 16],
    ['1995-10-10', 26],
  ])('calculates age correctly for birthdate %s', (birthdate, expectedAge) => {
    expect(calculateAge(birthdate)).toEqual(expectedAge);
  });
});

```

In this example, we create a `fixedDate` object with a fixed date value that we want to use for testing.
In the `beforeEach` hook, we use `jest.spyOn` to replace the toISOString method of the Date object with a mock function
that always returns the fixedDate value as an ISO string. This ensures that the calculateAge function always uses the fixedDate
when calculating the age, regardless of when the test is run.

In the `afterEach` hook, we restore the original `toISOString` method of the `Date` object to ensure that other tests are not affected by this mock.


<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6426a0931cc7ed0f73e54e83" %}
