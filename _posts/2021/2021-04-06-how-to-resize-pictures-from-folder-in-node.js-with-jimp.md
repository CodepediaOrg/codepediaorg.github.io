---
layout: post
title: How to resize pictures from folder in Node.js with Jimp
description: "Simple api tool implementation on localhost to resize from folder in Node.js with Jimp"
author: ama
permalink: /ama/resize-pictures-in-nodejs-with-jimp
published: true
categories: [article]
tags: [productivity, node.js, image-processing]
---

Recently I wrote a [few posts on my personal blog](https://www.adixchen.com/blog/) with lots of pictures. To save
on bandwidth and load time it's very important to reduce the size of the images. Which until recently I did manually in Preview, MacOS.
 It was a rather cumbersome manual process, that I automated with the help of Node.js/ExpressJS and [Jimp](https://github.com/oliver-moran/jimp)
 In this post I will presently the implementation.

> The code for this nad usage instructions are open source and available on [Github](https://github.com/CodepediaOrg/image-resize-batch-api#readme)

<!--more-->

### How it is implemented behind the scenes?

Becasue the implementation is so small, everything happens in the router:

```javascript
const path = require('path');

const express = require('express');
const router = express.Router();
const Jimp = require('jimp');

const fs = require('fs');
const util = require('util');

const readdir = util.promisify(fs.readdir);

router.post('/', async (request, response) => {
    const {width} = request.body;
    if ( !width ) {
        response.status(400).send({"validation_error": "desired width is mandatory"});
    }
    const quality = request.body.quality || 100;
    const numberPrefixOnly = request.body.numberPrefixOnly || false;

    const imgDir = request.body.imgDir || 'images';//fallback to images in input folder if no path is provided
    const outputImgDir = `${imgDir}/resized`;

    let files = await readdir(imgDir, {withFileTypes: true});

    files = files.filter(file => {
        if ( numberPrefixOnly ) {
            return file.name.match(/^\d/) && file.name.match(/\.(jpe?g|png|gif)$/)
        } else {
            return file.name.match(/\.(jpe?g|png|gif)$/)
        }
    });

    for ( const file of files ) {
        const image = await Jimp.read(`${imgDir}/${file.name}`);
        await image.resize(width, Jimp.AUTO);
        await image.quality(quality);
        let newFileName = '';
        if ( file.name.match(/^\d/) ) {
            const photoNumber = file.name.substring(0, file.name.indexOf('-'));
            newFileName = `${photoNumber}-${width}x${image.bitmap.height}-${file.name.substring(file.name.indexOf('-') + 1, file.length)}`;
        } else {
            newFileName = `${width}x${image.bitmap.height}-${file.name}`;
        }

        await image.writeAsync(`${outputImgDir}/${newFileName}`);
        console.log(`${outputImgDir}/${newFileName}`);
    }

    return response.status(201).send();
});

module.exports = router;
```

The API expects a POST request with the following parameters in the body, where as seen in code some defaults are set:
- `width` - **required** desired width list (height is automatically scaled)
- `quality` - (0 to 100)
- `numberPrefixOnly`
  - `false` - **default** all images in the directory are considered for resizing
  - `true` - **ONLY** images with number prefixes are resized (e.g. `4-good-view-zweisimmen-north.jpeg`)
- `imgDir` (optional) - **absolute path** where the images are stored
  - the resized images are placed in the `${imgDir}/resized` sub-directory
  - if this parameter is not provided the program expects that the images are placed in the [images](images)
  directory. Resized images are then generated in [images/resized](images/resized) directory
  - tested only in MacOS. Should work fine in Linux OS. For Windows place the images in the `input` directory
  as mentioned above

The whole Jimp part is listed in the following lines of code:
```javascript
    const image = await Jimp.read(`${imgDir}/${file.name}`);
    await image.resize(width, Jimp.AUTO);
    await image.quality(quality);
    let newFileName = '';
    if ( file.name.match(/^\d/) ) {
        const photoNumber = file.name.substring(0, file.name.indexOf('-'));
        newFileName = `${photoNumber}-${width}x${image.bitmap.height}-${file.name.substring(file.name.indexOf('-') + 1, file.length)}`;
    } else {
        newFileName = `${width}x${image.bitmap.height}-${file.name}`;
    }

    await image.writeAsync(`${outputImgDir}/${newFileName}`);
```

- the image is `read` given its path
- the `resize` method expects the `width` and the height is autoscaled (`Jimp.AUTO`)
- the quality is via the `quality` method (defaults to 90, but with reasonable picture quality still)
- in the end a new name is generated (depending on the number prefix) and written to the `resized` folder

Install and start the server with the following commands:
```shell
npm install
npm start
```

Then you  can [use `curl`]({% post_url 2014-12-03-how-to-test-a-rest-api-from-command-line-with-curl %}) or your favorite api client tool to make `POST` requests:
```shell
curl -0 -v -X POST localhost:9000/resize \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "width": 1200,
    "quality": 90,
    "numberPrefixOnly": false,
    "imgDir": "/Users/ama/Desktop/post-zweisimmen"
}
EOF
```

It's as simple as that...
