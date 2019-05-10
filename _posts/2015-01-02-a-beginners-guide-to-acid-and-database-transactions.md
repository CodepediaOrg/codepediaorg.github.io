---
id: 2194
title: 'A beginner&#8217;s guide to ACID and database transactions'
date: 2015-01-02T10:51:21+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=2194
permalink: /vladmihalcea/a-beginners-guide-to-acid-and-database-transactions/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3382334983
fsb_social_facebook:
  - 2
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - database
tags:
  - ACID
  - database
  - enterprise
  - isolation levels
  - transaction
---
<p style="color: #444444; text-align: justify;">
  Transactions are omnipresent in today’s enterprise systems, providing data integrity even in highly concurrent environments. So let’s get started by first defining the term and the context where you might usually employ it.<!--more-->
</p>

<p style="color: #444444; text-align: justify;">
  A transaction is a collection of read/write operations succeeding only if all contained operations succeed.
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/01/transaction-workflow1.gif"><img class="alignnone size-medium wp-image-1256" src="http://vladmihalcea.files.wordpress.com/2014/01/transaction-workflow1.gif?w=441&h=300" alt="Transaction-workflow" width="294" height="300" /></a>
</p>

<p style="color: #444444;">
  Inherently a transaction is characterized by four properties (commonly referred as ACID) :
</p>

<ol style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    Atomicity
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Consistency
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Isolation
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Durability
  </li>
</ol>

<p style="color: #444444; text-align: justify;">
  It’s very important to understand those, hence we will discuss each and every one of them as follows.
</p>

<p style="color: #444444; text-align: justify;">
  1. Atomicity takes individual operations and turns them into an all-or-nothing unit of work, succeeding if and only if all contained operations succeed.
</p>

<p style="color: #444444; text-align: justify;">
  2. A transaction might encapsulates a state change (unless it is a read-only one). A transaction must always leave the system in a consistent state, no matter how many concurrent transactions are inter-leaved at any given time.
</p>

<p style="color: #444444; text-align: justify;">
  Consistency has the following characteristics:
</p>

