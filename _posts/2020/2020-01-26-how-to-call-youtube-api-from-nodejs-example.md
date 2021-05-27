---
layout: post
title: Example on how to call the YouTube Data API from Node.js
description: "Shows an example on how you can call the YouTube Data API to get video details. The client calling the
 API is a Node.js/Express.js backend, where superagent is used. Source code is available on Github."
author: ama
permalink: /ama/how-to-call-youtube-api-from-nodejs-example
published: true
categories: [tutorial]
tags:
    - expressjs
    - node.js
    - rest
    - angular
    - youtube
    - api
    - bookmarks.dev
---

In this short blog post I will show you how to call the Youtube Data API v3 to get information about youtube videos.
 I am especially interested in the **video duration**, **description**, **title**, **tags** and **publication date** attributes.
  This is the metadata I use to [automagically bookmark youtube videos](https://dev.to/ama/automagically-bookmarking-youtube-videos-for-developers-lpn).

> Source code for this article can be found on [Github](https://github.com/codeverland/codever)

<!--more-->

## Let's dive directly into how it's done

You need to make a GET REST call. In my case the call is made from a  [Node.js/ExpressJS backend](https://github.com/codeverland/codever).
 I use [superagent](https://visionmedia.github.io/superagent/) to make the call, but you can use a REST client of your
 choice:

```javascript
let getYoutubeVideoData = async (youtubeVideoId) => {
  const response = await request
    .get('https://www.googleapis.com/youtube/v3/videos')
    .query({id: youtubeVideoId})
    .query({key: process.env.YOUTUBE_API_KEY || "change-me-with-a-valid-youtube-key-if-you-need-me"}) //used only when saving youtube videos
    .query({part: 'snippet,contentDetails,statistics,status'});

  let title = response.body.items[0].snippet.title;
  const tags = response.body.items[0].snippet.tags;
  const description = response.body.items[0].snippet.description;
  const publishedAt = response.body.items[0].snippet.publishedAt;
  const publishedOn = publishedAt.substring(0, publishedAt.indexOf('T'));
  const videoDuration = formatDuration(response.body.items[0].contentDetails.duration);
  if (title.endsWith('- YouTube')) {
    title = title.replace('- YouTube', ' - ' + videoDuration);
  } else {
    title = title + ' - ' + videoDuration;
  }

  let webpageData = {
    title: title,
    metaDescription: description.substring(0, 500),
    publishedOn: publishedOn,
    videoDuration: videoDuration
  }

  if(tags) { //some youtube videos might not have tags defined
    webpageData.tags = tags.slice(0,8).map(tag => tag.trim().replace(/\s+/g, '-'));
  }

  return webpageData;
}
```

What are the used parameters?
* `videoId` - this is the `id` of the video; it's the value you find the `v` query parameter of a youtube link
 - e.g. [https://www.youtube.com/watch?v=**4q12HDjWY44**](https://www.youtube.com/watch?v=4q12HDjWY44)
* `key` - you need an API key to access the Youtube Data API. You can create one from the [Google Developers Console](https://console.developers.google.com/).
Make sure you select the YouTube Data API (v3 in my case); in the code example above the value of the key is read from the environment variables
* you can query only the parts you are interested in, by specifying them in the `part` query parameter as shown in the code snippet above

I use the response data to make it easy to bookmark youtube videos. In the video below you can see it in action:

<iframe width="560" height="315" src="https://www.youtube.com/embed/4q12HDjWY44" frameborder="0" allowfullscreen></iframe>

> Note the [Save to Bookmarks.dev Chrome Extension](https://chrome.google.com/webstore/detail/save-url-to-bookmarksdev/diofdblfhjbpgackifolmboaiccmebjb)
 at the beginning of the video, which I developed and [open-sourced](https://github.com/codeverland/codever-chrome-extension) to make it easy to save bookmarks.

### [Quota usage](https://developers.google.com/youtube/v3/getting-started#quota)
The YouTube Data API uses a quota. All API requests, including invalid requests, incur at least a one-point quota cost.
 You can find the quota available to your application in the API Console.

Projects that enable the YouTube Data API have a default quota allocation of 10 thousand units per day.

