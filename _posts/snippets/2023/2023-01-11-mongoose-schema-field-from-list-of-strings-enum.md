---
layout: post
title: Mongoose schema field from list of strings (enum)
description: "Mongoose schema field from list of strings (enum) code snippet"
author: ama
permalink: /snippets/63b8562e3b43b9521ed04b0d/mongoose-schema-field-from-list-of-strings-enum-
published: true
snippetId: 63b8562e3b43b9521ed04b0d
categories: [snippets]
tags: [mongoose, mongodb, javascript, codever-snippets]
---

In the following example we define `template` attribute to be of type `String` with values defined in the `enum` array:

```javascript
const noteSchema = new Schema({
    title: {type:String, required: true},
    type: {type:String, required: true, default: 'note'},
    content: String,
    reference: String,
    tags: [String],
    template: {type:String, enum: ['note', 'checklist']},
    userId: {type: String, ref:'User'},
    __v: { type: Number, select: false}
},
{
  timestamps: true
});
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="63b8562e3b43b9521ed04b0d" %}
