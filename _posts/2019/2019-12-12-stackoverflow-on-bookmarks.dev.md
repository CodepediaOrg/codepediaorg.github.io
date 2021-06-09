---
layout: post
title: Improved bookmarking of StackOverflow questions
description: "New improvements for StackOverflow questions on bookmarks.dev - auto completion of bookmark attributes,
login with stackoverflow account and visual hinting for StackOverflow questions"
author: ama
permalink: /ama/improved-bookmarking-of-stackoverflow-questions
published: true
categories: [article]
tags:
    - codever
    - expressjs
    - node.js
    - angular
    - api
---

I found myself lately adding more StackOverflow questions to [my dev bookmarks collection](https://www.codever.land),
so I took the challenge over the weekend to make this experience more pleasant. In this blog post I will present you
the result - improve auto completion of bookmark attributes on creation, login with Stackoverflow account and visual hinting
for stackoverflow bookmarks.

<!--more-->


## Improve auto completion of stackoverflow bookmarks
The [existing scrapper](https://www.codepedia.org/ama/how-to-get-the-title-of-a-remote-web-page-using-javascript-and-nodejs) adds the
title of stackoverflow questions, but more was certainly possible. Indeed by using the [stackexchange api](https://api.stackexchange.com/docs)
I was able to automatically add extra the tags and the creation date of the question. Unfortunately the correct answer is not made
available via the API, or I did not find out how yet. So let's see how that works.

### Backend

In backend the [changes](https://github.com/codeverland/codever/commit/b0fe37c522998f4e8a7d79352b24299c95875181) are minimal.
Chedck if the `scrape` path contains a `stackoverflowQuestionId` query param and then we invoke the api with a key registered on [stackapps](https://stackapps.com/apps/oauth/register)
to get the data

#### Router

```javascript
/* GET stackoverflow question data */
router.get('/scrape', async function (request, response, next) {
  const stackoverflowQuestionId = request.query.stackoverflowQuestionId;
  if (stackoverflowQuestionId) {
    const webpageData = await PublicBookmarksService.getStackoverflowQuestionData(stackoverflowQuestionId)

    return response.send(webpageData);
  } else {
    next();
  }
});
```

#### Service

I use [superagent](https://github.com/visionmedia/superagent) to make the REST api call

```javascript
let getStackoverflowQuestionData = async (stackoverflowQuestionId) => {
  const response = await request
    .get(`https://api.stackexchange.com/2.2/questions/${stackoverflowQuestionId}`)
    .query({site: 'stackoverflow'})
    .query({key: process.env.STACK_EXCHANGE_API_KEY || "change-me-with-a-valid-stackexchange-key-if-you-need-me"});


  const tags = response.body.items[0].tags;
  const title = response.body.items[0].title;
  const creationDateMillis = response.body.items[0].creation_date * 1000;
  const creationDate = new Date(creationDateMillis).toISOString();
  const publishedOn = creationDate.substring(0, creationDate.indexOf('T'));

  const webpageData = {
    title: title,
    tags: tags,
    publishedOn: publishedOn
  }

  return webpageData;
}
```

> With a `key` your rate is currently limited to 10000 calls per day, otherwise to just 300

### Front-end
In front-end a little bit [more work](https://github.com/codeverland/codever/commit/22dae2dd163d81ade388c8602b386a79e97f1a7b) was needed,
since some refactoring was involved.

```typescript
  private getScrapeData(location) {
    this.personalBookmarkPresent = false;
    const youtubeVideoId = this.getYoutubeVideoId(location);
    if (youtubeVideoId) {
      this.bookmarkForm.get('youtubeVideoId').patchValue(youtubeVideoId, {emitEvent: false});
      this.publicBookmarksService.getYoutubeVideoData(youtubeVideoId).subscribe((webpageData: WebpageData) => {
          this.patchFormAttributesWithScrapedData(webpageData);
        },
        error => {
          console.error(`Problems when scraping data for youtube id ${youtubeVideoId}`, error);
          this.updateFormWithScrapingDataFromLocation(location);
        });
    } else {
      const stackoverflowQuestionId = this.getStackoverflowQuestionId(location);
      if (stackoverflowQuestionId) {
        this.bookmarkForm.get('stackoverflowQuestionId').patchValue(stackoverflowQuestionId, {emitEvent: false});
        this.publicBookmarksService.getStackoverflowQuestionData(stackoverflowQuestionId).subscribe((webpageData: WebpageData) => {
            this.patchFormAttributesWithScrapedData(webpageData);
          },
          error => {
            console.error(`Problems when scraping data for stackoverflow id ${stackoverflowQuestionId}`, error);
            this.updateFormWithScrapingDataFromLocation(location);
          });
      } else {
        this.updateFormWithScrapingDataFromLocation(location);
      }
    }
  }


  private getStackoverflowQuestionId(location: string) {
    let stackoverflowQuestionId = null;
    const regExpMatchArray = location.match(/stackoverflow\.com\/questions\/(\d+)/);
    if (regExpMatchArray) {
      stackoverflowQuestionId = regExpMatchArray[1];
    }

    return stackoverflowQuestionId;
  }

  private patchFormAttributesWithScrapedData(webpageData) {
    if (webpageData.title) {
      this.bookmarkForm.get('name').patchValue(webpageData.title, {emitEvent: false});
    }
    if (webpageData.publishedOn) {
      this.bookmarkForm.get('publishedOn').patchValue(webpageData.publishedOn, {emitEvent: false});
    }
    if (webpageData.metaDescription) {
      this.bookmarkForm.get('description').patchValue(webpageData.metaDescription, {emitEvent: false});
    }
    if (webpageData.tags) {
      for (let i = 0; i < webpageData.tags.length; i++) {
        const formTags = this.bookmarkForm.get('tags') as FormArray;
        formTags.push(this.formBuilder.control(webpageData.tags[i]));
      }

      this.tagsControl.setValue(null);
      this.tags.markAsDirty();
    }
  }
```

If it is recognised it involves a stackoverflow question via the `getStackoverflowQuestionId(location: string)` method.
The backend api is then called to receive the question metadata.

The API invoking part:
```typescript
  getStackoverflowQuestionData(stackoverflowQuestionId: string) {
    const params = new HttpParams()
      .set('stackoverflowQuestionId', stackoverflowQuestionId)
    return this.httpClient
      .get<WebpageData>(`${this.publicBookmarksApiBaseUrl}/scrape`, {params: params});
  }
```

With this data the title, tags and creation date autocomplete for you:

   ![Login with stackoverflow](/images/posts/stackoverflow-on-bookmarks.dev/autocompleted-attributes.png)

## Visual hints
Once the bookmark is submitted a stackoverflow "hint" is place in front of the resource:

   ![Stackoverflow hint](/images/posts/stackoverflow-on-bookmarks.dev/stackoverflow-hint.png)

## Single Sign-On (SSO) with Stackoverflow Account
Since I am using Keycloak as Identity provider, it was also pretty easy to setup the stackoverflow login
with it. See the [official documentation](https://www.keycloak.org/docs/latest/server_admin/index.html#stack-overflow) needed for setup.
Now you can use your Stackoverflow account to Login and register on [www.codever.land](https://www.codever.land):

 ![Login with stackoverflow](/images/posts/stackoverflow-on-bookmarks.dev/login-with-stackoverflow.png)

## Conclusion
I hope these new additions will make managing your dev bookmarks a little better. If you have other improvements ideas
please submit them on [Github](https://github.com/codeverland/codever/issues)
