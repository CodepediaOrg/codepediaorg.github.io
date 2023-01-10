---
layout: post
title: Access value from form data from request parameter in express js
description: "Access value from form data from request parameter in express js code snippet"
author: ama
permalink: /snippets/609ec1b0d89230173165956f/access-value-from-form-data-from-request-parameter-in-express-js
published: true
snippetId: 609ec1b0d89230173165956f
categories: [snippets]
tags: [expressjs, forms, javascript, node.js, codever-snippets]
---

**Project**: [`Codever`](https://github.com/CodeverDotDev/codever) - **File**:  `user.router.js`
The values are present in the `request.body` which contains key-value pairs of data submitted in the request body.
In our case we access the `userDisplayName` with the following expression `request.body.userDisplayName` as in the example below:

```javascript
usersRouter.post('/:userId/bookmarks/upload', keycloak.protect(),
  uploadBookmarks.single("bookmarks" /* name attribute of <file> element in your form */),
  async (request, response) => {
    userIdTokenValidator.validateUserId(request);

    const userDisplayName = request.body.userDisplayName;
    const importResponse = await browserBookmarksImportService.imporBrowserBookmarks(request.params.userId, request.file.buffer, userDisplayName);

    const str = JSON.stringify(importResponse, null, 2); // spacing level = 2
    console.log(str);

    return response.status(HttpStatus.OK).send(importResponse);
  }
);
```

In angular the `userDisplayName` value has been appended to the `FormData` and submitted to a post request
via the Angular Http Client:


```javascript
  uploadBookmarks(userId: String, bookmarks: File, userDisplayName: string): Observable<any> {
    const formData = new FormData();
    formData.append('bookmarks', bookmarks);
    formData.append('userDisplayName', userDisplayName);

    return this.httpClient.post(`${this.usersApiBaseUrl}/${userId}/bookmarks/upload`, formData);
  }
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://expressjs.com/en/api.html#req.body" target="_blank" style="font-weight: lighter">
     https://expressjs.com/en/api.html#req.body
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="609ec1b0d89230173165956f" %}
