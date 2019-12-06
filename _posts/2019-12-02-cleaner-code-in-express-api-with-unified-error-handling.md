---
layout: post
title: Cleaner code in an Express REST API with unified error handling
description: "Shows how you can make your backend ExpressJS REST API cleaner by using custom error handling middleware. Code snippets of before and after
refactoring are presented to make the point"
author: ama
permalink: /ama/cleaner-code-in-expressjs-rest-api-with-custom-error-handling
published: true
categories: [clean-code]
tags: [expressjs, nodejs, rest, api]
---

What started out as a simple code duplication removal, turned out into [a major refactoring](https://github.com/CodepediaOrg/bookmarks.dev-api/commit/fc2ba3339909d15f43450ba8ffbf472bacaef429)
 with complete rewriting of error handling, moving of business logic/db access into separate service files (about this in another blog post)
  and rewriting of all integration tests to use async/await. In this blog post I will focus on the custom error handling and how
  it made the code much cleaner for the [REST API](https://github.com/CodepediaOrg/bookmarks.dev-api) supporting [www.bookmarks.dev](https://www.bookmarks.dev).
  The API uses [ExpressJS](https://expressjs.com), currently in version 4. 

<!--more-->

* TOC
{:toc}

## Refactoring
To make my point I will show you an example of **before** and **after** code. In the **after** part where I drill down into the details
taking a top down approach.   

### Before

Our sample candidate is the router method where a personal bookmark is created:

```javascript
personalBookmarksRouter.post('/', keycloak.protect(), async (request, response) => {

  let userId = request.kauth.grant.access_token.content.sub;
  if ( userId !== request.params.userId ) {
    return response
      .status(HttpStatus.UNAUTHORIZED)
      .send(new MyError('Unauthorized', ['the userId does not match the subject in the access token']));
  }

  const bookmark = bookmarkHelper.buildBookmarkFromRequest(request);

  if ( bookmark.userId !== userId ) {
    return response
      .status(HttpStatus.BAD_REQUEST)
      .send(new MyError('The userId of the bookmark does not match the userId parameter', ['The userId of the bookmark does not match the userId parameter']));
  }

  const missingRequiredAttributes = !bookmark.name || !bookmark.location || !bookmark.tags || bookmark.tags.length === 0;
  if ( missingRequiredAttributes ) {
    return response
      .status(HttpStatus.BAD_REQUEST)
      .send(new MyError('Missing required attributes', ['Missing required attributes']));
  }
  if ( bookmark.tags.length > constants.MAX_NUMBER_OF_TAGS ) {
    return response
      .status(HttpStatus.BAD_REQUEST)
      .send(new MyError('Too many tags have been submitted', ['Too many tags have been submitted']));
  }

  let blockedTags = '';
  for ( let i = 0; i < bookmark.tags.length; i++ ) {
    const tag = bookmark.tags[i];
    if ( tag.startsWith('awesome') ) {
      blockedTags = blockedTags.concat(' ' + tag);
    }
  }

  if ( blockedTags ) {
    return response
      .status(HttpStatus.BAD_REQUEST)
      .send(new MyError('The following tags are blocked:' + blockedTags, ['The following tags are blocked:' + blockedTags]));
  }

  if ( bookmark.description ) {
    const descriptionIsTooLong = bookmark.description.length > constants.MAX_NUMBER_OF_CHARS_FOR_DESCRIPTION;
    if ( descriptionIsTooLong ) {
      return response
        .status(HttpStatus.BAD_REQUEST)
        .send(new MyError('The description is too long. Only ' + constants.MAX_NUMBER_OF_CHARS_FOR_DESCRIPTION + ' allowed',
          ['The description is too long. Only ' + constants.MAX_NUMBER_OF_CHARS_FOR_DESCRIPTION + ' allowed']));
    }

    const descriptionHasTooManyLines = bookmark.description.split('\n').length > constants.MAX_NUMBER_OF_LINES_FOR_DESCRIPTION;
    if ( descriptionHasTooManyLines ) {
      return response
        .status(HttpStatus.BAD_REQUEST)
        .send(new MyError('The description hast too many lines. Only ' + constants.MAX_NUMBER_OF_LINES_FOR_DESCRIPTION + ' allowed',
          ['The description hast too many lines. Only ' + constants.MAX_NUMBER_OF_LINES_FOR_DESCRIPTION + ' allowed']));
    }
  }

  if ( bookmark.shared ) {
    const existingBookmark = await Bookmark.findOne({
      shared: true,
      location: bookmark.location
    }).lean().exec();
    if ( existingBookmark ) {
      return response
        .status(HttpStatus.CONFLICT)
        .send(new MyError('A public bookmark with this location is already present',
          ['A public bookmark with this location is already present']));
    }
  }

  try {
    let newBookmark = await bookmark.save();

    response
      .set('Location', `${config.basicApiUrl}private/${request.params.userId}/bookmarks/${newBookmark.id}`)
      .status(HttpStatus.CREATED)
      .send({response: 'Bookmark created for userId ' + request.params.userId});

  } catch (err) {
    const duplicateKeyinMongoDb = err.name === 'MongoError' && err.code === 11000;
    if ( duplicateKeyinMongoDb ) {
      return response
        .status(HttpStatus.CONFLICT)
        .send(new MyError('Duplicate key', [err.message]));
    }
    response
      .status(HttpStatus.INTERNAL_SERVER_ERROR)
      .send(err);
  }

});
```

There are several issues with this code. To name a few:
* for one it is way too long for a single method
* the userId validation in the beginning is a pattern used in all methods protected
with Keycloak (btw. this was the trigger of the refactoring)
* if one validation exception occurs the code breaks and sends the response to the caller, potentially missing validation 
exceptions
* try/catch block around the database access, which is diligently used in the whole code base; I mean it's good that is there, but
maybe you can get rid of it

Now let's see the refactoring result.

### After

```javascript
personalBookmarksRouter.post('/', keycloak.protect(), async (request, response) => {

  UserIdValidator.validateUserId(request);
  
  const bookmark = bookmarkHelper.buildBookmarkFromRequest(request);
  let newBookmark = await PersonalBookmarksService.createBookmark(request.params.userId, bookmark);

  response
    .set('Location', `${config.basicApiUrl}private/${request.params.userId}/bookmarks/${newBookmark.id}`)
    .status(HttpStatus.CREATED)
    .send({response: 'Bookmark created for userId ' + request.params.userId});

});
```

Notice that the method is much shorter. 

#### Remove try/catch around code

The try/catch block has been eliminated, but there is a hack. When an error
is thrown it does not go to the error middleware, because express does not support promises currently and you will get 
an `UnhandledPromiseRejectionWarning`. The initial solution was to wrap the async function with a wrapper that would
have a `catch` call, which would forward the error to the next middleware:

```javascript
let wrapAsync = function (fn) {
  return function(req, res, next) {
    // Make sure to `.catch()` any errors and pass them along to the `next()`
    // middleware in the chain, in this case the error handler.
    fn(req, res, next).catch(next);
  };
}
```

This meant calling the function like the following:

```javascript
personalBookmarksRouter.post('/', keycloak.protect(), AsyncWrapper.wrapAsync(async (request, response) => {

  UserIdValidator.validateUserId(request);
  const bookmark = bookmarkHelper.buildBookmarkFromRequest(request);
  let newBookmark = await PersonalBookmarksService.createBookmark(request.params.userId, bookmark);

  response
    .set('Location', `${config.basicApiUrl}private/${request.params.userId}/bookmarks/${newBookmark.id}`)
    .status(HttpStatus.CREATED)
    .send({response: 'Bookmark created for userId ' + request.params.userId});

}));
```

Other options are mentioned on [javascript - Handling errors in express async middleware - Stack Overflow](https://stackoverflow.com/questions/51391080/handling-errors-in-express-async-middleware)

But later I found the [express-async-errors](https://github.com/davidbanham/express-async-errors) script, which you only
need to require **before** start using it:
```javascript
const express = require('express');
require('express-async-errors');
``` 
Then you are good to go - no wrapper needed. 

#### UserId validation

The userId validation was moved to its own file:
```javascript
let validateUserId = function (request) {
     const userId = request.kauth.grant.access_token.content.sub;
     if (userId !== request.params.userId) {
       throw new UseridTokenValidationError('the userId does not match the subject in the access token');
     }
   }
```

Now instead of returning a response, a custom `UserIdValidationError` exception is thrown:
```javascript
class UserIdValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'UserIdValidationError'
  }
}
```

The exception is handled by an [error-handling middleware](https://expressjs.com/en/guide/error-handling.html) in the `app.js` file:

```javascript
app.use(function handleUserIdValidationError(error, req, res, next) {
  if (error instanceof UseridValidationError) {
    res.status(HttpStatus.UNAUTHORIZED);
    return res.send({
      httpStatus: HttpStatus.UNAUTHORIZED,
      message: error.message
    });
  }
  next(error);
});
```

> Error-handling middleware functions are similar as other middleware functions, except they have four
arguments instead of three: `(err, req, res, next)`.


#### Service method 

The service method `PersonalBookmarksService.createBookmark`, takes now the task of input validation and 
saving the data to the database:
```javascript
let createBookmark = async function (userId, bookmark) {
  BookmarkInputValidator.validateBookmarkInput(userId, bookmark);

  await BookmarkInputValidator.verifyPublicBookmarkExistenceOnCreation(bookmark);

  let newBookmark = await bookmark.save();

  return newBookmark;
}
```
> The service is "Express"-free, so I could easily change the API Framework but keep the business logic part intact

#### Input validation handling

Let's focus now on the input validation handling - `BookmarkInputValidator.validateBookmarkInput(userId, bookmark)`:

```javascript
function validateBookmarkInput(userId, bookmark) {
  let validationErrorMessages = [];
  
  if (bookmark.userId !== userId) {
    validationErrorMessages.push("The userId of the bookmark does not match the userId parameter");
  }  

  if (!bookmark.userId) {
    validationErrorMessages.push('Missing required attribute - userId');
  }

  if (!bookmark.name) {
    validationErrorMessages.push('Missing required attribute - name');
  }
  if (!bookmark.location) {
    validationErrorMessages.push('Missing required attribute - location');
  }
  if (!bookmark.tags || bookmark.tags.length === 0) {
    validationErrorMessages.push('Missing required attribute - tags');
  } else if (bookmark.tags.length > constants.MAX_NUMBER_OF_TAGS) {
    validationErrorMessages.push('Too many tags have been submitted - max allowed 8');
  }

  let blockedTags = '';
  for (let i = 0; i < bookmark.tags.length; i++) {
    const tag = bookmark.tags[i];
    if (tag.startsWith('awesome')) {
      blockedTags = blockedTags.concat(' ' + tag);
    }
  }

  if (blockedTags) {
    validationErrorMessages.push('The following tags are blocked:' + blockedTags);
  }

  if (bookmark.description) {
    const descriptionIsTooLong = bookmark.description.length > constants.MAX_NUMBER_OF_CHARS_FOR_DESCRIPTION;
    if (descriptionIsTooLong) {
      validationErrorMessages.push('The description is too long. Only ' + constants.MAX_NUMBER_OF_CHARS_FOR_DESCRIPTION + ' allowed');
    }

    const descriptionHasTooManyLines = bookmark.description.split('\n').length > constants.MAX_NUMBER_OF_LINES_FOR_DESCRIPTION;
    if (descriptionHasTooManyLines) {
      validationErrorMessages.push('The description hast too many lines. Only ' + constants.MAX_NUMBER_OF_LINES_FOR_DESCRIPTION + ' allowed');
    }
  }

  if(validationErrorMessages.length > 0){
    throw new ValidationError('The bookmark you submitted is not valid', validationErrorMessages);
  }
}
```

Notice how the validation failures are now collected, instead of breaking the process flow when one occurs. At the end, if present,
 they are all packed and thrown via a custom exception:
 ```javascript
 class ValidationError extends Error {
   constructor(message, validatinErrors) {
     super(message);
     this.validationErrors = validatinErrors;
     this.name = 'ValidationError'
   }
 }
 ```

which is treated by a specialised error-handling middleware down the line:

```javascript
app.use(function handleValidationError(error, request, response, next) {
  if (error instanceof ValidationError) {
    return response
      .status(HttpStatus.BAD_REQUEST)
      .json({
        httpStatus: HttpStatus.BAD_REQUEST,
        message: error.message,
        validationErrors: error.validationErrors
      });
  }
  next(error);
});
```

#### Error handling middleware

Please find below the complete error handling middleware:

```javascript
app.use(function handleNotFoundError(error, req, res, next) {
  if (error instanceof NotFoundError) {
    return res.status(HttpStatus.NOT_FOUND).send({
      httpStatus: HttpStatus.NOT_FOUND,
      message: error.message,
      error: {}
    });
  }
  next(error);
});

app.use(function handlePublicBookmarkExistingError(error, req, res, next) {
  if (error instanceof PublicBookmarkExistingError) {
    return res.status(HttpStatus.CONFLICT).send({
      httpStatus: HttpStatus.CONFLICT,
      message: error.message,
      error: {}
    });
  }
  next(error);
});

app.use(function handleUserIdValidationError(error, req, res, next) {
  if (error instanceof UseridTokenValidationError) {
    res.status(HttpStatus.UNAUTHORIZED);
    return res.send({
      httpStatus: HttpStatus.UNAUTHORIZED,
      message: error.message
    });
  }
  next(error);
});

app.use(function handleValidationError(error, request, response, next) {
  if (error instanceof ValidationError) {
    return response
      .status(HttpStatus.BAD_REQUEST)
      .json({
        httpStatus: HttpStatus.BAD_REQUEST,
        message: error.message,
        validationErrors: error.validationErrors
      });
  }
  next(error);
});

app.use(function handleDatabaseError(error, request, response, next) {
  if (error instanceof MongoError) {
    if (error.code === 11000) {
      return response
        .status(HttpStatus.CONFLICT)
        .json({
          httpStatus: HttpStatus.CONFLICT,
          type: 'MongoError',
          message: error.message
        });
    } else {
      return response.status(503).json({
        httpStatus: HttpStatus.SERVICE_UNAVAILABLE,
        type: 'MongoError',
        message: error.message
      });
    }
  }
  next(error);
});

// production error handler
// no stacktraces leaked to user
app.use(function (error, req, res, next) {
  if (res.headersSent) {
    return next(error)
  } else {
    res.status(error.status || HttpStatus.INTERNAL_SERVER_ERROR);
    res.send({
      message: error.message,
      error: {}
    });
  }

});
```

> Instead of having one `app.use(function (error, req, res, next)` middleware with nested if/else statements verifying 
the error type, I preferred to check for one exception type or else forward it to the next middleware. I think this improved
the readability, and it's a pattern I used in some of the routers too


#### Code size reduction
Not only that the code is much cleaner now, it takes also less Javascript code (from 3441 to 3012), although the number of Javascript files
has grown by 9 (from 32 to 41). To measure this I used [cloc](https://github.com/AlDanial/cloc) - `brew install cloc` for MacOS


**Before**
```
$ cloc $(git ls-files)
      69 text files.
      69 unique files.                              
      18 files ignored.
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
JSON                             5              0              0           7964
JavaScript                      32            502            125           3441
YAML                             3              8             36            351
Markdown                         5            114              0            224
Dockerfile                       1             11             18             17
-------------------------------------------------------------------------------
SUM:                            46            635            179          11997
-------------------------------------------------------------------------------

```

**After**
```shell
$ cloc $(git ls-files)
      68 text files.
      68 unique files.                              
      18 files ignored.

github.com/AlDanial/cloc v 1.84  T=0.36 s (159.2 files/s, 37246.4 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
JSON                             6              0              0           8603
JavaScript                      41            713            186           3007
YAML                             3              8             36            351
Markdown                         6            127              0            257
Dockerfile                       1             11             18             17
-------------------------------------------------------------------------------
SUM:                            57            859            240          12235
-------------------------------------------------------------------------------

```


## Conclusion
It's not much left to say, I think that Express offers a decent way to handle exceptions. I hope you've learned something from this post 
and if you have any improvement please leave a comment, or better make a pull request at [bookmarks.dev-api github repo](https://github.com/CodepediaOrg/bookmarks.dev-api).

<p class="note_normal">
During refactoring I researched quite a few links, and I bookmarked the best of them at <a href="https://www.bookmarks.dev" target="_blank">www.bookmarks.dev</a>
 with the following tags <a href="https://www.bookmarks.dev/?tab=search-results&q=%5Bexpressjs%5D%20%5Berror-handling%5D%20%5Basync-await%5D&sd=public" target="_blank">[
 [expressjs] [error-handling] [async-await]</a> 
 <br/>  

They will be present shortly at the generated public bookmarks - <a href="https://github.com/CodepediaOrg/bookmarks">https://github.com/CodepediaOrg/bookmarks</a>
</p>
