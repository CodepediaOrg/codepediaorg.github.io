---
layout: post
title: How to query date range in Mongo
description: "Code snippets showing how to query date ranges in MongoDb. This can work also for a specific
day. Use only date or datetime."
author: ama
permalink: /snippets/60c8a106c807505ef1971306/how-to-query-date-range-in-mongo
published: true
snippetId: 60c8a106c807505ef1971306
categories: [snippets]
tags: [mongodb, terminal, shell, codever-snippets]
---

## Use only the **date** part
In the following example only the **date** part is set. Time is set to by default to `00:00:00`. So the following query compares all entries _archived_ within *a day* (`2021-06-14`):

```javascript
var start = new Date("2021-06-14");
var end = new Date("2021-06-15");

db.call_archive.find({archivedAt: {$gte: start, $lt: end}}).pretty();
```

## Use both **date** and **time**

You can also specify the Time in ISO format
- `new Date("<YYYY-mm-ddTHH:MM:ss>")` - specifies the datetime in the client's local timezone and returns the ISODate with the specified datetime in UTC
- `new Date("<YYYY-mm-ddTHH:MM:ssZ>") ` - specifies the datetime in UTC and returns the ISODate with the specified datetime in UTC

```javascript
var start = new Date("2021-06-14T13:13:13Z");
var end = new Date("2021-06-14T14:13:13Z");

db.call_archive.find({archivedAt: {$gte: start, $lt: end}}).sort({archivedAt: -1}).pretty(); //sort descending
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.mongodb.com/manual/reference/method/Date/" target="_blank" style="font-weight: lighter">
     https://docs.mongodb.com/manual/reference/method/Date/
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60c8a106c807505ef1971306" %}
