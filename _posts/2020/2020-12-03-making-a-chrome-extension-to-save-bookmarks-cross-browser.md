---
layout: post
title: Making a chrome extension to save bookmarks cross-browser compatible
description: "Shows how to make a real life chrome extension compatible with firefox with the help of webextension polyfill from mozilla"
author: ama
permalink: /ama/making-a-chrome-extension-to-save-bookmarks-cross-browser-compatible
published: true
categories: [tutorial]
tags:
    - javascript
    - browser-extension
    - bookmarks.dev
---

When I first just published the ["Save to Bookmarks.dev"  chrome extension in the Chrome Web Store](https://chrome.google.com/webstore/detail/save-link-to-bookmarksdev/diofdblfhjbpgackifolmboaiccmebjb),
 I did not have the time to develop a version for Firefox, thinking I would need to learn another technology.
  Recently I developed another extension to [Save code snippets to Bookmarks.dev](https://github.com/BookmarksDev/code-snippets-browser-extension).
   This time I was determined to invest the time to make also Firefox version. As it turns out I had to make little
    to no changes in code base to make it work both in Chromium based and Firefox browsers. All this with great help from the [webextension polyfill](https://github.com/mozilla/webextension-polyfill)
 the guys from Mozilla created for us.

 So, in this blog post we are going to revisit the [Save link to Bookmarks.dev](https://github.com/BookmarksDev/code-snippets-browser-extension) extension,
  and I will detail the changes required to make the extension compatible with Firefox.

The, newly renamed,  **Save link to Bookmarks.dev** browser extension is available for:

| [![Chrome](https://dev-to-uploads.s3.amazonaws.com/i/8rbe1k13zjid9peah37k.png)](https://chrome.google.com/webstore/detail/save-url-to-bookmarksdev/diofdblfhjbpgackifolmboaiccmebjb) | [![Firefox](https://dev-to-uploads.s3.amazonaws.com/i/nsruw2rq4mveb4cp11u6.png)](https://addons.mozilla.org/en-US/firefox/addon/save-link-to-bookmarks-dev/) |
|:---:|:---:|
| [Chrome](https://chrome.google.com/webstore/detail/save-to-bookmarksdev/diofdblfhjbpgackifolmboaiccmebjb) | [Firefox](https://addons.mozilla.org/en-US/firefox/addon/save-link-to-bookmarks-dev/) |

Before I do that, let's have a look at the extension in action, so you know what I am talking about:

![Browser extension in action](https://dev-to-uploads.s3.amazonaws.com/i/p34m9dbvcrlk041a9oiv.gif)

> **Right click** OR **click the extension icon** to save the active tab's link to [Bookmarks.dev](https://www.codever.land)

<!--more-->

## Install the polyfill

Since this extension is fairly simple, it doesn't use a `package.json` or a webpack bundle, so I needed to download the polyfill script.
 All the versions released on npm are available for direct download from unpkg.com
* [https://unpkg.com/webextension-polyfill/dist/](https://unpkg.com/webextension-polyfill/dist/)

and linked to the Github releases

* [https://github.com/mozilla/webextension-polyfill/releases](https://github.com/mozilla/webextension-polyfill/releases)

> For extensions that already include a package.json file, the last released version of this library can be quickly installed using `npm install --save-dev webextension-polyfill`

## Setup of the polyfill

In order to use the polyfill, it must be loaded into any context where `browser` APIs are accessed.
 The most common cases are background and content scripts, which can be specified in `manifest.json`
  (make sure to include the `browser-polyfill.js` script <span class="highlight-yellow">before any other scripts that use it</span>):

```javascript
{
  // ...

  "background": {
    "scripts": [
      "browser-polyfill.js",
      "background.js"
    ]
  },

  "content_scripts": [{
    // ...
    "js": [
      "browser-polyfill.js",
      "content.js"
    ]
  }]
}
```

For HTML documents, such as `browserAction` popups, or tab pages see the setup section in the project's README[^1].

[^1]: <https://github.com/mozilla/webextension-polyfill#basic-setup>

## The implementation changes
The implementation uses a **background** script that will trigger the execution of another javascript,
 `launch-bookmarksdev-dialog.js` when clicked on the extension icon or right click and select **Save link to Bookmarks.dev**.
  Here I only needed to change `chrome` with `browser`, so not it looks like this:

```javascript
browser.browserAction.onClicked.addListener(launchBookmarksDevDialog);

function launchBookmarksDevDialog() {
    browser.tabs.executeScript({
        file: 'launch-bookmarksdev-dialog.js'
    });
};

browser.contextMenus.onClicked.addListener(launchBookmarksDevDialog);

browser.runtime.onInstalled.addListener(function () {
    browser.contextMenus.create({
        "id": "save-link-to-bookmarksdev",
        "title": "Save link to Bookmarks.dev",
        "contexts": ["all"]
    });
});

```

instead of `chrome.browserAction...`

## Test the extension
You can still test the extension locally by loading and reloading the sources either in Chrome or Firefox,
 but with the help `web-ext`[^2] things have gotten easier.

[^2]: <https://github.com/mozilla/web-ext#readme>

Just run the following command in the project root directory
```shell
web-ext run
```

This will start Firefox with the extension installed and reloads it when you do changes in the source code.
 For options see **web-ext command reference**.[^3]

[^3]: <https://github.com/mozilla/web-ext#readme>
https://extensionworkshop.com/documentation/develop/web-ext-command-reference/>

## Build the extension
Packaging the extension for release has also gotten easier with the help of `web-ext` utility.

If before I would use a `zip` command

```shell
zip -r bookmarks.browser.extension.zip * -x *.idea* *.git* '*resources/*' '*assets/*' "*README.md*" "*CHANGELOG.md*" '*web-ext-artifacts/*'
```

now I use the `web-ext` build command

```
web-ext build --overwrite-dest -i 'resources' 'assets' 'README.md' 'CHANGELOG.md'
```

This packages an extension into a `.zip` file, ignoring files that are commonly unwanted in packages, such as `.git` and other artifacts.
 The name of the `.zip` file is taken from the name field in the extension manifest.

You can still exclude files by yourself, as seen above with the help of the `-i`option. See the command reference[^3] for
further options.

## Conclusion

In this post you've seen a way how to migrate your Chrome Extension to be compatible with Firefox with the help
of webextension polyfill.

> If you have found this useful, please show some love and give us a star on [Github](https://github.com/codeverland/bookmarks-browser-extension)

## References

