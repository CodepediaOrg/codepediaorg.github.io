---
layout: post
title: Count entries created in the last day in an oracle database
description: "How to count entries created in the last day in an oracle database code snippet"
author: ama
permalink: /snippets/61a8ee5afb0221040e493f0f/count-entries-created-in-the-last-day-in-an-oracle-database
published: true
snippetId: 61a8ee5afb0221040e493f0f
categories: [snippets]
tags: [oracle, sql, codever-snippets]
---

Use `sysdate - 1` (defaults to day unit) and compare with your timestamp column (in this case `CREATED_AT`) :

```sql
select count(*)
from PARTNER
WHERE CREATED_AT > (sysdate - 1)

```

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="61a8ee5afb0221040e493f0f" %}
