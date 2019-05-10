---
id: 2324
title: A beginner’s guide to JPA and Hibernate Cascade Types
date: 2015-03-06T20:21:52+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=2324
permalink: /vladmihalcea/a-beginners-guide-to-jpa-and-hibernate-cascade-types/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3573547090
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/vladmihalcea/hibernate-master-class";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 2
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 2
fsb_social_pinterest:
  - 0
categories:
  - hibernate
  - java
  - Java EE
tags:
  - cascade
  - CascadeType
  - hibernate
  - java
  - java ee
  - jpa
---
## <span id="Introduction">Introduction</span>

[JPA](http://en.wikipedia.org/wiki/Java_Persistence_API) translates [entity state transitions](http://vladmihalcea.com/2015/03/05/a-beginners-guide-to-jpa-and-hibernate-cascade-types/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/) to database [DML](http://en.wikipedia.org/wiki/Data_manipulation_language) statements. Because it’s common to operate on entity graphs, JPA allows us to propagate entity state changes from _Parents_ to _Child_ entities.

This behavior is configured through the [CascadeType](http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html) mappings.

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Introduction">Introduction</a>
    </li>
    <li>
      <a href="#JPA_vs_Hibernate_Cascade_Types">JPA vs Hibernate Cascade Types</a>
    </li>
    <li>
      <a href="#Cascading_best_practices">Cascading best practices</a><ul>
        <li>
          <a href="#One-To-One">One-To-One</a>
        </li>
        <li>
          <a href="#Cascading_the_one-to-one_persist_operation">Cascading the one-to-one persist operation</a>
        </li>
        <li>
          <a href="#Cascading_the_one-to-one_merge_operation">Cascading the one-to-one merge operation</a>
        </li>
        <li>
          <a href="#Cascading_the_one-to-one_delete_operation">Cascading the one-to-one delete operation</a>
        </li>
        <li>
          <a href="#The_one-to-one_delete_orphan_cascading_operation">The one-to-one delete orphan cascading operation</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Unidirectional_one-to-one_association">Unidirectional one-to-one association</a>
    </li>
    <li>
      <a href="#One-To-Many">One-To-Many</a><ul>
        <li>
          <a href="#Cascading_the_one-to-many_persist_operation">Cascading the one-to-many persist operation</a>
        </li>
        <li>
          <a href="#Cascading_the_one-to-many_merge_operation">Cascading the one-to-many merge operation</a>
        </li>
        <li>
          <a href="#Cascading_the_one-to-many_delete_operation">Cascading the one-to-many delete operation</a>
        </li>
        <li>
          <a href="#The_one-to-many_delete_orphan_cascading_operation">The one-to-many delete orphan cascading operation</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Many-To-Many">Many-To-Many</a><ul>
        <li>
          <a href="#Cascading_the_many-to-many_persist_operation">Cascading the many-to-many persist operation</a>
        </li>
        <li>
          <a href="#Dissociating_one_side_of_the_many-to-many_association">Dissociating one side of the many-to-many association</a>
        </li>
        <li>
          <a href="#The_many-to-many_CascadeTypeREMOVE_gotchas">The many-to-many CascadeType.REMOVE gotchas</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Conclusion">Conclusion</a>
    </li>
  </ul>
</div>

<!--more-->

## <span id="JPA_vs_Hibernate_Cascade_Types">JPA vs Hibernate Cascade Types</span>

Hibernate supports all JPA Cascade Types and some additional legacy cascading styles. The following table draws an association between JPA Cascade Types and their Hibernate native API equivalent:

<table>
  <tr>
    <th>
      JPA EntityManager action
    </th>
    
    <th>
      JPA CascadeType
    </th>
    
    <th>
      Hibernate native Session action
    </th>
    
    <th>
      Hibernate native CascadeType
    </th>
    
    <th>
      Event Listener
    </th>
  </tr>
  
  <tr>
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#detach%28java.lang.Object%29">detach(entity)</a>
    </td>
    
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#DETACH">DETACH</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#evict%28java.lang.Object%29">evict(entity)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#DETACH">DETACH</a> or<a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#EVICT"><del>EVICT</del></a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultEvictEventListener.html">Default Evict Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#merge%28T%29">merge(entity)</a>
    </td>
    
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#MERGE">MERGE</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#merge%28java.lang.Object%29">merge(entity)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#MERGE">MERGE</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultMergeEventListener.html">Default Merge Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#persist%28java.lang.Object%29">persist(entity)</a>
    </td>
    
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#PERSIST">PERSIST</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#persist%28java.lang.Object%29">persist(entity)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#PERSIST">PERSIST</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultPersistEventListener.html">Default Persist Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#refresh%28java.lang.Object%29">refresh(entity)</a>
    </td>
    
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#REFRESH">REFRESH</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#refresh%28java.lang.Object%29">refresh(entity)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#REFRESH">REFRESH</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultRefreshEventListener.html">Default Refresh Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#remove%28java.lang.Object%29">remove(entity)</a>
    </td>
    
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#REMOVE">REMOVE</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#delete%28java.lang.Object%29">delete(entity)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#REMOVE">REMOVE</a> or<a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#DELETE">DELETE</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultDeleteEventListener.html">Default Delete Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#saveOrUpdate%28java.lang.Object%29">saveOrUpdate(entity)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#SAVE_UPDATE">SAVE_UPDATE</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultSaveOrUpdateEventListener.html">Default Save Or Update Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#replicate%28java.lang.Object,%20org.hibernate.ReplicationMode%29">replicate(entity, replicationMode)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#REPLICATE">REPLICATE</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultReplicateEventListener.html">Default Replicate Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html#lock%28java.lang.Object,%20javax.persistence.LockModeType%29">lock(entity, lockModeType)</a>
    </td>
    
    <td>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html#buildLockRequest%28org.hibernate.LockOptions%29">buildLockRequest(entity, lockOptions)</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#LOCK">LOCK</a>
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/event/internal/DefaultLockEventListener.html">Default Lock Event Listener</a>
    </td>
  </tr>
  
  <tr>
    <td>
      All the above EntityManager methods
    </td>
    
    <td>
      <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#ALL">ALL</a>
    </td>
    
    <td>
      All the above Hibernate Session methods
    </td>
    
    <td>
      <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html#ALL">ALL</a>
    </td>
    
    <td>
    </td>
  </tr>
</table>

From this table we can conclude that:

<li style="text-align: justify;">
  There’s no difference between calling <em>persist</em>, <em>merge</em> or <em>refresh</em> on the JPA <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html">EntityManager</a> or the Hibernate <a href="https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/Session.html">Session</a>.
</li>
<li style="text-align: justify;">
  The JPA <em>remove</em> and <em>detach</em> calls are delegated to Hibernate <em>delete</em> and <em>evict</em> native operations.
</li>
<li style="text-align: justify;">
  Only Hibernate supports <em>replicate</em> and <em>saveOrUpdate</em>. While <em>replicate</em> is useful for some very specific scenarios (when the exact entity state needs to be mirrored between two distinct DataSources), the <em>persist</em> and <em>merge</em> combo is always a better alternative than the native <em>saveOrUpdate</em> operation. As a rule of thumb, you should always use <em>persist</em> for TRANSIENT entities and merge for DETACHED ones. The <em>saveOrUpdate</em> shortcomings (when passing a detached entity snapshot to a <em>Session</em> already managing this entity) had lead to the <em>merge</em> operation predecessor: the now extinct <a href="https://docs.jboss.org/hibernate/orm/3.2/api/org/hibernate/classic/Session.html#saveOrUpdateCopy%28java.lang.Object%29">saveOrUpdateCopy</a> operation.
</li>
<li style="text-align: justify;">
  The JPA lock method shares the same behavior with Hibernate lock request method.
</li>
<li style="text-align: justify;">
  The JPA <a href="http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html#ALL">CascadeType.ALL</a> doesn’t only apply to <em>EntityManager</em> state change operations, but to all Hibernate <a href="https://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/annotations/CascadeType.html">CascadeTypes</a> as well. So if you mapped your associations with <em>CascadeType.ALL</em>, you can still cascade Hibernate specific events. For example, you can cascade the JPA lock operation (although it behaves as reattaching, instead of an actual lock request propagation), even if JPA doesn’t define a LOCK <em>CascadeType</em>.
</li>

## <span id="Cascading_best_practices">Cascading best practices</span>

<p style="text-align: justify;">
  Cascading only makes sense only for <em>Parent</em> – <em>Child</em> associations (the <em>Parent</em> entity state transition being cascaded to its Child entities). Cascading from <em>Child</em> to <em>Parent</em> is not very useful and usually, it’s a mapping code smell.
</p>

Next, I’m going to take analyse the cascading behaviour of all JPA _Parent_ – _Child_ associations.

### <span id="One-To-One">One-To-One</span>

The most common One-To-One bidirectional association looks like this:

<pre class="lang:java decode:true">@Entity
public class Post {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    private String name;
 
    @OneToOne(mappedBy = "post",
        cascade = CascadeType.ALL, orphanRemoval = true)
    private PostDetails details;
 
    public Long getId() {
        return id;
    }
 
    public PostDetails getDetails() {
        return details;
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public void addDetails(PostDetails details) {
        this.details = details;
        details.setPost(this);
    }
 
    public void removeDetails() {
        if (details != null) {
            details.setPost(null);
        }
        this.details = null;
    }
}
 
@Entity
public class PostDetails {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    @Column(name = "created_on")
    @Temporal(TemporalType.TIMESTAMP)
    private Date createdOn = new Date();
 
    private boolean visible;
 
    @OneToOne
    @PrimaryKeyJoinColumn
    private Post post;
 
    public Long getId() {
        return id;
    }
 
    public void setVisible(boolean visible) {
        this.visible = visible;
    }
 
    public void setPost(Post post) {
        this.post = post;
    }
}</pre>

The _Post_ entity plays the _Parent_ role and the _PostDetails_ is the _Child_.

> The bidirectional associations should always be updated on both sides, therefore the _Parent_ side should contain the _addChild_ and _removeChild_ combo. These methods ensure we always synchronize both sides of the association, to avoid Object or Relational data corruption issues.

In this particular case, the _CascadeType.ALL_ and orphan removal make sense because the _PostDetails_ life-cycle is bound to that of its _Post_ _Parent_ entity.

### <span id="Cascading_the_one-to-one_persist_operation">Cascading the one-to-one persist operation</span>

The _CascadeType.PERSIST_ comes along with the _CascadeType.ALL_ configuration, so we only have to persist the _Post_ entity, and the associated _PostDetails_ entity is persisted as well:

<pre class="lang:java decode:true ">Post post = new Post();
post.setName("Hibernate Master Class");
 
PostDetails details = new PostDetails();
 
post.addDetails(details);
 
session.persist(post);</pre>

Generating the following output:

<pre class="lang:tsql decode:true">INSERT INTO post(id, NAME) 
VALUES (DEFAULT, Hibernate Master Class'')
 
insert into PostDetails (id, created_on, visible) 
values (default, '2015-03-03 10:17:19.14', false)</pre>

### <span id="Cascading_the_one-to-one_merge_operation">Cascading the one-to-one merge operation</span>

The _CascadeType.MERGE_ is inherited from the _CascadeType.ALL_ setting, so we only have to merge the _Post_ entity and the associated _PostDetails_ is merged as well:

<pre class="lang:java decode:true">Post post = newPost();
post.setName("Hibernate Master Class Training Material");
post.getDetails().setVisible(true);
 
doInTransaction(session -&gt; {
    session.merge(post);
});</pre>

The merge operation generates the following output:

<pre class="lang:tsql decode:true ">SELECT onetooneca0_.id     AS id1_3_1_,
   onetooneca0_.NAME       AS name2_3_1_,
   onetooneca1_.id         AS id1_4_0_,
   onetooneca1_.created_on AS created_2_4_0_,
   onetooneca1_.visible    AS visible3_4_0_
FROM   post onetooneca0_
LEFT OUTER JOIN postdetails onetooneca1_ 
    ON onetooneca0_.id = onetooneca1_.id
WHERE  onetooneca0_.id = 1
 
UPDATE postdetails SET
    created_on = '2015-03-03 10:20:53.874', visible = true
WHERE  id = 1
 
UPDATE post SET
    NAME = 'Hibernate Master Class Training Material'
WHERE  id = 1</pre>

### <span id="Cascading_the_one-to-one_delete_operation">Cascading the one-to-one delete operation</span>

The _CascadeType.REMOVE_ is also inherited from the _CascadeType.ALL_ configuration, so the _Post_ entity deletion triggers a _PostDetails_ entity removal too:

<pre class="lang:java decode:true ">Post post = newPost();
 
doInTransaction(session -&gt; {
    session.delete(post);
});</pre>

Generating the following output:

<pre class="lang:tsql decode:true ">delete from PostDetails where id = 1
delete from Post where id = 1</pre>

### <span id="The_one-to-one_delete_orphan_cascading_operation">The one-to-one delete orphan cascading operation</span>

If a _Child_ entity is dissociated from its _Parent_, the Child Foreign Key is set to NULL. If we want to have the _Child_ row deleted as well, we have to use the _orphan removal _support.

<pre class="lang:java decode:true ">doInTransaction(session -&gt; {
    Post post = (Post) session.get(Post.class, 1L);
    post.removeDetails();
});</pre>

The _orphan removal_ generates this output:

<pre class="lang:tsql decode:true ">SELECT onetooneca0_.id         AS id1_3_0_,
       onetooneca0_.NAME       AS name2_3_0_,
       onetooneca1_.id         AS id1_4_1_,
       onetooneca1_.created_on AS created_2_4_1_,
       onetooneca1_.visible    AS visible3_4_1_
FROM   post onetooneca0_
LEFT OUTER JOIN postdetails onetooneca1_
    ON onetooneca0_.id = onetooneca1_.id
WHERE  onetooneca0_.id = 1
 
delete from PostDetails where id = 1</pre>

## <span id="Unidirectional_one-to-one_association">Unidirectional one-to-one association</span>

Most often, the _Parent_ entity is the inverse side (e.g. _mappedBy_), the _Child_ controling the association through its Foreign Key. But the cascade is not limited to bidirectional associations, we can also use it for unidirectional relationships:

<pre class="lang:java decode:true ">@Entity
public class Commit {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    private String comment;
 
    @OneToOne(cascade = CascadeType.ALL)
    @JoinTable(
        name = "Branch_Merge_Commit",
        joinColumns = @JoinColumn(
            name = "commit_id", 
            referencedColumnName = "id"),
        inverseJoinColumns = @JoinColumn(
            name = "branch_merge_id", 
            referencedColumnName = "id")
    )
    private BranchMerge branchMerge;
 
    public Commit() {
    }
 
    public Commit(String comment) {
        this.comment = comment;
    }
 
    public Long getId() {
        return id;
    }
 
    public void addBranchMerge(
        String fromBranch, String toBranch) {
        this.branchMerge = new BranchMerge(
             fromBranch, toBranch);
    }
 
    public void removeBranchMerge() {
        this.branchMerge = null;
    }
}
 
@Entity
public class BranchMerge {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    private String fromBranch;
 
    private String toBranch;
 
    public BranchMerge() {
    }
 
    public BranchMerge(
        String fromBranch, String toBranch) {
        this.fromBranch = fromBranch;
        this.toBranch = toBranch;
    }
 
    public Long getId() {
        return id;
    }
}</pre>

> <p style="text-align: justify;">
>   Cascading consists in propagating the <em>Parent</em> entity state transition to one or more <em>Child</em> entities, and it can be used for both unidirectional and bidirectional associations.
> </p>

## <span id="One-To-Many">One-To-Many</span>

The most common _Parent_ – _Child_ association consists of a one-to-many and a many-to-one relationship, where the cascade being useful for the one-to-many side only:

<pre class="lang:java decode:true ">@Entity
public class Post {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    private String name;
 
    @OneToMany(cascade = CascadeType.ALL, 
        mappedBy = "post", orphanRemoval = true)
    private List&lt;Comment&gt; comments = new ArrayList&lt;&gt;();
 
    public void setName(String name) {
        this.name = name;
    }
 
    public List&lt;Comment&gt; getComments() {
        return comments;
    }
 
    public void addComment(Comment comment) {
        comments.add(comment);
        comment.setPost(this);
    }
 
    public void removeComment(Comment comment) {
        comment.setPost(null);
        this.comments.remove(comment);
    }
}
 
@Entity
public class Comment {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    @ManyToOne
    private Post post;
 
    private String review;
 
    public void setPost(Post post) {
        this.post = post;
    }
 
    public String getReview() {
        return review;
    }
 
    public void setReview(String review) {
        this.review = review;
    }
}</pre>

Like in the one-to-one example, the _CascadeType.ALL_ and orphan removal are suitable because the _Comment_ life-cycle is bound to that of its _Post_ _Parent_ entity.

### <span id="Cascading_the_one-to-many_persist_operation">Cascading the one-to-many persist operation</span>

We only have to persist the _Post_ entity and all the associated _Comment_ entities are persisted as well:

<pre class="lang:java decode:true ">Post post = new Post();
post.setName("Hibernate Master Class");
 
Comment comment1 = new Comment();
comment1.setReview("Good post!");
Comment comment2 = new Comment();
comment2.setReview("Nice post!");
 
post.addComment(comment1);
post.addComment(comment2);
 
session.persist(post);</pre>

The persist operation generates the following output:

<pre class="lang:tsql decode:true ">insert into Post (id, name) 
values (default, 'Hibernate Master Class')
 
insert into Comment (id, post_id, review) 
values (default, 1, 'Good post!')
 
insert into Comment (id, post_id, review) 
values (default, 1, 'Nice post!')</pre>

### <span id="Cascading_the_one-to-many_merge_operation">Cascading the one-to-many merge operation</span>

Merging the _Post_ entity is going to merge all _Comment_ entities as well:

<pre class="lang:java decode:true ">Post post = newPost();
post.setName("Hibernate Master Class Training Material");
 
post.getComments()
    .stream()
    .filter(comment -&gt; comment.getReview().toLowerCase()
         .contains("nice"))
    .findAny()
    .ifPresent(comment -&gt; 
        comment.setReview("Keep up the good work!")
);
 
doInTransaction(session -&gt; {
    session.merge(post);
});</pre>

Generating the following output:

<pre class="lang:mysql decode:true">SELECT onetomanyc0_.id    AS id1_1_1_,
       onetomanyc0_.NAME  AS name2_1_1_,
       comments1_.post_id AS post_id3_1_3_,
       comments1_.id      AS id1_0_3_,
       comments1_.id      AS id1_0_0_,
       comments1_.post_id AS post_id3_0_0_,
       comments1_.review  AS review2_0_0_
FROM   post onetomanyc0_
LEFT OUTER JOIN comment comments1_
    ON onetomanyc0_.id = comments1_.post_id
WHERE  onetomanyc0_.id = 1
 
update Post set
    name = 'Hibernate Master Class Training Material'
where id = 1
 
update Comment set
    post_id = 1, 
    review='Keep up the good work!'
where id = 2</pre>

### <span id="Cascading_the_one-to-many_delete_operation">Cascading the one-to-many delete operation</span>

When the _Post_ entity is deleted, the associated _Comment_ entities are deleted as well:

<pre class="lang:java decode:true ">Post post = newPost();
 
doInTransaction(session -&gt; {
    session.delete(post);
});</pre>

Generating the following output:

<pre class="lang:tsql decode:true ">delete from Comment where id = 1
delete from Comment where id = 2
delete from Post where id = 1</pre>

### <span id="The_one-to-many_delete_orphan_cascading_operation">The one-to-many delete orphan cascading operation</span>

The orphan-removal allows us to remove the Child entity whenever it’s no longer referenced by its Parent:

<pre class="lang:java decode:true ">newPost();
 
doInTransaction(session -&gt; {
    Post post = (Post) session.createQuery(
        "select p " +
                "from Post p " +
                "join fetch p.comments " +
                "where p.id = :id")
        .setParameter("id", 1L)
        .uniqueResult();
    post.removeComment(post.getComments().get(0));
});</pre>

The Comment is deleted, as we can see in the following output:

<pre class="lang:tsql decode:true ">SELECT onetomanyc0_.id    AS id1_1_0_,
       comments1_.id      AS id1_0_1_,
       onetomanyc0_.NAME  AS name2_1_0_,
       comments1_.post_id AS post_id3_0_1_,
       comments1_.review  AS review2_0_1_,
       comments1_.post_id AS post_id3_1_0__,
       comments1_.id      AS id1_0_0__
FROM   post onetomanyc0_
INNER JOIN comment comments1_
    ON onetomanyc0_.id = comments1_.post_id
WHERE  onetomanyc0_.id = 1
 
delete from Comment where id = 1</pre>

## <span id="Many-To-Many">Many-To-Many</span>

The many-to-many relationship is tricky because each side of this association plays both the _Parent_ and the _Child_ role. Still, we can identify one side from where we’d like to propagate the entity state changes.

We shouldn’t default to _CascadeType.ALL_, because the CascadeTpe.REMOVE might end-up deleting more than we’re expecting (as you’ll soon find out):

<pre class="lang:java decode:true">@Entity
public class Author {
 
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
 
    @Column(name = "full_name", nullable = false)
    private String fullName;
 
    @ManyToMany(mappedBy = "authors", 
        cascade = {CascadeType.PERSIST, CascadeType.MERGE})
    private List&lt;Book&gt; books = new ArrayList&lt;&gt;();
 
    private Author() {}
 
    public Author(String fullName) {
        this.fullName = fullName;
    }
 
    public Long getId() {
        return id;
    }
 
    public void addBook(Book book) {
        books.add(book);
        book.authors.add(this);
    }
 
    public void removeBook(Book book) {
        books.remove(book);
        book.authors.remove(this);
    }
 
    public void remove() {
        for(Book book : new ArrayList&lt;&gt;(books)) {
            removeBook(book);
        }
    }
}
 
@Entity
public class Book {
 
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
 
    @Column(name = "title", nullable = false)
    private String title;
 
    @ManyToMany(cascade = 
        {CascadeType.PERSIST, CascadeType.MERGE})
    @JoinTable(name = "Book_Author",
        joinColumns = {
            @JoinColumn(
                name = "book_id", 
                referencedColumnName = "id"
            )
        },
        inverseJoinColumns = {
            @JoinColumn(
                name = "author_id", 
                referencedColumnName = "id"
            )
        }
    )
    private List&lt;Author&gt; authors = new ArrayList&lt;&gt;();
 
    private Book() {}
 
    public Book(String title) {
        this.title = title;
    }
}</pre>

### <span id="Cascading_the_many-to-many_persist_operation">Cascading the many-to-many persist operation</span>

Persisting the _Author_ entities will persist the _Books_ as well:

<pre class="lang:java decode:true">Author _John_Smith = new Author("John Smith");
Author _Michelle_Diangello = 
    new Author("Michelle Diangello");
Author _Mark_Armstrong = 
    new Author("Mark Armstrong");
 
Book _Day_Dreaming = new Book("Day Dreaming");
Book _Day_Dreaming_2nd = 
    new Book("Day Dreaming, Second Edition");
 
_John_Smith.addBook(_Day_Dreaming);
_Michelle_Diangello.addBook(_Day_Dreaming);
 
_John_Smith.addBook(_Day_Dreaming_2nd);
_Michelle_Diangello.addBook(_Day_Dreaming_2nd);
_Mark_Armstrong.addBook(_Day_Dreaming_2nd);
 
session.persist(_John_Smith);
session.persist(_Michelle_Diangello);
session.persist(_Mark_Armstrong);</pre>

The _Book_ and the _Book_Author_ rows are inserted along with the _Authors_:

<pre class="lang:tsql decode:true ">insert into Author (id, full_name) 
values (default, 'John Smith')
 
insert into Book (id, title) 
values (default, 'Day Dreaming')
 
insert into Author (id, full_name) 
values (default, 'Michelle Diangello')
 
insert into Book (id, title) 
values (default, 'Day Dreaming, Second Edition')
 
insert into Author (id, full_name) 
values (default, 'Mark Armstrong')
 
insert into Book_Author (book_id, author_id) values (1, 1)
insert into Book_Author (book_id, author_id) values (1, 2)
insert into Book_Author (book_id, author_id) values (2, 1)
insert into Book_Author (book_id, author_id) values (2, 2)
insert into Book_Author (book_id, author_id) values (3, 1)
</pre>

### <span id="Dissociating_one_side_of_the_many-to-many_association">Dissociating one side of the many-to-many association</span>

To delete an _Author_, we need to dissociate all _Book_Author_ relations belonging to the removable entity:

<pre class="lang:java decode:true ">doInTransaction(session -&gt; {
    Author _Mark_Armstrong =
        getByName(session, "Mark Armstrong");
    _Mark_Armstrong.remove();
    session.delete(_Mark_Armstrong);
});</pre>

This use case generates the following output:

<pre class="lang:tsql decode:true ">SELECT manytomany0_.id        AS id1_0_0_,
       manytomany2_.id        AS id1_1_1_,
       manytomany0_.full_name AS full_nam2_0_0_,
       manytomany2_.title     AS title2_1_1_,
       books1_.author_id      AS author_i2_0_0__,
       books1_.book_id        AS book_id1_2_0__
FROM   author manytomany0_
INNER JOIN book_author books1_
    ON manytomany0_.id = books1_.author_id
INNER JOIN book manytomany2_
    ON books1_.book_id = manytomany2_.id
WHERE  manytomany0_.full_name = 'Mark Armstrong'
 
SELECT books0_.author_id  AS author_i2_0_0_,
       books0_.book_id    AS book_id1_2_0_,
       manytomany1_.id    AS id1_1_1_,
       manytomany1_.title AS title2_1_1_
FROM   book_author books0_
INNER JOIN book manytomany1_
    ON books0_.book_id = manytomany1_.id
WHERE  books0_.author_id = 2
 
delete from Book_Author where book_id = 2
 
insert into Book_Author (book_id, author_id) values (2, 1)
insert into Book_Author (book_id, author_id) values (2, 2)
 
delete from Author where id = 3</pre>

The many-to-many association generates way too many redundant SQL statements and often, they are very difficult to tune. Next, I’m going to demonstrate the many-to-many _CascadeType.REMOVE_ hidden dangers.

### <span id="The_many-to-many_CascadeTypeREMOVE_gotchas">The many-to-many CascadeType.REMOVE gotchas</span>

The many-to-many _CascadeType.ALL_ is another code smell, I often bump into while reviewing code. The _CascadeType.REMOVE_ is automatically inherited when using_CascadeType.ALL_, but the entity removal is not only applied to the link table, but to the other side of the association as well.

Let’s change the _Author_ entity _books_ many-to-many association to use the_CascadeType.ALL_ instead:

<pre class="lang:java decode:true ">@ManyToMany(mappedBy = "authors", 
    cascade = CascadeType.ALL)
private List&lt;Book&gt; books = new ArrayList&lt;&gt;();</pre>

When deleting one _Author_:

<pre class="lang:java decode:true ">doInTransaction(session -&gt; {
    Author _Mark_Armstrong = 
        getByName(session, "Mark Armstrong");
    session.delete(_Mark_Armstrong);
    Author _John_Smith = 
        getByName(session, "John Smith");
    assertEquals(1, _John_Smith.books.size());
});</pre>

All books belonging to the deleted _Author_ are getting deleted, even if other _Authors_we’re still associated to the deleted _Books_:

<pre class="lang:tsql decode:true">SELECT manytomany0_.id        AS id1_0_,
       manytomany0_.full_name AS full_nam2_0_
FROM   author manytomany0_
WHERE  manytomany0_.full_name = 'Mark Armstrong' 
 
SELECT books0_.author_id  AS author_i2_0_0_,
       books0_.book_id    AS book_id1_2_0_,
       manytomany1_.id    AS id1_1_1_,
       manytomany1_.title AS title2_1_1_
FROM   book_author books0_
INNER JOIN book manytomany1_ ON
       books0_.book_id = manytomany1_.id
WHERE  books0_.author_id = 3  
 
delete from Book_Author where book_id=2
delete from Book where id=2
delete from Author where id=3</pre>

Most often, this behavior doesn’t match the business logic expectations, only being discovered upon the first entity removal.

We can push this issue even further, if we set the _CascadeType.ALL_ to the _Book_ entity side as well:

<pre class="lang:java decode:true ">@ManyToMany(cascade = CascadeType.ALL)
@JoinTable(name = "Book_Author",
    joinColumns = {
        @JoinColumn(
            name = "book_id", 
            referencedColumnName = "id"
        )
    },
    inverseJoinColumns = {
        @JoinColumn(
            name = "author_id", 
            referencedColumnName = "id"
        )
    }
)</pre>

This time, not only the _Books_ are being deleted, but _Authors_ are deleted as well:

<pre class="lang:java decode:true ">doInTransaction(session -&gt; {
    Author _Mark_Armstrong = 
        getByName(session, "Mark Armstrong");
    session.delete(_Mark_Armstrong);
    Author _John_Smith = 
        getByName(session, "John Smith");
    assertNull(_John_Smith);
});</pre>

The _Author_ removal triggers the deletion of all associated _Books_, which further triggers the removal of all associated _Authors_. This is a very dangerous operation, resulting in a massive entity deletion that’s rarely the expected behavior.

<pre class="lang:tsql decode:true ">SELECT manytomany0_.id        AS id1_0_,
       manytomany0_.full_name AS full_nam2_0_
FROM   author manytomany0_
WHERE  manytomany0_.full_name = 'Mark Armstrong' 
 
SELECT books0_.author_id  AS author_i2_0_0_,
       books0_.book_id    AS book_id1_2_0_,
       manytomany1_.id    AS id1_1_1_,
       manytomany1_.title AS title2_1_1_
FROM   book_author books0_
INNER JOIN book manytomany1_
   ON books0_.book_id = manytomany1_.id
WHERE  books0_.author_id = 3  
 
SELECT authors0_.book_id      AS book_id1_1_0_,
       authors0_.author_id    AS author_i2_2_0_,
       manytomany1_.id        AS id1_0_1_,
       manytomany1_.full_name AS full_nam2_0_1_
FROM   book_author authors0_
INNER JOIN author manytomany1_
   ON authors0_.author_id = manytomany1_.id
WHERE  authors0_.book_id = 2  
 
SELECT books0_.author_id  AS author_i2_0_0_,
       books0_.book_id    AS book_id1_2_0_,
       manytomany1_.id    AS id1_1_1_,
       manytomany1_.title AS title2_1_1_
FROM   book_author books0_
INNER JOIN book manytomany1_
   ON books0_.book_id = manytomany1_.id
WHERE  books0_.author_id = 1 
 
SELECT authors0_.book_id      AS book_id1_1_0_,
       authors0_.author_id    AS author_i2_2_0_,
       manytomany1_.id        AS id1_0_1_,
       manytomany1_.full_name AS full_nam2_0_1_
FROM   book_author authors0_
INNER JOIN author manytomany1_
   ON authors0_.author_id = manytomany1_.id
WHERE  authors0_.book_id = 1  
 
SELECT books0_.author_id  AS author_i2_0_0_,
       books0_.book_id    AS book_id1_2_0_,
       manytomany1_.id    AS id1_1_1_,
       manytomany1_.title AS title2_1_1_
FROM   book_author books0_
INNER JOIN book manytomany1_
   ON books0_.book_id = manytomany1_.id
WHERE  books0_.author_id = 2  
 
delete from Book_Author where book_id=2
delete from Book_Author where book_id=1
delete from Author where id=2
delete from Book where id=1
delete from Author where id=1 
delete from Book where id=2
delete from Author where id=3</pre>

<p style="text-align: justify;">
  This use case is wrong in so many ways. There are a plethora of unnecessary SELECT statements and eventually we end up deleting all Authors and all their Books. That’s why CascadeType.ALL should raise your eyebrow, whenever you spot it on a many-to-many association.
</p>

When it comes to Hibernate mappings, you should always strive for simplicity. The [Hibernate documentation](http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch26.html) confirms this assumption as well:

> <p style="text-align: justify;">
>   Practical test cases for real many-to-many associations are rare. Most of the time you need additional information stored in the “link table”. In this case, it is much better to use two one-to-many associations to an intermediate link class. In fact, most associations are one-to-many and many-to-one. For this reason, you should proceed cautiously when using any other association style.
> </p>

## <span id="Conclusion">Conclusion</span>

<p style="text-align: justify;">
  Cascading is a handy ORM feature, but it’s not free of issues. You should only cascade from Parent entities to Children and not the other way around. You should always use only the casacde operations that are demanded by your business logic requirements, and not turn the CascadeType.ALL into a default Parent-Child association entity state propagation configuration.
</p>

Code available on [GitHub](https://github.com/vladmihalcea/hibernate-master-class).

<p class="note_normal">
  Published at Codingpedia.org with permission of Vlad Mihalcea – source <a title="http://vladmihalcea.com/2015/03/05/a-beginners-guide-to-jpa-and-hibernate-cascade-types/" href="http://vladmihalcea.com/2015/03/05/a-beginners-guide-to-jpa-and-hibernate-cascade-types/" target="_blank">A beginner’s guide to JPA and Hibernate Cascade Types</a> from http://vladmihalcea.com/
</p>

<p class="note_normal">
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
