---
layout: post
title: Example on how to call the YouTube Data API from Node.js
description: "Shows an example with how to call the YouTube API to get metadata about videos. The client calling the
 API is a Node.js/Express.js backend. Source code is also available."
author: ama
permalink: /ama/how-to-call-youtube-api-from-nodejs-example
published: false
categories: [bookmarks]
tags: [expressjs, nodejs, rest, angular, youtube, api]
---

In this short blog post I will show you how to call the Youtube API v3 to get information details about videos. I am especially
interested in the **video duration**, **description**, **title**, **tags**  and **publication date**. These is the 
metadata to [auto-magically bookmark youtube videos for developers](TODO - link to dev.to post ). 

<!--more-->

## Let's dive directly into how it's done

I use [superagent](https://visionmedia.github.io/superagent/) to make the call from the backend. 

```javascript
let getYoutubeVideoData = async (youtubeVideoId) => {
  const response = await request
    .get('https://www.googleapis.com/youtube/v3/videos')
    .query({id: youtubeVideoId})
    .query({key: process.env.YOUTUBE_API_KEY || "change-me-with-a-valid-youtube-key-if-you-need-me"}) //used only when adding youtube videos
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

What are the required parameters?
* **videoId** - 
* **key** - you need an API key to access the Youtube API. You can create one from the [Google Developers Console](https://console.developers.google.com/). 
Make sure you have the YouTube Data API v3 (in my case) for the selected API; here the key is read from the environment variables
* you can query only the parts you are interested in by specifying them in the ``part`` query parameter as shown in the code snippet above

With the data I get I fill the details I need for front-end.


Youtube API call
https://www.googleapis.com/youtube/v3/videos?id=Ke90Tje7VS0&key=CHANGE_ME&part=snippet,contentDetails,statistics,status
