---
layout: post
title: Create a unique constraint on existing column in oracle database
description: "Code snippet showing how to create a unique constraint on existing column in oracle database"
author: ama
permalink: /snippets/60014398a4155b6d32a91d6c/create-an-unique-constraint-on-existing-column-in-oracle-database
published: true
snippetId: 60014398a4155b6d32a91d6c
categories: [snippets]
tags: [oracle, sql, ddl]
---

Let's say we have the following `bookmark` table and we want to add an unique constraint on the `url` column:

```sql
CREATE TABLE bookmark (
    id          NUMBER(10, 0),
    url         VARCHAR2(500 CHAR) ,
    PRIMARY KEY ( id )
);

INSERT INTO bookmark ( id, title, url, is_public) VALUES (1, 'https://www.codever.dev');
INSERT INTO bookmark ( id, title, url, is_public) VALUES (2, 'https://www.codepedia.org');
INSERT INTO bookmark ( id, title, url, is_public) VALUES (3, 'https://www.codever.dev');
```

Normally you would use the following `ALTER TABLE` command to add a unique constraint on the `url` column

```sql
ALTER TABLE bookmark ADD CONSTRAINT unique_url UNIQUE (url);

-- but you get an ORA-02299 saying the "unique_url" cannot be validated, because the table has duplicate key values
```

In this case and if you have existing data, that might not be unique you will get an `ORA: error`, if you execute the command before.
 However, you can overcome this if you use one of the following:

```sql
-- tell oracle not to validate the existing data at the time of the creation of the uniqueconstraint
ALTER TABLE bookmark ADD CONSTRAINT unique_url UNIQUE (url) DEFERRABLE NOVALIDATE;

-- create the unique constraint "disabled" and "enable" after that with the "NOVALIDATE" option
ALTER TABLE bookmark ADD CONSTRAINT unique_url UNIQUE (url) DISABLE;
ALTER TABLE bookmark ENABLE NOVALIDATE CONSTRAINT unique_url;

```

To see the created constraint and its attributes use the following command(s):

```sql
-- show all constraint for the table
SELECT * FROM user_constraints WHERE table_name = 'BOOKMARK'; -- note upper case

-- show our created unique constraint
SELECT * FROM user_constraints WHERE table_name = 'BOOKMARK'
   AND CONSTRAINT_NAME ='UNIQUE_URL';  -- note also here the upper case

```

To drop the unique constraint use the following command:

```sql
ALTER TABLE bookmark DROP CONSTRAINT unique_url;
```

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60014398a4155b6d32a91d6c" %}
