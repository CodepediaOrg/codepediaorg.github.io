---
layout: post
title: Count entries created recently in an oracle database
description: "Count entries created recently in an oracle database code snippet"
author: ama
permalink: /snippets/61a8ee5afb0221040e493f0f/count-entries-created-recently-in-an-oracle-database
published: true
snippetId: 61a8ee5afb0221040e493f0f
categories: [snippets]
tags: [oracle, sql, codever-snippets]
---

We will base the logic around [`sysdate`](https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions172.htm) which returns the current datetime, from which we substract units. For example for the **last day** from now use `sysdate - 1` (defaults to day) and compare with the timestamp column (in this case `CREATED_AT`) :

```sql
select count(*)
from PARTNER
WHERE CREATED_AT > (sysdate - 1)

-- last 2 days would be
select count(*)
from PARTNER
WHERE CREATED_AT > (sysdate - 2)
```

From the **last hour**, respectively **last two hours** use the following commands, where `1/24` is the unit for hour:

```sql
-- last hour
select count(*)
from PARTNER
WHERE CREATED_AT > (sysdate - 1)

-- last 2 hours
select count(*)
from PARTNER
WHERE CREATED_AT > (sysdate - 2/24)
```

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="61a8ee5afb0221040e493f0f" %}
