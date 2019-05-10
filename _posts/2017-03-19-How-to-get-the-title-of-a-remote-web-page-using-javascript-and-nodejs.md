---
layout: post
title: How to get the title of a remote web page using javascript and NodeJS
description: "This post presents how to use web scraping with Cheerio in a NodeJS backend to retrieve the title and the meta description of a bookmark added in www.codingmarks.org"
author: ama
permalink: /ama/how-to-get-the-title-of-a-remote-web-page-using-javascript-and-nodejs
published: true
categories: [nodejs, javascript]
tags: [codingmarks, coding bookmarks, web scraping, cheerio, angular]
---

When I [add a new bookmark](https://youtu.be/bj22xbE5ZiY?t=5m10s) to my bookmarks collection, I set the title of the new bookmark, most of the time, the same as the title of the web page being bookmark -
 I assume the authors have put some thought into it. To make this happen automatically, I am using a technique called web scraping[^1].  I cannot do it in front end (angular), since most of the URLs are outside
   of the [https://www.codingmarks.org/](https://www.codingmarks.org/) domain. So the magic happens in back end, in NodeJS, with the help of a library called cheerio[^2], thank you [Matthew](https://github.com/matthewmueller) for that.
   Cheerio is a fast, flexible, and lean implementation of core jQuery[^3] designed specifically for the server. Read on to learn how this works.

[^1]: <https://en.wikipedia.org/wiki/Web_scraping>
[^2]: <https://cheerio.js.org/>
[^3]: <https://jquery.com/>

{% include source-code-codingmarks.html %}

<!--more-->

### Check the dependencies

In the _package.json_ make sure you have the `cheerio` and `request` dependencies:

```{
     "name": "bookmarks-api.codingpedia.org",
     "version": "1.0.0",
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
       "keycloak-connect": "2.5.0",
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

> Request[^4] is a simplified HTTP request client for NodeJS. It is designed to be the simplest way possible to make http calls.

[^4]: <https://www.npmjs.com/package/request>

## Implementation

```js
var express = require('express');
var request = require('request');
var cheerio = require('cheerio');
var router = express.Router();
var Bookmark = require('../models/bookmark');
var MyError = require('../models/error');

router.get('/scrape', function(req, res, next) {
  if(req.query.url){
    request(req.query.url, function (error, response, body) {
      if (!error && response.statusCode == 200) {
        const $ = cheerio.load(body);
        const webpageTitle = $("title").text();
        const metaDescription =  $('meta[name=description]').attr("content");
        const webpage = {
          title: webpageTitle,
          metaDescription: metaDescription
        }
        res.send(webpage);
      }
    });
  }
});
```

### What happens?

The URL of the webpage comes as a `url` query parameter under the `/scrape resource. Then the web page with the URL is requested.
If there is no error and we receive an <span class="highlight-yellow">HTTP 200 OK status</span> the body of the response is loaded into cheerio.

> With Cheerio we need to pass in the HTML document.

Once loaded we call the  `text()`[^5] function to get the content of the `title` element and also identify the meta description content. 
We return them both in a custom `webpage` object. If I am not satisfied with the title or the description I can edit it manually after.

[^5]: <http://api.jquery.com/text/>


If you found this useful, please star it, share it and improve it:

{% include source-code-codingmarks.html %}

> A note on web scraping from Wikipedia - "<span class="highlight-yellow">the legality of web scraping varies across the world. In general, web scraping may be against the terms of use of some websites, but the enforceability of these terms is unclear</span>"[^6].
 Since I am using the method to get the title for bookmarking and then reference back the link, Ì think I don't do anything illegal, but be wary...

[^6]: <https://en.wikipedia.org/wiki/Web_scraping#Legal_issues>

## References
