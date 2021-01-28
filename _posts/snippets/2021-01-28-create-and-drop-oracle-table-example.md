---
layout: post
title: Create and drop oracle table example
description: "Code snippet showing how to create and drop a table in an oracle database"
author: ama
permalink: /snippets/60009253e6eb82224c98e4bb/create-and-drop-oracle-table-example
published: true
snippetId: 60009253e6eb82224c98e4bb
categories: [snippets]
tags: [oracle, sql, ddl]
---

Create an example table in an existing Oracle schema, which holds data about bookmarks.
 Add some comments as metadata to the table and to a column:

```sql
CREATE TABLE bookmark (
    id                 NUMBER(10, 0), -- number 10 digits before the decimal and 0 digits after the decimal
    title              VARCHAR2(255 CHAR) NOT NULL, -- String with a maximum length of 255 charachters
    url                VARCHAR2(500 CHAR) UNIQUE NOT NULL, -- holds unique values across the table data
    category      VARCHAR2(500 CHAR) NOT NULL, -- holds unique values across the table data
    is_public      NUMBER(1, 0) NOT NULL, -- plays the role of a boolean '0'-false, '1'-true ,
    created_at   DATE NOT NULL, --  when the entry is created
    PRIMARY KEY( id )
);

COMMENT ON TABLE bookmark IS
    'Table holding data about bookmarks';

COMMENT ON COLUMN bookmark.is_public IS
    'Boolean like 1-is public accessible, 0-is private';
```

With the table now created, insert values in it with the following syntax

```sql
INSERT INTO bookmark ( id, title, url, category, is_public, created_at )
VALUES (
    1,
    'BookmarksDev - Bookmarks and Code Snippets Manager',
    'https://www.bookmarks.dev',
    'developer-tools',
    1,
    TO_DATE( '2021-01-01', 'YYYY-MM-DD' )
);

INSERT INTO bookmark ( id, title, url, category, is_public, created_at )
VALUES (
    2,
    'CodepediaOrg - Share code knowledge',
    'https://www.codepedia.org',
    ' blog',
    1,
    SYSDATE -- current time in oracle
);

SELECT * FROM bookmark;
```

To remove the table, all rows from the table, table indexes and domain indexes are removed with the following command

```sql
DROP TABLE bookmark;
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60009253e6eb82224c98e4bb" %}
