---
layout: post
title: How to get latest entries from MongoDB collection with Mongoose
description: "Example on how to retrieve the latest entries from a MongoDB collection. Two possibilities are presented."
author: ama
permalink: /ama/how-to-get-latest-entries-from-mongodb-collection-with-mongoose
published: true
categories: [mongodb]
tags: [codingmarks, mongodb, mongoose, nodejs]
---

I have a use case where I need to retrieve the latest added public [codingmarks](https://github.com/Codingpedia/codingmarks).
They are store in a mongo database, and I use Mongoose as ORM. 

I want to have two possibilites to achieve this:

1. specify the number of days to look back
2. specify the timestamp since when to look forward

I think the code is self explanatory
```
/**
 * Returns the codingmarks added recently.
 * 
 * The since query parameter is a timestamp which specifies the date since we want to look forward to present time.
 * If this parameter is present it has priority. If it is not present, we might specify the number of days to look back via
 * the query parameter numberOfDays. If not present it defaults to 7 days, last week.
 *
 */
router.get('/latest-entries', async (req, res) => {
  try
  {

    if(req.query.since) {
      const bookmarks = await Bookmark.find(
        {
          createdAt: { $gte: new Date(parseFloat(req.query.since,0)) }
        }).sort({createdAt: 'desc'}).lean().exec();

      res.send(bookmarks);
    } else {
      const numberOfDaysToLookBack = req.query.days ? req.query.days : 7;

      const bookmarks = await Bookmark.find(
        {
          createdAt: { $gte: new Date((new Date().getTime() - (numberOfDaysToLookBack * 24 * 60 * 60 * 1000))) }
        }).sort({createdAt: 'desc'}).lean().exec();

      res.send(bookmarks);
    }

  }
  catch (err)
  {
    return res.status(500).send(err);
  }
});
```



