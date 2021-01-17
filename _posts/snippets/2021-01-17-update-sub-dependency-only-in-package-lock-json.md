---
layout: post
title: Update (sub)dependency only in package-lock.json
description: "Code snippet showing how to update dependency only in package-lock.json"
author: ama
permalink: /snippets/5fd0f6ee6fd7966de9f12933/update-sub-dependency-only-in-package-lock-json
published: true
snippetId: 5fd0f6ee6fd7966de9f12933
categories: [snippets]
tags: [npm, dependencies, dependency-management]
---

You might need to update a dependency only in your `package-lock.json` file,  because there might be a security vulnerability for the version you use but you can't update the dependency in your `package.json` that uses that dependency. You can use `--no-save` and `--package-lock-only` options as in the example below

```shell
npm install y18n@4.0.1 --no-save --package-lock-only
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.npmjs.com/cli/v6/configuring-npm/package-locks" target="_blank" style="font-weight: lighter">
     https://docs.npmjs.com/cli/v6/configuring-npm/package-locks
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fd0f6ee6fd7966de9f12933" %}
