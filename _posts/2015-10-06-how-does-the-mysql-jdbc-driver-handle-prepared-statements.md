---
id: 2488
title: How does the MySQL JDBC driver handle prepared statements
date: 2015-10-06T08:04:27+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=2488
permalink: /vladmihalcea/how-does-the-mysql-jdbc-driver-handle-prepared-statements/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4198263800
fsb_social_facebook:
  - 0
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 3
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - database
  - java
tags:
  - driver
  - jdbc
  - mysql
---
## Prepared statement types

<p style="text-align: justify;">
  While researching for the<span class="Apple-converted-space"> </span><em>Statement Caching</em><span class="Apple-converted-space"> </span>chapter in my<span class="Apple-converted-space"> </span><a href="https://leanpub.com/high-performance-java-persistence">High-Performance Java Persistence</a><span class="Apple-converted-space"> </span>book, I got the chance to compare how Oracle, SQL Server, PostgreSQL and MySQL handle prepare statements.
</p>

<p style="text-align: justify;">
  Thanks to<span class="Apple-converted-space"> </span><a href="https://www.linkedin.com/in/jbalint">Jess Balint</a><span class="Apple-converted-space"> </span>(MySQL JDBC driver contributor), who gave a<span class="Apple-converted-space"> </span><a href="http://stackoverflow.com/questions/32286518/whats-the-difference-between-cacheprepstmts-and-useserverprepstmts-in-mysql-jdb/32645365#32645365">wonderful answer</a><span class="Apple-converted-space"> </span>on StackOverflow, I managed to get a better understanding of how MySQL handles prepared statements from a database performance point of view.
</p>

<p style="text-align: justify;">
  Basically, there are two ways of preparing a statement: on the server-side or on the client-side.<!--more-->
</p>

### Server-side prepared statements

<p style="text-align: justify;">
  The most common type is the server-side statement, which requires two database round-trips:
</p>

<ul style="text-align: justify;">
  <li>
    The driver submits a<span class="Apple-converted-space"> </span><em>prepare</em><span class="Apple-converted-space"> </span>request and the database parses the statement into a<span class="Apple-converted-space"> </span><em>query tree</em>, which can also be transformed into a pre-optimized tree structure. Because it is very difficult to build an<span class="Apple-converted-space"> </span><em>execution plan</em><span class="Apple-converted-space"> </span>without the actual bind parameter values, the execution plan is deferred until the statement gets executed
  </li>
  <li>
    The execution request contains the current bind values, which the database uses to transform the parse tree into an optimal execution plan. The executor takes the plan and builds the associated result set.
  </li>
</ul>

<p style="text-align: justify;">
  If the data access logic doesn’t cache prepared statements, the extra database round-trip can actually hurt performance. For this purpose, some database systems don’t default to server-side prepared statement and execute a client-side statement preparation instead.
</p>

<p style="text-align: justify;">
  To enable server-side prepared statement, the<span class="Apple-converted-space"> </span><em>useServerPrepStmts</em><span class="Apple-converted-space"> </span>property must be enabled.
</p>

### Client-side prepared statements

<p style="text-align: justify;">
  When the statement is prepared on the client-side, the bind parameter tokens are replaced with actual parameter values prior to sending the statement to the database server. This way, the driver can use a single request to fetch the result set.
</p>

## Caching statements

<p style="text-align: justify;">
  In a high-performance OLTP system, statement caching plays a very important role in lowering transaction latencies. To avoid preparing a statement multiple times, the MySQL driver offers a client-side statement cache. Being disabled by default, the cache is activated by the<span class="Apple-converted-space"> </span><em>cachePrepStmts</em><span class="Apple-converted-space"> </span>Connection property.
</p>

<p style="text-align: justify;">
  For client-side statements, the tokenized statement structure can be reused in-between different preparing statement calls. The cache is bound to a database connection, but when using a connection pool, the physical connection lifetime spans over multiple application-level transactions (so frequently executed statements can benefit from using the cache).
</p>

<p style="text-align: justify;">
  For server-side statements, the driver caches the<span class="Apple-converted-space"> </span><a href="http://grepcode.com/file/repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.35/com/mysql/jdbc/ServerPreparedStatement.java#ServerPreparedStatement">ServerPreparedStatement</a>, as well as the check for server-side statement support (not all statements are allowed to be prepared on the server-side).
</p>

<p style="text-align: justify;">
  Caching statements can have a significant impact on application performance If you are interested in this topic, then you might as well subscribe for the<span class="Apple-converted-space"> </span><a href="https://leanpub.com/high-performance-java-persistence">High-Performance Java Persistence</a><span class="Apple-converted-space"> </span>book status notification.
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with permission of Vlad Mihalcea – source <a title="http://vladmihalcea.com/2015/03/05/a-beginners-guide-to-jpa-and-hibernate-cascade-types/" href="http://vladmihalcea.com/2015/09/21/how-does-the-mysql-jdbc-driver-handle-prepared-statements/" target="_blank">How does the MySQL JDBC driver handle prepared statements</a> from http://vladmihalcea.com/
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://lh5.googleusercontent.com/-TE09duPdvbA/U1pkmDy2uSI/AAAAAAAACUM/0AVivijfro4/w896-h897-no/VladMihalcea.jpg" alt="Vlad Mihalcea" /> 
  
  <p id="about_author_header">
    <strong>Vlad Mihalcea</strong>
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Software architect passionate about software integration, high scalability and concurrency challenges
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://vladmihalcea.com/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/102351970868518518557/posts" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/vlad_mihalcea" target="_blank"> </a> <a class="icon-github" href="https://github.com/vladmihalcea" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/pub/vlad-mihalcea/20/a59/580" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>
