---
layout: post
title: How to use Showdown in Angular and NodeJS
description: "This post shows how to dynamically fill in data in a reactive form field, based on other field's data"
author: ama
permalink: /ama/how-to-use-shodown-in-angular-and-nodejs
published: true
categories: [angular, nodejs]
tags: [codingmarks, coding bookmarks, angular, nodejs, showdown, markdown]
---

When managing my coding bookmarks via [www.codingmarks.org](https://www.codingmarks.org/), I often have the need to place in the bookmark's description either a code snippet (might be a command), add a list or add links and emphasize words - this seems like a perfect match for Markdown[^1]. In this post I will demonstrate how I enabled Markdown support with the help of ShodownJS[^2], both in front end (developed with Angular[^3]) and in backend, developed with NodeJS[^4].

[^1]: <https://daringfireball.net/projects/markdown/>
[^2]: <https://github.com/showdownjs/showdown>
[^3]: <https://angular.io/>
[^4]: <https://nodejs.org/en/>

{% include source-code-codingmarks.html %}

<!--more-->

## What is Showdown?
So what it Showdown? Well, according to the its authors "showdown is a Javascript Markdown to HTML converter, based on the original works by John Gruber. Showdown can be used client side (in the browser) or server side (with NodeJs)."[^1]

## How to use Showdown in Angular?

I use showdown in front end everytime I create a new bookmark or I update one. The notes written in Markdown are transformed in html and persisted in the database. I thought it would be more performant than just to persist the markdown code
 and generate html on the fly.

### Showdown dependencies

First thing is to add the proper dependencies `@types/showdown` and `showdown` in `package.json`:

```js
  "dependencies": {
    "@angular/common": "2.4.2",
    "@angular/compiler": "2.4.2",
    "@angular/core": "2.4.2",
    "@angular/forms": "2.4.2",
    "@angular/http": "2.4.2",
    "@angular/platform-browser": "2.4.2",
    "@angular/platform-browser-dynamic": "2.4.2",
    "@angular/router": "3.4.2",
    "@types/showdown": "^1.4.31",
    "bootstrap": "^4.0.0-alpha.5",
    "core-js": "^2.4.1",
    "immutable": "^3.7.6",
    "keycloak-js": "2.3.0",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.1",
    "showdown": "^1.6.4",
    "zone.js": "^0.7.2"
  },
```

Then  and `npm install` and you're ready to go.

### Code setup

I am building the application with Webpack 2.x[^5], so I need to import `showdown/dist/showdown.js` in the _vendor.ts_ file:

[^5]: <https://webpack.github.io/>

```typescript
// Angular 2
import '@angular/platform-browser';
import '@angular/platform-browser-dynamic';
import '@angular/core';
import '@angular/common';
import '@angular/http';
import '@angular/router';

import 'rxjs';
import '@angularclass/hmr';

// Other vendors for example jQuery, Lodash or Bootstrap
// You can import js, ts, css, sass, ...
import 'bootstrap/dist/css/bootstrap.css';
import "keycloak-js/dist/keycloak.js";
import "showdown/dist/showdown.js";
```

Now the library is ready to be used in code. First require the `showdown` module, then create a  showdown `converter`

```typescript
import {Component, OnInit} from "@angular/core";
import {Bookmark} from "../../model/bookmark";
import {FormGroup, FormBuilder, Validators} from "@angular/forms";
import {KeycloakService} from "../../keycloak/keycloak.service";
import {UserBookmarkStore} from "../../personal/store/UserBookmarkStore";
import {Router} from "@angular/router";
import {BookmarkService} from "../../bookmark/bookmark.service";

const showdown = require('showdown');
const converter = new showdown.Converter();
```

and finally call the `makeHthml` method on the `converter`, before the bookmark is created and persisted:

```typescript
  saveBookmark(model: Bookmark) {
    model.tags = model.tagsLine.split(",");
    var newBookmark = new Bookmark(model.name, model.location, model.category,model.tagsLine.split(","), model.description, null);

    newBookmark.userId = this.userId;
    newBookmark.shared = model.shared;

    newBookmark.descriptionHtml = converter.makeHtml(newBookmark.description);

    let obs = this.userBookmarkStore.addBookmark(this.userId, newBookmark);

    obs.subscribe(
      res => {
        console.log(res);
        this.router.navigate(['/personal']);
      });
  }
```

> I use the same approach when I update a bookmark. Just generate new html from the description and persist it.

## How to use Showdown in NodeJS?

At this point, there isn't much of a need to use showdown in the back-end, but I kept it for two reasons:

1. there might be cases when the descriptionHtml not present is, like API calls
2. I get to write about how to do it in this post, maybe I'll still need it later

Similar to front-end, add the `showdown` dependency in `package.json` and then `npm install` it:

```javascript
{
  "name": "bookmarks-api.codingpedia.org",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.15.1",
    "cheerio": "latest",
    "cookie-parser": "~1.4.3",
    "debug": "~2.2.0",
    "express": "~4.13.4",
    "jade": "~1.11.0",
    "keycloak-connect": "^2.5.0",
    "mongoose": "~4.3.7",
    "mongoose-unique-validator": "1.0.2",
    "morgan": "~1.7.0",
    "request": "latest",
    "serve-favicon": "~2.3.0",
    "showdown": "^1.6.4"
  },
  "devDependencies": {
    "nodemon": "^1.10.2"
  }
}
```

Then require the `showdown` module and create a showdown `converter`:

```javascript
var showdown  = require('showdown'),
  converter = new showdown.Converter();
```

and finally use the `makeHtml` method of the  `converter` in the `post` and `put` routes:

```javascript
router.post('/:id/bookmarks', keycloak.protect(), function(req, res, next){
  var descriptionHtml = req.body.descriptionHtml ? req.body.descriptionHtml: converter.makeHtml(req.body.description);

  .............

});

...............

router.put('/:userId/bookmarks/:bookmarkId', keycloak.protect(), function(req, res, next) {
  if(!req.body.descriptionHtml){
    req.body.descriptionHtml = converter.makeHtml(req.body.description);
  }

  Bookmark.findOneAndUpdate({_id: req.params.bookmarkId, userId: req.params.userId}, req.body, {new: true}, function(err, bookmark){
  .............
  });

});
```

Works like a charm.

If you found this useful, please star it, share it and improve it:

{% include source-code-codingmarks.html %}

## References
