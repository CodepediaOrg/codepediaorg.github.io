---
layout: post
title: Fast, faster, Angular CLI - how I converted my Angular project to use Angular CLI
description: "Main steps to migrate a regular Angular project built with Webpack to a Angular CLI project.
My test candidate was the codingmarks.org project "
author: ama
permalink: /ama/fast-faster-angular-cli-how-i-converted-my-angular-project-to-use-angular-cli
published: true
categories: [javascript]
tags: [angular, angular-cli, codingmarks, webpack, bootstrap, font-awesome, showdown]
---

Last week I migrated the [codingmarks](http://codingmarks.org) project to use [Angular CLI](https://cli.angular.io/) and it's awesome. Initially it was based on Angular Seed with Webpack - [preboot/angular-webpack](https://github.com/preboot/angular-webpack).
  I couldn't take the update breaking changes pain anymore when I wanted to add  Ahead-Of-Time compilation[^1] support. My webpack[^2] configuration was also getting out of hand. But I did it, I moved it to Angular CLI and in this post I am going to list the major steps I had to take. It is based on the [Moving your project to Angular CLI story](https://github.com/angular/angular-cli/wiki/stories-moving-into-the-cli) with my own needs and flavor added to it.

[^1]: <https://angular.io/docs/ts/latest/cookbook/aot-compiler.html>
[^2]: <https://webpack.js.org/guides/get-started/>


<!--more-->

* TOC
{:toc}

## Conversion setup

The easiest way to move an existing project to Angular CLI is to copy your application files into a new, empty CLI project.

I started with preparing my existing project folder - `bookmarks`.

* commit and push existing changes.
* clean the folder from temporary files and ignored files using `git clean -fdx`.
* renamed the project folder to `old-bookmarks`.

> Before you run `git clean -fdx`, perform a `git clean -fndx` "dry run" (`-n`). This will show you which files are going to be removed without actually doing it.

Second step is to make a new project on the same parent folder as `old-bookmarks` using Angular CLI.

* Verifiy the [Angular CLI prerequisites](https://github.com/angular/angular-cli#prerequisites).
* Install the CLI globally: `npm install -g @angular/cli`.
* Make a new app: `ng new bookmarks --style=scss`.
* Move into the folder: `cd bookmarks`.
* Test the app is working: `ng serve --open`.

> I am using the Sassy CSS (SCSS)[^3] for the project. You can advise Angular CLI to generate a project with this style
by specifying the `--style=scss` option.

[^3]: <http://sass-lang.com/documentation/file.SASS_REFERENCE.html#syntax>

Copy over your app files.

* Remove the existing app: `rm -rf src/app src/styles.css src/index.html e2e`.
* Copy `src/app/`, `src/index.html` from the old app. Move the app styles from `src/app/app.scss`  to `src/styles.scss`
* Copy over any other files hte app needs like images into `src/assets.` Adjust paths on the app to use this folder e.g. `<img src='assets/my-image.jpg>`.

### webpack.config.js

Gone. Under the hood the whole thing still works via Webpack, but the configuration is done now via the _.angular-cli.json file_.
 I find this more readable and decently explained - [Angular CLI Config Schema](https://github.com/angular/angular-cli/wiki/angular-cli).

### package.json

Compare the `old-bookmarks/package.json` to the new `./package.json`. Add in third party libraries, the `@types/*` packages, and any other types. In my case you can see clearly what happened in this [commit](https://github.com/Codingpedia/bookmarks/commit/e7edb064483927618b182cfc5054efb913ed205f#diff-b9cfc7f2cdf78a7f4b91a753d10865a2)

Run `npm install` to install the added packages.

### main.ts

For me it meant replacing the condition to `enableProdMode()` from `if (process.env.ENV === 'build')` to `if (environment.production)`. See this [commit](https://github.com/Codingpedia/bookmarks/commit/e7edb064483927618b182cfc5054efb913ed205f#diff-8cfead41d88ad47d44509a8ab0a109ad) for a complete comparison.

### Unit Testing

No extra work done. Works automatically.

### Commit changes
The final step is to copy your git history so you can continue working without losing anything:

Copy over the git folder: `cp -r ../old-bookmarks/.git .git`
Commit and push the changes as normal.

## Additional steps

There are stille some things I had to reconfigure, like making bootstrap, font awesome and showdown work as before.

### Migrate bootstrap[^4]

[^4]: <https://v4-alpha.getbootstrap.com/>

> As I mentioned before I am using SASS and version [4 alpha of Boostrap](https://v4-alpha.getbootstrap.com/)

First thing - create an empty file `_variables.scss` in `src/`. Then, because I am using `bootstrap-sass`, I added the following to `_variables.scss`:

```
$icon-font-path: '../node_modules/bootstrap-sass/assets/fonts/bootstrap/';
```
> The `~` syntax  resolves the path like a module, basically replaces `../node_modules/` here

Finally in `styles.scss` add the following:

```
@import 'variables';
@import '~bootstrap/scss/bootstrap';
```

## Using Font Awesome[^5]

[^5]: <https://github.com/angular/angular-cli/wiki/stories-include-font-awesome>

[Font Awesome](http://fontawesome.io/) gives you scalable vector icons that can instantly be customized — size, color, drop shadow, and anything that can be done with the power of CSS.

To use it, I had to initially install it via `npm install --save font-awesome`. Similar as before, add then in the `_variables.scss` the following:

```
$fa-font-path: "~font-awesome/fonts";
```

Same in `styles.scss`:

```
//import font-awesome
@import '~font-awesome/scss/font-awesome.scss';
```

## Using third party libraries[^6]

[^6]: <https://github.com/angular/angular-cli/wiki/stories-third-party-lib>

I use [showdown](http://showdownjs.github.io/demo/) to enable markdown[^7] in the description field of bookmarks. To enable it, **npm install** `showdown` and `@types/showdown`. Then in code just import it and it's ready to go:

```typescript
import {Injectable} from '@angular/core';

import * as showdown from 'showdown';

// const showdown = require('showdown');
const converter = new showdown.Converter();

@Injectable()
export class MarkdownService {
  // converter object is not typescript

  toHtml(text: string) {
    return converter.makeHtml(text);
  }
}
```

This approach is valid for any other libraries that have available typings. Even without them you can still manually add them[^6].

> Check out my post [How to use Showdown in Angular and NodeJS](http://www.codingpedia.org/ama/how-to-use-shodown-in-angular-and-nodejs) to see how I used it before...


[^7]: <https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet>

## Environment specific configuration

Every application that runs on more than just `localhost` has environment specific configurations, that we need to address.  

### Before

Before moving to Angular CLI, I set the environment specific variables via the Webpack DefinePlugin[^8] and `process.env`[^9]. The values would get replaced when building.

[^8]: https://webpack.github.io/docs/list-of-plugins.html#defineplugin
[^9]: https://nodejs.org/api/process.html#process_process_env

In `webpack.config.js`:

```
var API_URL = process.env.API_URL = '';
var isProd = ENV === 'build';
if (isProd) {
  API_URL = 'https://www.codingmarks.org/api';
} else {
  API_URL = 'http://localhost:3000/api';
}
...
new webpack.DefinePlugin({
  // Environment helpers
  'process.env': {
    ENV: JSON.stringify(ENV),
    'API_URL' : JSON.stringify(API_URL),
  }
})
```
and in code, where I would need the value:

```
constructor(private http: Http) {
  this.bookmarksUrl = process.env.API_URL + '/bookmarks/';
}
```

### After

Now, with Angular CLI, there is the concept of different environments file like `environment.ts` for development
and `environment.prod.ts` for production. These two files are created when a new app is generated via `ng new app`.
So the `API_URL` is now set in the two files.

`environment.ts` (dev)

```javascript
// The file contents for the current environment will overwrite these during build.
// The build system defaults to the dev environment which uses `environment.ts`, but if you do
// `ng build --env=prod` then `environment.prod.ts` will be used instead.
// The list of which env maps to which file can be found in `.angular-cli.json`.
export const environment = {
  production: false,
  API_URL: 'http://localhost:3000/api'
};
```

and

`environment.prod.ts` (prod):

```javascript
export const environment = {
  production: true,
  'https://www.codingmarks.org/api'
};
```
> The magic happens during builds - see in comments above

Then, in code just import the `environment` to get access to the desired value:

```typescript
import { environment } from 'environments/environment';

@Injectable()
export class BookmarkService {

  private bookmarksUrl = '';  // URL to web api

  constructor(private http: Http) {
    this.bookmarksUrl = environment.API_URL + '/bookmarks/';
  }
```

## Speed

I saved the best for last - <span class="highlight-yellow">speed of development, speed of building and speed at runtime</span> ( not that in my case, the website was not loading rapidly with Just-In-Time Compilation (~500ms), but Ahead-of-Time Compilation (~280ms) takes it one step further ).

With this I rest my case.

<p class="note_normal">
    PS: By the way you can find most of links referenced here if you filter the public bookmarks for the **angular-cli tag** in codingmarks - <a href="http://codingmarks.org/search?q=[angular-cli]" target="_blank">http://codingmarks.org/search?q=[angular-cli]</a>
</p>

## References
