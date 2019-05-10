---
layout: post
title: Angular2 modules lazy loading with Webpack
description: "This post describes how to lazy load NgModules with Webpack"
author: ama
permalink: /ama/lazy-loading-ngmodules-in-angular2-with-webpack
published: false
categories: [nodejs]
tags: [codingpedia bookmarks, angular, angular2, webpack, bugfix]
---



```
"devDependencies": {
    ......
    "angular2-router-loader": "^0.3.4",
    ......
}
```

https://www.npmjs.com/package/angular2-router-loader

This happens in the preflight request[^1]
[^1]: <https://fetch.spec.whatwg.org/#http-cors-protocol>


Add in webpack.config.js:

```javascript
config.module = {
    rules: [
      // Support for .ts files.
      {
        test: /\.ts$/,
        loaders: ['awesome-typescript-loader?' + atlOptions, 'angular2-template-loader', '@angularclass/hmr-loader', 'angular2-router-loader'],
        exclude: [isTest ? /\.(e2e)\.ts$/ : /\.(spec|e2e)\.ts$/, /node_modules\/(?!(ng2-.+))/]
      },
      ......
    ]
}
```

TODO - add some webpack stuff here and angular 2 configuration
Link to NgModules, some explanation and so on....


## References
