---
layout: post
title: How to use a cursor in a Oracle PLSQL FOR loop
description: "Example showing how to use the cursor in an oracle plsql for loop. Based on a scenario where data from main table
is updated and new entries in the revision data are inserted."
author: ama
permalink: /ama/how-to-use-a-cursor-in-oracle-plsql-for-loop
published: true
snippetId: 60019329e6eb82224c98e625
categories: [tutorials]
tags: [oracle, sql, plsql, ddl, database]
---

To use a PL/SQL cursor we set up a scenario where we want to update values from a main table and also add auditing data about it.
 We create some tables in oracle database to hold data about bookmarks and revision info data (bookmarks auditing table, and a revision info table):
* **BOOKMARK** - to hold data about bookmarks
* **REVINFO** - holds data about the revisions that took place in our system
* **BOOKMARK_AUD** - audit table, to hold data from the main table (**BOOKMARK**) that were affected by the different revisions

> The concept from the scenarios is inspired from the [Hibernate Envers](https://hibernate.org/orm/envers/) project, where you would the same functionality
> in java, automatically, with annotations. There might be cases where you need to this at the SQL level, maybe for
> some single time house keeping jobs or similar...

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


-- revision info table
CREATE TABLE revinfo
(
    id NUMBER(10, 0) NOT NULL,
    revtstmp NUMBER(10, 0), -- unix time in milliseconds
    PRIMARY KEY( id )
)

-- audit table for BOOKMARK table
CREATE TABLE bookmark_aud (
    id                 NUMBER(10, 0),
    rev NUMBER(10, 0) NOT NULL, -- the id of the revision
    revtype NUMBER(1, 0), -- 0=CREATE, 1=MODIFY and 2=DELETE
    title              VARCHAR2(255 CHAR) NOT NULL,
    url                VARCHAR2(500 CHAR) UNIQUE NOT NULL,
    category      VARCHAR2(500 CHAR) NOT NULL,
    is_public      NUMBER(1, 0) NOT NULL,
    created_at   DATE NOT NULL,
    PRIMARY KEY( id )
);
```


More exactly, we want to update all the entries from the **bookmark** table that have a given category (`v_category` in plsql code),
with a new one (`v_new_category` in the plsql script). We select the values afect in a cursor (`bookmark_cur` in plsql script),
which we iterate over in a plsql `FOR LOOP` to do the individual updates and inserts. If any exception occurs in the loop
it will be printed in the `DBMS_OUTPUT`:

```sql
SET SERVEROUTPUT ON;
DECLARE
    v_category VARCHAR2(500) := 'javax';
    v_new_category VARCHAR2(500) := 'java';
    next_rev_id NUMBER := HIBERNATE_SEQUENCE.nextval;
    created_at NUMBER(19, 0);
    CURSOR bookmark_cur IS
        SELECT
            *
        FROM
            bookmark
        WHERE
          category = v_category
       );
BEGIN
    -- get current timestamp as number
    SELECT
                EXTRACT(DAY FROM(sys_extract_utc(systimestamp) - to_timestamp('1970-01-01', 'YYYY-MM-DD'))) * 86400000
            + to_number(TO_CHAR(sys_extract_utc(systimestamp), 'SSSSSFF3'))
    INTO created_at
    FROM
        dual;

    -- update REVISION table
    INSERT
    INTO revinfo (
               rev,
               revtstmp
    )
    VALUES (
       next_rev_id,
       created_at
    );

    FOR c IN bookmark_cur LOOP
        BEGIN
            -- update BOOKMARK table
            UPDATE bookmark
            SET
                category = v_new_category
            WHERE
                 id = c.id;

            -- INSERT ENTRY into BOOKMARK_AUD table
            INSERT INTO bookmark_aud (
               id
               title,
               url,
               category,
               is_public,
               created_at,
               rev,
               revtype
            ) VALUES (
               c.id
               c.title,
               c.url,
               c.category,
               c.is_public,
               c. created_at,
               next_rev_id,
               1 -- 1=UPDATE/MODIFY (0=INSERT and 2=DELETE)
           );
        EXCEPTION
            WHEN OTHERS THEN --handle all exceptions
                DBMS_OUTPUT.PUT_LINE('Exception' || SQLCODE || ' captured for bookmark with id ' || c.id);
        END;
    END LOOP;
END;
/
```

<span class="highlight-yellow">But beware, it is not recommended using a cursor FOR loop if the body executes non-query DML (INSERT, UPDATE, DELETE, MERGE),
because the INSERT or UPDATE will happen on a row-by-row basis</span>[^1].

[^1]: <https://blogs.oracle.com/oraclemagazine/on-cursor-for-loops>

In this particular case you can ditch the cursor and use use plain SQL updates and inserts in the plsql body, as shown
in the snippet below:
```sql
BEGIN
    -- get current timestamp as number
    SELECT
                EXTRACT(DAY FROM(sys_extract_utc(systimestamp) - to_timestamp('1970-01-01', 'YYYY-MM-DD'))) * 86400000
            + to_number(TO_CHAR(sys_extract_utc(systimestamp), 'SSSSSFF3'))
    INTO created_at
    FROM
        dual;

    -- update REVISION table
    INSERT
        INTO revinfo (
           rev,
           revtstmp
        )
        VALUES (
           next_rev_id,
           created_at
        );


    -- update BOOKMARK table
    UPDATE bookmark
    SET
        category = v_new_category
    WHERE
        category = v_category; --

    -- INSERT ENTRY into BOOKMARK_AUD table
    INSERT INTO bookmark_aud (
        id
        title,
        url,
        category,
        is_public,
        created_at,
        rev,
        revtype
    )
    SELECT
        c.id
        c.title,
        c.url,
        c.category,
        c.is_public,
        c. created_at,
        next_rev_id,
        1 -- 1=UPDATE/MODIFY (0=INSERT and 2=DELETE)
    FROM bookmark c
    WHERE
        c.id in (
           SELECT
               id
           FROM
                bookmark
           WHERE
             category = v_category
        )
    );
    EXCEPTION
        WHEN OTHERS THEN --handle all exceptions
              DBMS_OUTPUT.PUT_LINE('Exception' || SQLCODE || ' captured for bookmark with id ' || c.id);
END;
/
```

> Note some times is not possible and then you might have to stick to the cursor. In oracle you can take advantage
> of the CURSOR bulk updates and INSERTS[^2]

[^2]: <https://blogs.oracle.com/oraclemagazine/on-cursor-for-loops>
https://blogs.oracle.com/oraclemagazine/bulk-processing-with-bulk-collect-and-forall

## References
