---
layout: post
title: Set field of all documents with given value only if not existing in Mongo
description: "Set field of all documents with given value only if not existing in Mongo code snippet"
author: ama
permalink: /snippets/60e14b212237f17a9845767f/set-field-of-all-documents-with-given-value-only-if-not-existing-in-mongo
published: true
snippetId: 60e14b212237f17a9845767f
categories: [snippets]
tags: [mongodb, script, codever-snippets]
---

Use  `update` method with the `multi` flag set to `true`

```javascript
db.users.update(
  { "welcomeAck": { "$exists": false } },
  { "$set": { "welcomeAck": true } },
  { "multi": true }
);

```

Or the equivalent shortcut with `updateMany`:

```javascript
db.users.updateMany(
  { "welcomeAck": { "$exists": false } },
  { "$set": { "welcomeAck": true } }
);
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.mongodb.com/manual/tutorial/update-documents/" target="_blank" style="font-weight: lighter">
     https://docs.mongodb.com/manual/tutorial/update-documents/
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60e14b212237f17a9845767f" %}
