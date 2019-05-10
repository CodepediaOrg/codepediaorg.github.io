---
layout: post
title: Cleaner code in NodeJs with async-await - Mongoose calls example
description: "Example showing migration of Mongoose calls from previously using callbacks to using the new
async-await feature in NodeJs"
author: ama
permalink: /ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example
published: true
categories: [javascript, nodejs]
tags: [javascript, nodejs, mongoose, async-await, codingmarks]
---

A lot has been written already about the transition from [callbacks](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) 
to [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) and now to the new 
`async/await`[^1] feature in ES7.

<p class="note_normal">
  "The purpose of `async/await` functions is to simplify the behavior of using promises synchronously and to perform some behavior on a group of `Promises`.
   Just as `Promises` are similar to structured callbacks, `async/await is similar to combining generators and promises."[^1]
</p>

[^1]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function>

In this blog post I present what this code "upgrade" meant for [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
   performed on [codingmarks](https://www.codingmarks.org). I use [Moongoose](http://mongoosejs.com/) in an ExpressJS/NodeJS backend
    to perform the operations against a MongoDB database. 
     
<!--more-->

* TOC
{:toc}     

## Create

### Before
```
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

{% highlight javascript %}
router.post('/:id/bookmarks', keycloak.protect(), async (req, res) => {
  const descriptionHtml = req.body.descriptionHtml ? req.body.descriptionHtml: converter.makeHtml(req.body.description);

  const bookmark = new Bookmark({
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

  try{
    let newBookmark = await bookmark.save();

    res.set('Location', 'http://localhost:3000/' + req.params.id + '/bookmarks/' + newBookmark.id);
    res.status(201).send({response:'Bookmark created for userId ' + req.params.id});

  } catch (err){
    if (err.name === 'MongoError' && err.code === 11000) {
      res.status(409).send(new MyError('Duplicate key', [err.message]));
    }

    res.status(500).send(err);
  }

});
{% endhighlight %}

## Read

### Before
```
/* GET bookmarks for user */
router.get('/:id/bookmarks', keycloak.protect(), function(req, res, next) {
  if(req.query.term){
    var regExpTerm = new RegExp(req.query.term, 'i');
    var regExpSearch=[{name:{$regex:regExpTerm}}, {description:{$regex: regExpTerm }}, {category:{$regex:regExpTerm }}, {tags:{$regex:regExpTerm}}];
    Bookmark.find({userId:req.params.id, '$or':regExpSearch}, function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmarks);
    });
  } else {//no filter - all bookmarks
    Bookmark.find({userId:req.params.id}, function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmarks);
    });
  }

});
```

### After

{% highlight javascript %}
/* GET bookmarks for user */
router.get('/:id/bookmarks', keycloak.protect(), async (req, res) => {
  try{
    let bookmarks;
    if(req.query.term){
      var regExpTerm = new RegExp(req.query.term, 'i');
      var regExpSearch=[{name:{$regex:regExpTerm}}, {description:{$regex: regExpTerm }}, {category:{$regex:regExpTerm }}, {tags:{$regex:regExpTerm}}];
      bookmarks = await Bookmark.find({userId:req.params.id, '$or':regExpSearch});
    } else {//no filter - all bookmarks
      bookmarks = await Bookmark.find({userId:req.params.id});
    }

    res.send(bookmarks);
  } catch (err) {
    return res.status(500).send(err);
  }
});
{% endhighlight %}

## Update

### Before
```
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

{% highlight javascript %}
/**
 * full UPDATE via PUT - that is the whole document is required and will be updated
 * the descriptionHtml parameter is only set in backend, if only does not come front-end (might be an API call)
 */
router.put('/:userId/bookmarks/:bookmarkId', keycloak.protect(), async (req, res) => {
  if(!req.body.descriptionHtml){
    req.body.descriptionHtml = converter.makeHtml(req.body.description);
  }
  try {
    let bookmark = await Bookmark.findOneAndUpdate({_id: req.params.bookmarkId, userId: req.params.userId}, req.body, {new: true});

    if (!bookmark) {
      return res.status(404).send(new MyError('Not Found Error', ['Bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId + ' not found']));
    } else {
      res.status(200).send(bookmark);
    }
  } catch (err) {
    if (err.name === 'MongoError' && err.code === 11000) {
      res.status(409).send(new MyError('Duplicate key', [err.message]));
    }
    res.status(500).send(new MyError('Unknown Server Error', ['Unknow server error when updating bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId]));
  }
});
{% endhighlight %}

## Delete

### Before
```
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

{% highlight javascript %}
/*
* DELETE bookmark for user`
*/
router.delete('/:userId/bookmarks/:bookmarkId', keycloak.protect(), async (req, res) => {
  try {
    let bookmark = await Bookmark.findOneAndRemove({_id: req.params.bookmarkId, userId: req.params.userId});

    if (!bookmark) {
      return res.status(404).send(new MyError('Not Found Error', ['Bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId + ' not found']));
    } else {
      res.status(204).send('Bookmark successfully deleted');
    }
  } catch (err) {
    return res.status(500).send(new MyError('Unknown server error', ['Unknown server error when trying to delete bookmark with id ' + req.params.bookmarkId]));
  }
});
{% endhighlight %}

> Note how the code has become shorter, easier to read (especially for someone like me, with a Java/JavaEE background) and the error handling is now much clearer.
   
Another cool feature of `async/await` is how to easily implement multiple parallel, but that in a coming post...     
   
{% include source-code-codingmarks.html %}

