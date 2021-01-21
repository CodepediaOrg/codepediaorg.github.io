---
layout: post
title: Get the current unix timestamp in oracle
description: "Code snippet showing how to get the current unix timestamp in Oracle database. This is similar to "
Java's currentTimeMillis"
author: ama
permalink: /snippets/60092685a4155b6d32a92582/get-the-current-unix-timestamp-in-oracle
published: true
snippetId: 60092685a4155b6d32a92582
categories: [snippets]
tags: [oracle, database, sql, datetime, unix-timestamp]
---

The following expression returns the Unix time in milliseconds independent of the time zone.

It sums the number of days passed from the start of the Unix epoch multiplied by the number of milliseconds in a day (24 * 60 * 60 * 1000 = 86400000) plus number of milliseconds past midnight (`SSSSS` with a precision of 3 `FF3` to express milliseconds).

It uses the `SYS_EXTRACT_UTC` method to extract the UTC (Coordinated Universal Time—formerly Greenwich Mean Time) from the current timestamp  (`systimestamp` in oracle):

```sql
SELECT
     EXTRACT(DAY FROM(sys_extract_utc(systimestamp) - to_timestamp('1970-01-01', 'YYYY-MM-DD'))) * 86400000
    + to_number(TO_CHAR(sys_extract_utc(systimestamp), 'SSSSSFF3'))
  FROM dual;
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/SYS_EXTRACT_UTC.html" target="_blank" style="font-weight: lighter">
     https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/SYS_EXTRACT_UTC.html
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60092685a4155b6d32a92582" %}
