---
layout: post
title: Test error with array messages content containing message in jest test.each
description: "Test error with array messages content containing message in jest test.each code snippets"
author: ama
permalink: /snippets/640858c71b5b8945055f7c42/test-error-with-array-messages-content-containing-message-in-jest-test-each
published: true
snippetId: 640858c71b5b8945055f7c42
categories: [snippets]
tags: [javascript, testing, jest, codever-snippets]
---

When validating a **note**, **bookmark** or **snippet** on [Codever](https://www.codever.dev) several validation rules
might be broken and the response given to the consumer will contain **all** the corresponding error messages.

In the following example we will consider the case of **notes** and how we can verify in **jest** repetitive tests
if a broken rule is present in the error response. To do that we surround the method under test in a `try` `catch` block
and use jest `expect.arrayContaining` assertion on the `validationErrors` array in the error content


```javascript
describe('validateNoteInput', () => {
  test.each([
    // Test cases go here as an array of arrays
    // Each inner array represents a set of arguments to pass to the function
    // The last element of the inner array is the expected error message
    [123, { userId: null, title: 'Test', content: 'This is a test note' }, NoteValidationErrorMessages.MISSING_USER_ID],
    [456, { userId: 789, title: 'Test', content: 'This is a test note' }, NoteValidationErrorMessages.USER_ID_NOT_MATCHING],
    [111, { userId: 111, title: null, content: 'This is a test note' }, NoteValidationErrorMessages.MISSING_TITLE],
    [222, { userId: 222, title: 'Test', content: null }, NoteValidationErrorMessages.MISSING_CONTENT],
    [333, { userId: 333, title: 'Test', content: 'x'.repeat(NoteValidationRules.MAX_NUMBER_OF_CHARS_FOR_CONTENT + 1) }, NoteValidationErrorMessages.CONTENT_TOO_LONG],
  ])('throws a ValidationError with the correct error message', (userId, note, expectedErrorMessage) => {
    try {
    expect(() => noteInputValidator.validateNoteInput(userId, note)).toThrowError(ValidationError);
    expect(() => noteInputValidator.validateNoteInput(userId, note)).toThrowError(NoteValidationErrorMessages.NOTE_NOT_VALID);
    } catch (error) {
      // If the function threw an error, test that the error message is correct
      expect(error.validationErrors).toEqual(expect.arrayContaining(expectedErrorMessage));
    }
  });
});
```

**Project**: [`codever`](https://github.com/CodeverDotDev/codever) - **File**:  `note-input.validator.test.js`

The method under test is defined in the following snippet

```javascript
const ValidationError = require('../../../error/validation.error');

let validateNoteInput = function (userId, note) {

  let validationErrorMessages = [];

  if ( !note.userId ) {
    validationErrorMessages.push(NoteValidationErrorMessages.MISSING_USER_ID);
  }

  if ( note.userId !== userId ) {
    validationErrorMessages.push(NoteValidationErrorMessages.MISSING_USER_ID);
  }

  if ( !note.title ) {
    validationErrorMessages.push(NoteValidationErrorMessages.MISSING_TITLE);
  }

  if ( !note.content ) {
    validationErrorMessages.push(NoteValidationErrorMessages.MISSING_CONTENT);
  }

  if ( note.content ) {
    const descriptionIsTooLong = note.content.length > NoteValidationRules.MAX_NUMBER_OF_CHARS_FOR_CONTENT;
    if ( descriptionIsTooLong ) {
      validationErrorMessages.push(NoteValidationErrorMessages.CONTENT_TOO_LONG);
    }
  }

  if ( validationErrorMessages.length > 0 ) {
    throw new ValidationError(NoteValidationErrorMessages.NOTE_NOT_VALID, validationErrorMessages);
  }
}

const NoteValidationRules = {
  MAX_NUMBER_OF_CHARS_FOR_CONTENT: 10000,
  MAX_NUMBER_OF_TAGS: 8
}

const NoteValidationErrorMessages = {
  NOTE_NOT_VALID: 'The note you submitted is not valid',
  MISSING_USER_ID: 'Missing required attribute - userId',
  USER_ID_NOT_MATCHING: 'The userId of the bookmark does not match the userId parameter',
  MISSING_TITLE: 'Missing required attribute - title',
  MISSING_CONTENT: 'Missing required attribute - content',
  CONTENT_TOO_LONG: `The content is too long. Only ${NoteValidationRules.MAX_NUMBER_OF_CHARS_FOR_CONTENT} allowed`,
}

module.exports = {
  validateNoteInput: validateNoteInput,
  NoteValidationRules: NoteValidationRules,
  NoteValidationErrorMessages: NoteValidationErrorMessages
};

```


<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://github.com/CodeverDotDev/codever" target="_blank" style="font-weight: lighter">
     https://github.com/CodeverDotDev/codever
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="640858c71b5b8945055f7c42" %}
