---
layout: post
title: Create and delete an index on oracle table columns
description: "Code snippet example showing how to create and delete index on oracle table column(s)"
author: ama
permalink: /snippets/60019329e6eb82224c98e625/create-and-delete-index-on-oracle-table-columns
published: true
snippetId: 60019329e6eb82224c98e625
categories: [snippets]
tags: [oracle, sql, ddl, database]
---

Suppose we have the following table, and we often execute queries to display the bookmarks belonging to a certain `category`:

```sql
CREATE TABLE bookmark (
    id           NUMBER(10, 0), -- number 10 digits before the decimal and 0 digits after the decimal
    title        VARCHAR2(255 CHAR) NOT NULL, -- String with a maximum length of 255 charachters
    url          VARCHAR2(500 CHAR) UNIQUE NOT NULL, -- holds unique values across the table data
    category     VARCHAR2(500 CHAR) NOT NULL, -- holds unique values across the table data
    is_public    NUMBER(1, 0) NOT NULL, -- plays the role of a boolean '0'-false, '1'-true ,
    created_at   DATE NOT NULL, --  when the entry is created
    PRIMARY KEY ( id )
);
```

Then an index on the column `category` would make the query more performant.
 To create it, issue the following command (creates by default a [B-Tree index](https://blogs.oracle.com/sql/how-to-create-and-use-indexes-in-oracle-database)):

```sql
CREATE INDEX category_i ON bookmark (category);

--confirm its creation by querying the existing index on the table
SELECT * FROM all_indexes WHERE table_name = 'BOOKMARK';
```

Now, maybe retrieving the latest bookmarks with a category is also a common scenario, so it might be worth creating then an index on multiple columns,
 in our case `category` and `created_at`. The command would look something like the following:

```sql
CREATE INDEX category_created_at_i ON bookmark (category, created_at DESC);

-- this would be a perfect fit for the following query
SELECT * FROM bookmark WHERE category='blog' ORDER BY created_at DESC;
```

To delete the created indexes use the `DROP INDEX` commands:

```sql
DROP INDEX category_i;
DROP INDEX category_created_at_i;
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://blogs.oracle.com/sql/how-to-create-and-use-indexes-in-oracle-database" target="_blank" style="font-weight: lighter">
     https://blogs.oracle.com/sql/how-to-create-and-use-indexes-in-oracle-database
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60019329e6eb82224c98e625" %}