<ul style="color: #444444; text-align: justify;">
  <li style="font-weight: inherit; font-style: inherit;">
    if one operation triggers secondary actions (CASCADE, TRIGGERS), those must also succeed otherwise the transaction fails .
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    if the system is composed of multiple nodes, then consistency mandates that all changes be propagated to all nodes (<a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Multi-master_replication">multi-master replication</a>). If slaves nodes are updated asynchronously then we break the consistency rule, the system becoming “<a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Eventual_consistency">eventually consistent</a>“.
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    a transaction is a data state transition, so the system must operate as if all transactions occur in a serial form even if those are concurrently executed.
  </li>
</ul>

<p style="color: #444444; text-align: justify;">
  If there would be only one connection running at all times, then serializability wouldn’t impose any concurrency control cost. In reality, all transactional systems must accommodate concurrent requests, hence serialization has its toll on scalability. The <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Amdahl%27s_law">Amdahl’s law</a> describes the relation between serial execution and concurrency:
</p>

<blockquote style="color: #444444;">
  <p style="font-weight: inherit; font-style: inherit;">
    “The speed up of a program using multiple processors in parallel computing is limited by the time needed for the sequential fraction of the program.”
  </p>
</blockquote>

<p style="color: #444444; text-align: justify;">
  As you’ll see later, most database management systems choose (by default) to relax consistency to achieve better concurrency.
</p>

<p style="color: #444444; text-align: justify;">
  3. Isolation
</p>

<p style="color: #444444; text-align: justify;">
  Transactions are concurrency control mechanisms, and they deliver consistency even when being interleaved. Isolation brings us the benefit of hiding uncommitted state changes from the outside world, as failing transactions shouldn’t ever corrupt the state of the system. Isolation is achieved through <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Concurrency_control">concurrency control</a> using pessimistic or optimistic locking mechanisms.
</p>

<p style="color: #444444;">
  4. Durability
</p>

<p style="color: #444444; text-align: justify;">
  A successful transaction must permanently change the state of a system, and before ending it, the state changes are recorded in a persisted <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Transaction_log">transaction log</a>. If our system is suddenly affected by a system crash or a power outage, then all unfinished committed transactions may be replayed.
</p>

<p style="color: #444444; text-align: justify;">
  For messaging systems like <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Java_Message_Service">JMS</a>, transactions are not mandatory. That’s why we have non-transacted <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/6/api/javax/jms/Session.html">acknowledgement modes</a>.
</p>

<p style="color: #444444; text-align: justify;">
  File system operations are usually non-managed, but if your business requirements demand transaction file operations, you might make use a tool such as <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://xadisk.java.net/">XADisk</a>.
</p>

<p style="color: #444444; text-align: justify;">
  While, messaging and file systems use transactions optionally, for database management systems transactions are compulsory. That’s the reason database connections define a default <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Autocommit">autocommit</a> mode. ACID is mandated by the SQL Standard, hence all operations must be embedded in a database transaction. But for an enterprise application, the autocommit mode is something you’d generally avoid, since it performs badly and it doesn’t allow you to include multiple <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Data_manipulation_language">DML</a> operations in a single atomic unit of work.
</p>

<p style="color: #444444; text-align: justify;">
  ACID is old school. <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://research.microsoft.com/en-us/um/people/gray/papers/theTransactionConcept.pdf">Jim Gray</a> described atomicity, consistency and durability long before I was even born. But that particular paper doesn’t mention anything about isolation. This is understandable, if we think of the production systems of the late 70’s, which according to Jim Gray:
</p>

<blockquote style="color: #444444;">
  <p style="font-weight: inherit; font-style: inherit;">
    “At present, the largest airlines and banks have about 10,000 terminals and about 100 active transactions at any instant”.
  </p>
</blockquote>

<p style="color: #444444; text-align: justify;">
  So all efforts were spent for delivering consistency rather than concurrency. Things have changed drastically ever since, and nowadays even modest set-ups are able to run 1000 TPS.
</p>

<p style="color: #444444; text-align: justify;">
  From a database perspective, the atomicity is a fixed property, but everything else may be traded-off for performance/scalability reasons.
</p>

<p style="color: #444444; text-align: justify;">
  Playing with durability makes sense for <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://wiki.postgresql.org/images/3/3b/2011-11-11_PostgreSQL_SyncRepPerformance.pdf">highly performing clustered databases </a>if the enterprise system business requirements don’t mandate durable transactions. But, most often durability is better off untouched.
</p>

<p style="color: #444444; text-align: justify;">
  Although some database management systems offer <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Multiversion_concurrency_control">MVCC</a>, usually concurrency control is achieved through locking. But as we all know, locking increases the serializable portion of the executed code, affecting <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Amdahl%27s_law#Parallelization">parallelization</a>.
</p>

<p style="color: #444444;">
  The SQL standard defines four Isolation levels:
</p>

<ul style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    READ_UNCOMMITTED
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    READ_COMMITTED
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    REPEATABLE_READ
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    SERIALIZABLE
  </li>
</ul>

<p style="color: #444444;">
  All but the SERIALIZABLE level are subject to data anomalies (phenomena) that might occur according to the following pattern:
</p>

<table style="color: #444444;">
  <tr style="font-weight: inherit; font-style: inherit;">
    <th style="font-style: inherit;">
      ISOLATION LEVEL
    </th>
    
    <th style="font-style: inherit;">
      DIRTY READ
    </th>
    
    <th style="font-style: inherit;">
      NON-REPEATABLE READ
    </th>
    
    <th style="font-style: inherit;">
      PHANTOM READ
    </th>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      READ_UNCOMMITTED
    </td>
    
    <td style="font-style: inherit;">
      allowed
    </td>
    
    <td style="font-style: inherit;">
      allowed
    </td>
    
    <td style="font-style: inherit;">
      allowed
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      READ_COMMITTED
    </td>
    
    <td style="font-style: inherit;">
      prevented
    </td>
    
    <td style="font-style: inherit;">
      allowed
    </td>
    
    <td style="font-style: inherit;">
      allowed
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      REPEATABLE_READ
    </td>
    
    <td style="font-style: inherit;">
      prevented
    </td>
    
    <td style="font-style: inherit;">
      prevented
    </td>
    
    <td style="font-style: inherit;">
      allowed
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      SERIALIZABLE
    </td>
    
    <td style="font-style: inherit;">
      prevented
    </td>
    
    <td style="font-style: inherit;">
      prevented
    </td>
    
    <td style="font-style: inherit;">
      prevented
    </td>
  </tr>
</table>

<p style="color: #444444;">
  But what are all those phenomena we’d just listed. Let’s discuss each and every one of them.
</p>

<p style="color: #444444;">
  1. Dirty read
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/01/acid-dirty-read.gif"><img class="alignnone size-medium wp-image-1265" src="http://vladmihalcea.files.wordpress.com/2014/01/acid-dirty-read.gif?w=450&h=428" alt="ACID-dirty-read" width="300" height="284" /></a>
</p>

<p style="color: #444444; text-align: justify;">
  A dirty read happens when a transaction is allowed to read uncommitted changes of some other running transaction. This happens because there is no locking preventing it. In the picture above, you can see that the second transaction uses an inconsistent value as of the first transaction had rollbacked.
</p>

<p style="color: #444444;">
  2. Non-repeatable read
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/01/acid-non-repeatable-read.gif"><img class="alignnone size-medium wp-image-1799" src="http://vladmihalcea.files.wordpress.com/2014/01/acid-non-repeatable-read.gif?w=450&h=411" alt="ACID-non-repeatable-read" width="300" height="273" /></a>
</p>

<p style="color: #444444; text-align: justify;">
  A non-repeatable read manifests when consecutive reads yield different results due to a concurring transaction that has just updated the record we’re reading. This is undesirable since we end up using stale data. This is prevented by holding a shared lock (read lock) on the read record for the whole duration of the current transaction.
</p>

<p style="color: #444444;">
  3. Phantom read
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/01/acid-phantom-read.gif"><img class="alignnone size-medium wp-image-1267" src="http://vladmihalcea.files.wordpress.com/2014/01/acid-phantom-read.gif?w=450&h=428" alt="ACID-phantom-read" width="300" height="285" /></a>
</p>

<p style="color: #444444; text-align: justify;">
  A phantom read happens when a second transaction inserts a row that matches a previously select criteria of the first transaction. We therefore end up using stale data, which might affect our business operation. This is prevented using range locks or <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://research.microsoft.com/en-us/um/people/gray/papers/On%20the%20Notions%20of%20Consistency%20and%20Predicate%20Locks%20in%20a%20Database%20System%20CACM.pdf">predicate locking</a>.
</p>

<p style="color: #444444; text-align: justify;">
  Even if the SOL standard mandates the use of the SERIALIZABLE isolation level, most database management system use a different default level.
</p>

<table style="color: #444444;">
  <tr style="font-weight: inherit; font-style: inherit;">
    <th style="font-style: inherit;">
      DATABASE
    </th>
    
    <th style="font-style: inherit;">
      DEFAULT ISOLATION LEVEL
    </th>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      Oracle
    </td>
    
    <td style="font-style: inherit;">
      READ_COMMITTED
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      MySQL
    </td>
    
    <td style="font-style: inherit;">
      REPEATABLE_READ
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      Microsoft SQL Server
    </td>
    
    <td style="font-style: inherit;">
      READ_COMMITTED
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      PostgreSQL
    </td>
    
    <td style="font-style: inherit;">
      READ_COMMITTED
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      DB2
    </td>
    
    <td style="font-style: inherit;">
      CURSOR STABILITY (a.k.a READ_COMMITTED)
    </td>
  </tr>
</table>

<p style="color: #444444; text-align: justify;">
  Usually, READ_COMMITED is the right choice, since not even SERIALIZABLE can protect you from a “lost update” where the read/write happen in different transactions (and web requests). You should take into consideration your enterprise system requirements and set up tests for deciding which isolation level best suits your needs.
</p>

<p style="color: #444444;">
  If you are interested in learning more about Transactions you can follow me on my blog, or on <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://twitter.com/vlad_mihalcea">twitter</a> since all this material will hopefully materialize in my <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/01/05/a-beginners-guide-to-acid-and-database-transactions/2014/01/01/my-open-book-movement/">open-source book</a>.
</p>

Published on Codingpedia.org with permission of Vlad Mihalcea – source <a title="A beginner's guide to ACID and database transactions" href="A%20beginner's guide to ACID and database transactions" target="_blank">A beginner&#8217;s guide to ACID and database transactions</a> from <a style="color: #bc360a;" title="http://vladmihalcea.com/" href="http://vladmihalcea.com/" target="_blank">http://vladmihalcea.com/</a> 

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

<p style="text-align: center;">
  “Image courtesy of ddpavumba/ FreeDigitalPhotos.net”
</p>
