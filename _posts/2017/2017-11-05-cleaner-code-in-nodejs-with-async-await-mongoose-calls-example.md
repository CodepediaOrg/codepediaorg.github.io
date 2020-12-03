---
layout: post
title: Cleaner code in NodeJs with async-await - Mongoose calls example
description: "Example showing migration of Mongoose calls from previously using callbacks to using the new
async-await feature in NodeJs"
author: ama
permalink: /ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example
published: true
categories: [tutorial]
tags:
    - javascript
    - node.js
    - mongoose
    - async-await
    - bookmarks.dev
---

A lot has been written already about the transition from [callbacks](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
to [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) and now to the new
`async/await`[^1] feature in ES7.

<p class="note_normal">
  "The purpose of `async/await` functions is to simplify the behavior of using promises synchronously and to perform some behavior on a group of `Promises`.
   Just as `Promises` are similar to structured callbacks, `async/await` is similar to combining generators and promises."[^1]
</p>

[^1]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function>

In this blog post I present what this code "upgrade" meant for [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
   performed on [dev bookmarks](https://www.bookmarks.dev). I use [Moongoose](http://mongoosejs.com/) in an ExpressJS/NodeJS backend
    to perform the operations against a MongoDB database.

> In a later refactoring the database access part has been decoupled from ExpressJS and moved to separate service functions.
Thanks to [unified error handling](https://www.codepedia.org/ama/cleaner-code-in-expressjs-rest-api-with-custom-error-handling) the database errors are now handled centrally.

{% include source-code-bookmarks.dev.html %}

<!--more-->

* TOC
{:toc}

## Create

### Before
```javascript
router.post('/:id/bookmarks', keycloak.protect(), function(req, res, next){
  const descriptionHtml = req.body.descriptionHtml ? req.body.descriptionHtml: converter.makeHtml(req.body.description);

  console.log(req.body);

  var bookmark = new Bookmark({
    name: req.body.name,
    location: req.body.location,
    language: req.body.language,
    description: req.body.description,
    descriptionHtml: descriptionHtml,
    category: req.body.category,
    tags: req.body.tags,
    publishedOn: req.body.publishedOn,
    githubURL: req.body.githubURL,
    userId: req.params.id,
    shared: req.body.shared,
    starredBy: req.body.starredBy
  });

  console.log('Bookmark to create ' + bookmark);

  bookmark.save(function (err, updatedBookmark) {
    if (err){

      if(err.name == 'ValidationError'){
        var errorMessages = [];
        for (var i in err.errors) {
          errorMessages.push(err.errors[i].message);
        }

        var error = new Error('Validation MyError', errorMessages);
        console.log(JSON.stringify(error));
        res.setHeader('Content-Type', 'application/json');
        return res.status(409).send(JSON.stringify(new MyError('Validation Error', errorMessages)));
      }

      console.log(err);
      res.status(500).send(err);
    } else {
      res.set('Location', 'http://localhost:3000/' + req.params.id + '/bookmarks/' + updatedBookmark.id);
      res.status(201).send({response:'Bookmark created for userId ' + req.params.id});
    }
    // saved!
  });

});
```

### After

#### Router
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

#### Service
```javascript
let createBookmark = async function (userId, bookmark) {
  BookmarkInputValidator.validateBookmarkInput(userId, bookmark);

  await BookmarkInputValidator.verifyPublicBookmarkExistenceOnCreation(bookmark);

  let newBookmark = await bookmark.save();

  return newBookmark;
}
```

## Read

### Before
```javascript
/* GET bookmarks for user */
router.get('/:userId/bookmarks/:bookmarkId', keycloak.protect(), function(req, res, next) {
    Bookmark.findOne({_id: bookmarkId, userId:req.params.userId}, function(err, bookmark){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmark);
    });
});
```

### After

#### Router
```javascript
/* GET bookmarks for user */
personalBookmarksRouter.get('/:bookmarkId', keycloak.protect(), async (request, response) => {
  UserIdValidator.validateUserId(request);

  const {userId, bookmarkId} = request.params;
  const bookmark = await PersonalBookmarksService.getBookmarkById(userId, bookmarkId);

  return response.status(HttpStatus.OK).send(bookmark);
});
```

#### Service
```javascript
let getBookmarkById = async (userId, bookmarkId) => {
  const bookmark = await Bookmark.findOne({
    _id: bookmarkId,
    userId: userId
  });

  if (!bookmark) {
    throw new NotFoundError(`Bookmark NOT_FOUND the userId: ${userId} AND id: ${bookmarkId}`);
  } else {
    return bookmark;
  }
};
```

## Update

### Before
```javascript
/**
 * full UPDATE via PUT - that is the whole document is required and will be updated
 * the descriptionHtml parameter is only set in backend, if only does not come front-end (might be an API call)
 */
router.put('/:userId/bookmarks/:bookmarkId', keycloak.protect(), function(req, res, next) {
  if(!req.body.descriptionHtml){
    req.body.descriptionHtml = converter.makeHtml(req.body.description);
  }
  Bookmark.findOneAndUpdate({_id: req.params.bookmarkId, userId: req.params.userId}, req.body, {new: true}, function(err, bookmark){
    if(err){
      if (err.name === 'MongoError' && err.code === 11000) {
        res.status(409).send(new MyError('Duplicate key', [err.message]));
      }

      res.status(500).send(new MyError('Unknown Server Error', ['Unknow server error when updating bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId]));
    }
    if(!bookmark){
      return res.status(404).send('Bookmark not found for user');
    }
    res.status(200).send(bookmark);
  });

});
```

### After

#### Router
```javascript
/**
 * full UPDATE via PUT - that is the whole document is required and will be updated
 * the descriptionHtml parameter is only set in backend, if only does not come front-end (might be an API call)
 */
personalBookmarksRouter.put('/:bookmarkId', keycloak.protect(), async (request, response) => {

  UserIdValidator.validateIsAdminOrUserId(request);

  const bookmark = bookmarkHelper.buildBookmarkFromRequest(request);

  const {userId, bookmarkId} = request.params;
  const updatedBookmark = await PersonalBookmarksService.updateBookmark(userId, bookmarkId, bookmark);

  return response.status(HttpStatus.OK).send(updatedBookmark);
});
```

#### Service

```javascript
let updateBookmark = async (userId, bookmarkId, bookmark) => {

  BookmarkInputValidator.validateBookmarkInput(userId, bookmark);

  await BookmarkInputValidator.verifyPublicBookmarkExistenceOnUpdate(bookmark, userId);

  const updatedBookmark = await Bookmark.findOneAndUpdate(
    {
      _id: bookmarkId,
      userId: userId
    },
    bookmark,
    {new: true}
  );

  const bookmarkNotFound = !updatedBookmark;
  if (bookmarkNotFound) {
    throw new NotFoundError('Bookmark NOT_FOUND with id: ' + bookmarkId + ' AND location: ' + bookmark.location);
  } else {
    return updatedBookmark;
  }
};
```

## Delete

### Before
```javascript
/*
* DELETE bookmark for user
*/
router.delete('/:userId/bookmarks/:bookmarkId', keycloak.protect(), function(req, res, next) {
  Bookmark.findOneAndRemove({_id: req.params.bookmarkId, userId: req.params.userId}, function(err, bookmark){
    if(err){
      return res.status(500).send(new MyError('Unknown server error', ['Unknown server error when trying to delete bookmark with id ' + req.params.bookmarkId]));
    }
    if(!bookmark){
      return res.status(404).send(new MyError('Not Found Error', ['Bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId + ' not found']));
    }
    res.status(204).send('Bookmark successfully deleted');
  });

});
```

### After

#### Router
```javascript
/*
* DELETE bookmark for user
*/
personalBookmarksRouter.delete('/:bookmarkId', keycloak.protect(), async (request, response) => {

  UserIdValidator.validateIsAdminOrUserId(request);

  await PersonalBookmarksService.deleteBookmarkById(request.params.userId, request.params.bookmarkId);
  return response.status(HttpStatus.NO_CONTENT).send();
});
```

#### Service
```javascript
let deleteBookmarkById = async (userId, bookmarkId) => {
  const bookmark = await Bookmark.findOneAndRemove({
    _id: bookmarkId,
    userId: userId
  });

  if (!bookmark) {
    throw new NotFoundError('Bookmark NOT_FOUND with id: ' + bookmarkId);
  }

  return true;
};
```

## Unified error handling
As mentioned in the beginning of the post the error handling has been centralised - see below the code snippet dealing
with MongoDB Errors:

```javascript

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
```

## Conclusion
> Note how the code has become shorter, easier to read (especially for someone like me, with a Java/JavaEE background) and the error handling is now much clearer.

Another cool feature of `async/await` is how to easily implement multiple parallel, but that in a coming post...

{% include action-to-star-bookmarksdev-on-github.html %}
