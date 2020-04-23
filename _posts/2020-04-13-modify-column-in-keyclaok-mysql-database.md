---
layout: post
title: How to modify the size of a column in a Mysql database from command line
description: "Presents how to connect to the MySql database Keycloak and modify a column's size from the command line. It's
based on the investigation of bug "
author: ama
permalink: /ama/how-to-modify-a-column-size-in-a-mysql-database-from-command-line
published: true
categories: [debugging]
tags: [mysql, keycloak]
---

This blog post presents the steps required to connect to the MySql database from the command line and modify the size of
a column in a table. The example is based on the MySql database that is backing Keycloak to run [www.bookmarks.dev](https://www.bookmarks.dev).
For a setup of the environment you can see the wiki article [Keycloak MySQL Setup](https://github.com/CodepediaOrg/bookmarks.dev/wiki/Keycloak-MySQL-Setup)

<!--more-->

This all started with an error thrown by the Chrome extension - [Save to Bookmarks.dev](https://chrome.google.com/webstore/detail/save-to-bookmarksdev/diofdblfhjbpgackifolmboaiccmebjb)
when selecting a very long text to add the bookmark's description. The error in the Keycloak logs was the following:

```shell
Caused by: com.mysql.jdbc.MysqlDataTruncation: Data truncation: Data too long for column 'DETAILS_JSON' at row 1
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3971)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3909)
	at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2527)
	at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2680)
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2490)
	at com.mysql.jdbc.PreparedStatement.executeInternal(PreparedStatement.java:1858)
	at com.mysql.jdbc.PreparedStatement.executeUpdateInternal(PreparedStatement.java:2079)
	at com.mysql.jdbc.PreparedStatement.executeUpdateInternal(PreparedStatement.java:2013)
	at com.mysql.jdbc.PreparedStatement.executeLargeUpdate(PreparedStatement.java:5104)
	at com.mysql.jdbc.PreparedStatement.executeUpdate(PreparedStatement.java:1998)
	at org.jboss.jca.adapters.jdbc.WrappedPreparedStatement.executeUpdate(WrappedPreparedStatement.java:537)
	at org.hibernate.engine.jdbc.internal.ResultSetReturnImpl.executeUpdate(ResultSetReturnImpl.java:175)
	... 91 more
```

Fortunately in the this case the error in the logs is clear enough -  the column's size seemed too short to handle the bigger selection.
So let's dig in to correct the problem.


## Connect to the database

First connect to the database

```shell
mysql -u keycloak -p keycloak
```

where
- `keycloak` in `-u keycloak` is the username
- the last `keycloak` is the name of the database

You will be asked for the user's password (`-p`).


## Find the right table

The column name `DETAILS_JSON` was mentioned in logs. To find out which table this columns belongs to I listed the
columns of all tables from the `keycloak` database:

```sql
select * from information_schema.columns
where table_schema = 'keycloak'
order by table_name,ordinal_position;
```

The result was highly uninterpretable - I got a pretty long lists of columns because Keycloak uses lots of tables. So I needed a way
to grep the result. You can do that in mysql shell by issuing the following command

```shell
pager grep DETAILS_JSON;
```

and then run the above command again:

```shell
mysql> select * from information_schema.columns where table_schema = 'keycloak' order by table_name,ordinal_position;
| def           | keycloak     | EVENT_ENTITY                  | DETAILS_JSON                 |                3 | NULL                        | YES         | varchar   |                     5550 |                   5550 |              NULL |          NULL |               NULL | latin1             | latin1_swedish_ci | varchar(5550) |            |       | select,insert,update,references |                |                       |
519 rows in set (0.00 sec)
```

We can now clearly identify the table as `EVENT_ENTITY`

## Change the column's size

First we need to disable the grep/pager to be able to see anything else which does not contain the `DETAILS_JSON` text in it.
 You can do that by issuing the following command:

```shell
mysql> nopager;
```

Then display the columns of the `EVENT_ENTITY` table to make sure the searched column is there:

```shell
mysql> show columns from EVENT_ENTITY;
+--------------+---------------+------+-----+---------+-------+
| Field        | Type          | Null | Key | Default | Extra |
+--------------+---------------+------+-----+---------+-------+
| ID           | varchar(36)   | NO   | PRI | NULL    |       |
| CLIENT_ID    | varchar(255)  | YES  |     | NULL    |       |
| DETAILS_JSON | varchar(2550) | YES  |     | NULL    |       |
| ERROR        | varchar(255)  | YES  |     | NULL    |       |
| IP_ADDRESS   | varchar(255)  | YES  |     | NULL    |       |
| REALM_ID     | varchar(255)  | YES  |     | NULL    |       |
| SESSION_ID   | varchar(255)  | YES  |     | NULL    |       |
| EVENT_TIME   | bigint(20)    | YES  |     | NULL    |       |
| TYPE         | varchar(255)  | YES  |     | NULL    |       |
| USER_ID      | varchar(255)  | YES  |     | NULL    |       |
+--------------+---------------+------+-----+---------+-------+
10 rows in set (0.00 sec)
```

We can see now it has a size of `2550` characters. We'll just more than double that by altering the table:

```shell
mysql> ALTER TABLE EVENT_ENTITY MODIFY DETAILS_JSON VARCHAR(5550) ;
Query OK, 0 rows affected (0.00 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

Run the above command again to make sure the column has now the new size.


## Conclusion

This post serves me as a reminder on how to connect to a MySql database and modify a column.

