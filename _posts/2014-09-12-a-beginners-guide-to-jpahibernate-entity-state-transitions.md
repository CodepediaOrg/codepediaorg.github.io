---
id: 1793
title: 'A beginner&#8217;s guide to JPA/Hibernate entity state transitions'
date: 2014-09-12T06:39:29+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=1793
permalink: /vladmihalcea/a-beginners-guide-to-jpahibernate-entity-state-transitions/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3008574305
fsb_social_facebook:
  - 1
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
  - beginner
  - entity state
  - hibernate
  - Hibernate training
  - jpa
---
<h2 style="font-weight: inherit; color: #444444;">
  Introduction
</h2>

<p style="color: #444444; text-align: justify;">
  Hibernate shifts the developer mindset from SQL statements to entity state transitions. Once an entity is actively managed by Hibernate, all changes are going to be automatically propagated to the database.
</p>

<p style="color: #444444; text-align: justify;">
  Manipulating domain model entities (along with their associations) is much easier than writing and maintaining SQL statements. Without an <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://en.wikipedia.org/wiki/Object-relational_mapping">ORM</a> tool, adding a new column requires modifying all associated INSERT/UPDATE statements. <!--more-->
</p>

<p style="color: #444444; text-align: justify;">
  But Hibernate is no silver bullet either. Hibernate doesn&#8217;t free us from ever worrying about the actual executed SQL statements. Controlling Hibernate is not as straightforward as one might think and it’s mandatory to <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/2013/12/10/hibernate-facts-always-check-criteria-api-sql-queries/">check all SQL statements</a> Hibernate executes on our behalf.
</p>

<h2 style="font-weight: inherit; color: #444444;">
  The entity states
</h2>

<p style="color: #444444;">
  As I previously mentioned, Hibernate monitors currently attached entities. But for an entity to become managed, it must be in the right entity state.
</p>

<p style="color: #444444;">
  First we must define all entity states:
</p>

<ul style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    <strong style="font-style: inherit;">New (Transient)</strong> <p style="font-weight: inherit; font-style: inherit; text-align: justify;">
      A newly created object that hasn’t ever been associated with a Hibernate Session (a.k.a Persistence Context) and is not mapped to any database table row is considered to be in the <strong style="font-style: inherit;">New (Transient)</strong> state.
    </p>
    
    <p style="font-weight: inherit; font-style: inherit;">
      To become persisted we need to either explicitly call the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#persist%28java.lang.Object%29">EntityManager#persist</a> method or make use of the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch11.html#objectstate-transitive">transitive persistence</a> mechanism.
    </p>
  </li>
  
  <li style="font-weight: inherit; font-style: inherit;">
    <strong style="font-style: inherit;">Persistent (Managed)</strong> <p style="font-weight: inherit; font-style: inherit; text-align: justify;">
      A <strong style="font-style: inherit;">persistent</strong> entity has been associated with a database table row and it’s being managed by the current running Persistence Context. Any change made to such entity is going to be detected and propagated to the database (during the Session flush-time). With Hibernate, we no longer have to execute INSERT/UPDATE/DELETE statements. Hibernate employs a “transactional write-behind” working style and changes are synchronized at the very last responsible moment, during the current Session flush-time.
    </p>
  </li>
  
  <li style="font-weight: inherit; font-style: inherit;">
    <strong style="font-style: inherit;">Detached</strong> <p style="font-weight: inherit; font-style: inherit; text-align: justify;">
      Once the current running Persistence Context is closed all the previously managed entities become <strong style="font-style: inherit;">detached</strong>. Successive changes will no longer be tracked and no automatic database synchronization is going to happen.
    </p>
    
    <p style="font-weight: inherit; font-style: inherit;">
      To associate a <strong style="font-style: inherit;">detached</strong> entity to an active Hibernate Session, you can choose one of the following options:
    </p>
    
    <ul style="font-weight: inherit; font-style: inherit;">
      <li style="font-weight: inherit; font-style: inherit;">
        <strong style="font-style: inherit;">Reattaching</strong> <p style="font-weight: inherit; font-style: inherit;">
          Hibernate (but not JPA 2.1) supports reattaching through the <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#update%28java.lang.Object%29">Session#update</a> method.
        </p>
        
        <p style="font-weight: inherit; font-style: inherit; text-align: justify;">
          A Hibernate Session can only associate one Entity object for a given database row. This is because the Persistence Context acts as an in-memory cache (first level cache) and only one value (entity) is associated to a given key (entity type and database identifier).
        </p>
        
        <p style="font-weight: inherit; font-style: inherit;">
          An entity can be reattached only if there is no other JVM object (matching the same database row) already associated to the current Hibernate Session.
        </p>
      </li>
      
      <li style="font-weight: inherit; font-style: inherit;">
        <strong style="font-style: inherit;">Merging</strong> <p style="font-weight: inherit; font-style: inherit; text-align: justify;">
          The merge is going to copy the <strong style="font-style: inherit;">detached</strong> entity state (source) to a managed entity instance (destination). If the merging entity has no equivalent in the current Session, one will be fetched from the database.
        </p>
        
        <p style="font-weight: inherit; font-style: inherit;">
          The <strong style="font-style: inherit;">detached</strong> object instance will continue to remain detached even after the merge operation.
        </p>
      </li>
    </ul>
  </li>
  
  <li style="font-weight: inherit; font-style: inherit;">
    <strong style="font-style: inherit;">Removed</strong> <p style="font-weight: inherit; font-style: inherit;">
      Although JPA demands that <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#remove%28java.lang.Object%29">managed entities only</a> are allowed to be removed, Hibernate can also <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.jboss.org/hibernate/core/4.3/devguide/en-US/html/ch03.html#d5e824">delete detached entities</a> (but only through a <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.jboss.org/hibernate/core/4.3/javadocs/org/hibernate/Session.html#delete%28java.lang.Object%29">Session#delete</a> method call).
    </p>
    
    <p style="font-weight: inherit; font-style: inherit;">
      A removed entity is only scheduled for deletion and the actual database DELETE statement will be executed during Session flush-time.
    </p>
  </li>
