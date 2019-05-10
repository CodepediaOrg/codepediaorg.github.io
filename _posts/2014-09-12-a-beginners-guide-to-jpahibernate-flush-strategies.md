---
id: 1810
title: 'A beginner&#8217;s guide to JPA/Hibernate flush strategies'
date: 2014-09-12T06:51:39+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=1810
permalink: /vladmihalcea/a-beginners-guide-to-jpahibernate-flush-strategies/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3010427831
fsb_social_facebook:
  - 2
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - hibernate
tags:
  - entity manager flush
  - hibernate
  - Hibernate training
  - jpa
  - Session flush
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Introduction">Introduction</a>
    </li>
    <li>
      <a href="#Write-behind">Write-behind</a><ul>
        <li>
          <a href="#Reducing_lock_contention">Reducing lock contention</a>
        </li>
        <li>
          <a href="#Batching">Batching</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Read-your-own-writes_consistency">Read-your-own-writes consistency</a>
    </li>
    <li>
      <a href="#From_JPA_to_Hibernate_flushing_strategies">From JPA to Hibernate flushing strategies</a>
    </li>
    <li>
      <a href="#Current_Flush_scope">Current Flush scope</a>
    </li>
    <li>
      <a href="#Stay_tuned">Stay tuned</a>
    </li>
  </ul>
</div>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Introduction">Introduction</span>
</h2>

<p style="color: #444444; text-align: justify;">
  In my <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/">previous post</a> I introduced the <strong style="font-style: inherit;">entity state transitions</strong> <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" title="http://en.wikipedia.org/wiki/Object-relational_mapping" href="http://en.wikipedia.org/wiki/Object-relational_mapping">Object-relational mapping</a> paradigm.
</p>

<p style="color: #444444; text-align: justify;">
  All managed entity state transitions are translated to associated database statements when the current Persistence Context gets flushed. Hibernate’s flush behavior is not always as obvious as one might think.<!--more-->
</p>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Write-behind">Write-behind</span>
</h2>

<p style="color: #444444; text-align: justify;">
  Hibernate tries to defer the Persistence Context flushing up until the last possible moment. This strategy has been traditionally known as <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.jboss.org/hibernate/orm/4.3/devguide/en-US/html/ch02.html#d5e720"><strong style="font-style: inherit;">transactional write-behind</strong></a>.
</p>

<p style="color: #444444; text-align: justify;">
  The write-behind is more related to Hibernate flushing rather than any logical or physical transaction. During a transaction, the flush may occur multiple times.
</p>

<p style="color: #444444; text-align: justify;">
  The flushed changes are visible only for the current database transaction. Until the current transaction is committed, no change is visible by <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/2014/01/05/a-beginners-guide-to-acid-and-database-transactions">other concurrent transactions</a>.
</p>

<p style="color: #444444; text-align: justify;">
  The persistence context, also known as the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch02.html#architecture-overview-terms">first level cache</a>, acts as a buffer between the current entity state transitions and the database.
</p>

<p style="color: #444444; text-align: justify;">
  In <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Write-behind#WRITE-BEHIND">caching theory</a>, the <strong style="font-style: inherit;">write-behind</strong> synchronization requires that all changes happen against the cache, whose responsibility is to eventually synchronize with the backing store.
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Reducing_lock_contention">Reducing lock contention</span>
</h3>

<p style="color: #444444; text-align: justify;">
  Every <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Data_manipulation_language">DML statement</a> runs inside a database transaction. Based on the current database <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/2014/01/05/a-beginners-guide-to-acid-and-database-transactions/">transaction isolation level</a>, locks (shared or explicit) may be acquired for the current selected/modified table rows.
</p>

<p style="color: #444444; text-align: justify;">
  Reducing the lock holding holding time lowers the dead-lock probability, and according to the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/2014/05/20/the-simple-scalability-equation/">scalability theory</a>, it increases throughput. Locks always introduce serial executions, and according to <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Amdahl%27s_law">Amdahl’s law</a>, the maximum speedup is inversely proportional with the serial part of the currently executing program.
</p>

<p style="color: #444444; text-align: justify;">
  Even in <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Isolation_%28database_systems%29#Read_committed">READ_COMMITTED</a> isolation level, UPDATE and DELETE statements acquire locks. This behavior prevents other concurring transactions from reading uncommitted changes or modify the rows in question.
</p>

<p style="color: #444444; text-align: justify;">
  So, deferring locking statements (UPDATE/DELETE) may increase performance, but we must make sure that data consistency is not affected whatsoever.
</p>

<h3 style="font-weight: inherit; color: #444444;">
  <span id="Batching">Batching</span>
</h3>

<p style="color: #444444;">
  Postponing the entity state transition synchronization has another major advantage. Since all changes are being flushed at once, Hibernate may benefit from the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://access.redhat.com/documentation/en-US/JBoss_Enterprise_Application_Platform/5/html/Performance_Tuning_Guide/sect-Performance_Tuning_Guide-Entity_Beans-Batching_Database_Operations.html">JDBC batching optimization</a>.
