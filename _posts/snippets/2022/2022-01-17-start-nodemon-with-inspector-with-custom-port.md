---
layout: post
title: Start nodemon with inspector with custom port
description: "Start nodemon with inspector with custom port code snippet"
author: ama
permalink: /snippets/60621d22967b5a363dfa8fb6/start-nodemon-with-inspector-with-custom-port
published: true
snippetId: 60621d22967b5a363dfa8fb6
categories: [snippets]
tags: [nodemon, node.js, codever-snippets]
---

Enable the inspector by using `--inspect` option and giving it a **port number** to listen on as in the following example
(here **9230**)

```json
  "scripts" : {
    "start": "nodemon  --inspect=9230 ./bin/www"
  }
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://nodejs.org/en/docs/guides/debugging-getting-started/" target="_blank" style="font-weight: lighter">
     https://nodejs.org/en/docs/guides/debugging-getting-started/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60621d22967b5a363dfa8fb6" %}