</ul>

<h2 style="font-weight: inherit; color: #444444;">
  Entity state transitions
</h2>

<p style="color: #444444;">
  To change one Entity state, we need to use one of the following entity management interfaces:
</p>

<ul style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html">EntityManager</a> <p style="font-weight: inherit; font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://vladmihalcea.files.wordpress.com/2014/07/jpaentitystates.png"><img class="alignnone size-medium wp-image-2876" src="http://vladmihalcea.files.wordpress.com/2014/07/jpaentitystates.png?w=450&h=287" alt="JPAEntityStates" width="300" height="190" /></a>
    </p>
  </li>
  
  <li style="font-weight: inherit; font-style: inherit;">
    <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://docs.jboss.org/hibernate/core/4.3/javadocs/org/hibernate/Session.html">Session</a> <p style="font-weight: inherit; font-style: inherit;">
      <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://vladmihalcea.files.wordpress.com/2014/07/hibernateentitystates1.png"><img class="alignnone size-medium wp-image-2883" src="http://vladmihalcea.files.wordpress.com/2014/07/hibernateentitystates1.png?w=450&h=284" alt="HibernateEntityStates" width="300" height="188" /></a>
    </p>
  </li>
</ul>

<p style="color: #444444; text-align: justify;">
  These interfaces define the entity state transition operations we must explicitly call to notify Hibernate of the entity state change. At flush-time the entity state transition is materialized into a database SQL statement (INSERT/UPDATE/DELETE).
</p>

<p style="color: #444444;">
  <strong style="font-style: inherit;">If you have enjoyed reading my article and you’re looking forward to getting instant email notifications of my latest posts, you just need to <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/follow-me/">follow my blog</a>.</strong>
</p>

<p class="note_normal" style="color: #444444;">
  Published at Codingpedia.org with permission of <a title="http://www.codingpedia.org/author/vladmihalcea" href="http://www.codingpedia.org/author/vladmihalcea" target="_blank">Vlad Mihalcea</a> &#8211; source <a title="http://vladmihalcea.com/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/" href="http://vladmihalcea.com/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/" target="_blank">A BEGINNER’S GUIDE TO JPA/HIBERNATE ENTITY STATE TRANSITIONS</a> from <a title="http://vladmihalcea.com/" href="http://vladmihalcea.com/" target="_blank">http://vladmihalcea.com/</a>
</p>

<p style="color: #444444;">
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
