---
layout: post
title: How to not select attribute in mongoose schema
description: "How to not select attribute in mongoose schema code snippet"
author: ama
permalink: /snippets/6442adcc83bdf3228599b1f1/how-to-not-select-attribute-in-mongoose-schema
published: true
snippetId: 6442adcc83bdf3228599b1f1
categories: [snippets]
tags: [javascript, mongoose, mongo, codever-snippets]
---

Use `select: false` when defining an attribute of a mongoose schema,
like `initiator: {type:String, select: false}` in the following schema for a bookmark object:

```html
const bookmarkSchema = new Schema(
  {
    shareableId: { type: String, select: false },
    name: { type: String, required: true },
    type: { type: String, required:  true, default: 'bookmark' },
    location: { type: String, required: true },
    description: String,
    descriptionHtml: String,
    tags: [String],
    initiator: {type:String, select: false},
    publishedOn: Date,
    sourceCodeURL: { type: String },
    userId: { type: String, ref: 'User' },
    userDisplayName: String,
    public: Boolean,
    language: String,
    lastAccessedAt: { type: Date, select: false },
    likeCount: Number,
    ownerVisitCount: { type: Number, select: false },
    youtubeVideoId: { type: String, required: false },
    stackoverflowQuestionId: { type: String, required: false },
    __v: { type: Number, select: false },
  },
  {
    timestamps: true,
  }
);
```

**Project**: `codever` - **File**:  `bookmark.js`

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="6442adcc83bdf3228599b1f1" %}
