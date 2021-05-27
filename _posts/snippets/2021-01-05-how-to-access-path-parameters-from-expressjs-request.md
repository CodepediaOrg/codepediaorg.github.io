---
layout: post
title: How to access path parameters from ExpressJS request
description: "Code snippets showing to access single and multiple path parameters from ExpressJS request. Tagged with javascript, expressjs, node.js"
author: ama
permalink: /snippets/5feed4910b79fd210e120c4d/how-to-access-path-parameters-from-expressjs-request
published: true
snippetId: 5feed4910b79fd210e120c4d
categories: [snippets]
tags: [javascript, expressjs, node.js]
---

Select one parameter

```javascript
router.get('/:userId/profile', async (request, response) => {
  const userId = request.params.userId;
 // ....
});
```

<hr/>

Select multiple path parameters

```javascript
personalBookmarksRouter.get('/:bookmarkId', keycloak.protect(), async (request, response) => {
  const {userId, bookmarkId} = request.params;
  // ...
});
```

<span style="font-size: 0.9rem">
    <strong>Reference - </strong>
    <a href="https://github.com/codeverland/codever" target="_blank" style="font-weight: lighter">
        https://github.com/codeverland/codever
    </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="5feed4910b79fd210e120c4d" %}