</p>

<p style="color: #444444;">
  Batching improves performance by grouping multiple DML statements into a single operation, therefore reducing database round-trips.
</p>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Read-your-own-writes_consistency">Read-your-own-writes consistency</span>
</h2>

<p style="color: #444444; text-align: justify;">
  Since queries are always running against the database (unless second level query cache is being hit), we need to make sure that all pending changes are synchronized before the query starts running.
</p>

<p style="color: #444444; text-align: justify;">
  Therefore, both JPA and Hibernate define a <strong style="font-style: inherit;">flush-before-query</strong> synchronization strategy.
</p>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="From_JPA_to_Hibernate_flushing_strategies">From JPA to Hibernate flushing strategies</span>
</h2>

<table style="color: #444444;">
  <tr style="font-weight: inherit; font-style: inherit;">
    <th style="font-style: inherit;">
      JPA <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/FlushModeType.html">FLUSHMODETYPE</a>
    </th>
    
    <th style="font-style: inherit;">
      HIBERNATE <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html">FLUSHMODE</a>
    </th>
    
    <th style="font-style: inherit;">
      HIBERNATE IMPLEMENTATION DETAILS
    </th>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/FlushModeType.html#AUTO">AUTO</a>
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html#AUTO">AUTO</a>
    </td>
    
    <td style="font-style: inherit;">
      The Session is <strong style="font-style: inherit;">sometimes</strong> flushed before query execution.
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/FlushModeType.html#COMMIT">COMMIT</a>
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html#COMMIT">COMMIT</a>
    </td>
    
    <td style="font-style: inherit;">
      The Session is <strong style="font-style: inherit;">only</strong> flushed prior to a transaction commit.
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html#ALWAYS">ALWAYS</a>
    </td>
    
    <td style="font-style: inherit;">
      The Session is <strong style="font-style: inherit;">always</strong> flushed before query execution.
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html#MANUAL">MANUAL</a>
    </td>
    
    <td style="font-style: inherit;">
      The Session can <strong style="font-style: inherit;">only</strong> be <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#flush%28%29">manually flushed</a>.
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
    </td>
    
    <td style="font-style: inherit;">
      <del style="font-weight: inherit; font-style: inherit;" datetime="2014-08-01T20:22:39+00:00"><a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html#NEVER">NEVER</a></del>
    </td>
    
    <td style="font-style: inherit;">
      Deprecated. Use MANUAL instead. This was the original name given to manual flushing, but it was misleading users into thinking that the Session won’t ever be flushed.
    </td>
  </tr>
</table>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Current_Flush_scope">Current Flush scope</span>
</h2>

<p style="color: #444444; text-align: justify;">
  The Persistence Context defines a default flush mode, that can be overridden upon Hibernate Session creation. Queries can also take a flush strategy, therefore overruling the current Persistence Context flush mode.
</p>

<table style="color: #444444;">
  <tr style="font-weight: inherit; font-style: inherit;">
    <th style="font-style: inherit;">
      SCOPE
    </th>
    
    <th style="font-style: inherit;">
      HIBERNATE
    </th>
    
    <th style="font-style: inherit;">
      JPA
    </th>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      Persistence Context
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/Session.html#setFlushMode%28org.hibernate.FlushMode%29">Session</a>
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#setFlushMode%28javax.persistence.FlushModeType%29">EntityManager</a>
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      Query
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/Query.html#setFlushMode%28org.hibernate.FlushMode%29">Query</a><br /> <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/Criteria.html#setFlushMode%28org.hibernate.FlushMode%29">Criteria</a>
    </td>
    
    <td style="font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/Query.html#setFlushMode%28javax.persistence.FlushModeType%29">Query</a><br /> <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/TypedQuery.html#setFlushMode%28javax.persistence.FlushModeType%29">TypedQuery</a>
    </td>
  </tr>
</table>

<h2 style="font-weight: inherit; color: #444444;">
  <span id="Stay_tuned">Stay tuned</span>
</h2>

<p style="color: #444444; text-align: justify;">
  In my next post, you’ll find out that <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/FlushMode.html#AUTO">Hibernate FlushMode.AUTO</a> breaks data consistency for SQL queries and you’ll see how you can overcome this shortcoming.
</p>

<p style="color: #444444; text-align: justify;">
  <strong style="font-style: inherit;">If you have enjoyed reading my article and you’re looking forward to getting instant email notifications of my latest posts, you just need to <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/follow-me/">follow my blog</a>.</strong>
</p>

<p class="note_normal" style="color: #444444; text-align: justify;">
  Published at Codingpedia.org with permission of Vlad Mihalcea &#8211; source <a title="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/" href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/" target="_blank">A BEGINNER’S GUIDE TO JPA/HIBERNATE FLUSH STRATEGIES</a> from <a title="http://vladmihalcea.com/" href="http://vladmihalcea.com/" target="_blank">http://vladmihalcea.com/</a>
</p>

<p style="color: #444444; text-align: justify;">
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
</p>
