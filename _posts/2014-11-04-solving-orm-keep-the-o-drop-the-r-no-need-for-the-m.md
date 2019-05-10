---
id: 2040
title: 'Solving ORM &#8211; Keep the O, Drop the R, no need for the M'
date: 2014-11-04T14:31:29+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=2040
permalink: /jhadesdev/solving-orm-keep-the-o-drop-the-r-no-need-for-the-m/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3157026711
fsb_social_facebook:
  - 2
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 1
fsb_social_pinterest:
  - 0
categories:
  - architecture
  - database
  - hibernate
  - java
tags:
  - graphDB
  - jpql
  - neo4j
  - orm
  - relational
  - SQL
---
<p style="text-align: justify;">
  ORM has a simple, production-ready solution hiding in plain sight in the Java world. Let’s go through it in this post, alongside with the following topics:
</p>

  * ORM / Hibernate in 2014 &#8211; the word on the street
  * ORM is _still_ the Vietnam of Computer Science
  * ORM has 2 main goals only
  * When does ORM make sense?
  * A simple solution for the ORM problem
  * A production-ready ORM Java-based alternative

<!--more-->

### ORM / Hibernate in 2014 &#8211; the word on the street {#ormhibernatein2014thewordonthestreet}

<p style="text-align: justify;">
  It&#8217;s been almost 20 years since ORM is around, and soon we will reach the 15th birthday of the creation of the de-facto and likely best ORM implementation in the Java world: Hibernate.
</p>

<p style="text-align: justify;">
  We would then expect that this is by know a well understood problem. But what are developers saying these days about Hibernate and ORM?
</p>

Let&#8217;s take some quotes from two recent posts on this topic: [Thoughts on Hibernate](http://java.dzone.com/articles/thoughts-hibernate) and [JPA Hibernate Alternatives](http://java.dzone.com/articles/jpa-hibernate-alternatives):

> There are performance problems related to using Hibernate
> 
> A lot of business operations and reports involve writing complex queries. Writing them in terms of objects and maintaining them seems to be difficult
> 
> We shouldn&#8217;t be needing a 900 page book to learn a new framework.

<p style="text-align: justify;">
  As Java developers we can easily relate to that: ORM frameworks tend to give cryptic error messages, the mapping is hard to do and the runtime behavior namely with lazy initialization exceptions can be surprising when first encountered.
</p>

<p style="text-align: justify;">
  Who hasn&#8217;t had to maintain that application that uses <a href="http://blog.jhades.org/open-session-in-view-pattern-pros-and-cons/">Open Session In View</a> pattern that generated a flood of SQL requests that took weeks to optimize?
</p>

<p style="text-align: justify;">
  I believe it literally can take a couple of years to really understand Hibernate, lot&#8217;s of practice and several readings of the <a href="http://www.manning.com/bauer3/">Java Persistence with Hibernate</a> book (still 600 pages in it&#8217;s upcoming second edition).
</p>

### Are the criticisms on Hibernate warranted? {#arethecriticismsonhibernatewarranted}

<p style="text-align: justify;">
  I personally don&#8217;t think so, in fact most developers really criticize the complexity of the object-relational mapping approach itself, and not a concrete ORM implementation of it in a given language.
</p>

<p style="text-align: justify;">
  This sentiment seems to come and go in periodic waves, maybe when a newer generation of developers hits the labor force. After hours and days trying to do what feels it should be much simpler, it&#8217;s only a natural feeling.
</p>

<p style="text-align: justify;">
  The fact is that there is a problem: why do many projects spend 30% of their time developing the persistence layer still today?
</p>

### ORM is the Vietnam of Computer Science {#ormisthevietnamofcomputerscience}

<p style="text-align: justify;">
  The problem is that the ORM problem is complex, and there are no good solutions. Any solution to it is a huge compromise.
</p>

<p style="text-align: justify;">
  ORM has been famously named almost 10 years ago the Vietnam of Computer Science, in a <a href="http://blog.codinghorror.com/object-relational-mapping-is-the-vietnam-of-computer-science/">blog post</a> from one of the creators of Stackoverflow, <a href="http://en.wikipedia.org/wiki/Jeff_Atwood">Jeff Atwood</a>.
</p>

The problems of ORM are well known and we won&#8217;t go through them in detail here, here is a [summary](http://martinfowler.com/bliki/OrmHate.html) from Martin Fowler on why ORM is hard:

  * object identity vs database identity
  * How to map object oriented inheritance in the relational world
  * unidirectional associations in the database vs bi-directional in the OO world
  * Data navigation &#8211; lazy loading, eager fetching
  * Database transactions vs no rollbacks in the OO world

<p style="text-align: justify;">
  This is just to name the main obstacles. The problem is also that it&#8217;s easy to forget what we are trying to achieve in the first place.
</p>

### ORM has 2 main goals only {#ormhas2maingoalsonly}

ORM has two main goals clearly defined:

  * map objects from the OO world into tables in a relational database
  * provide a runtime mechanism for keeping an in-memory graph of objects and a set of database tables in sync

Given this, when should we use Hibernate and ORM in general?

### When does ORM make sense ? {#whendoesormmakesense}

<p style="text-align: justify;">
  ORM makes sense when the project at hand is being done using a <a href="http://en.wikipedia.org/wiki/Domain-driven_design">Domain Driven Development</a> approach, where the whole program is built around a set of core classes called the domain model, that represent concepts in the real world such as <code>Customer</code>, <code>Invoice</code>, etc.
</p>

<p style="text-align: justify;">
  If the project does not have a minimum threshold complexity that needs DDD, then an ORM can likely be overkill. The problem is that even the most simple of enterprise applications are well above this threshold, so ORM really pulls it&#8217;s weight most of the time.
</p>

<p style="text-align: justify;">
  It&#8217;s just that ORM is hard to learn and full of pitfalls. So how can we tackle this problem?
</p>

### A simple solution for the ORM problem {#asimplesolutionfortheormproblem}

Someone once said something like this:

> A smart man solves a problem, but a wise man avoids it.

<p style="text-align: justify;">
  As often happens in programming, we can find the solution by going back to the beginning and see what we are trying to solve:
</p>

![ORM](http://martinfowler.com/bliki/images/ormHate/sketch.png)

<p style="text-align: justify;">
  So we are trying to synchronize an in-memory <em>graph</em> of objects with a set of tables. But these are two completely different types of data structures!
</p>

<p style="text-align: justify;">
  But which data structure is the most generic? It turns out that the graph is the most generic one of the two: actually a set of linked database tables is really just a special type of graph.
</p>

The same can be said of basically almost any other data structure.

<p style="text-align: justify;">
  Graphs and their traversal are very well understood and have a body of knowledge of decades available, similar to the theory on which relational databases are built upon: Relational Algebra.
</p>

### Solving the impedance mismatch {#solvingtheimpedancemismatch}

The logical conclusion is that the solution for the ORM impedance mismatch is removing the mismatch itself:

> Let&#8217;s store the graph of in-memory domain objects in a transactional-capable graph database!

This solves the mapping problem, by removing the need for mapping in the first place.

### A production-ready solution for the ORM problem {#aproductionreadysolutionfortheormproblem}

This is easier said than done, or is it? It turns out that graph databases have been around for years, and the prime example in the Java community is [Neo4j](http://www.neo4j.org/).

Neo4j is a stable product that is well understood and documented, see the [Neo4J in Action](http://www.manning.com/partner/) book. It can used as an external server or in embedded mode inside the Java process itself.

But it&#8217;s core API is all about graphs and nodes, something like this:

<pre class="lang:java decode:true ">GraphDatabaseService gds = new EmbeddedGraphDatabase("/path/to/store"); 
Node forrest=gds.createNode(); 
forrest.setProperty("title","Forrest Gump"); 
forrest.setProperty("year",1994); 
gds.index().forNodes("movies").add(forrest,"id",1);
 
Node tom=gds.createNode();</pre>

The problem is that this is too far from domain driven development, writing to this would be like coding JDBC by hand.

This is the typical task of a framework like Hibernate, with the big difference that because the impedance mismatch is minimal such framework can operate in a much more transparent and less intrusive way.

> It turns out that such framework is already written (there are even two).

### Spring Data Neo4J {#springdataneo4j}

<p style="text-align: justify;">
  One of the creators of the Spring framework Rod Johnson took the task of implementing himself the initial version of the Neo4j integration, the <a href="http://projects.spring.io/spring-data-neo4j/">Spring Data Neo4j</a> project.
</p>

<p style="text-align: justify;">
  This is an important extract from the <a href="http://docs.spring.io/spring-data/data-neo4j/docs/3.2.0.RELEASE/reference/html/#foreword">foreword</a> of Rod Johnson in the documentation concerning the design of the framework:
</p>

> <p style="text-align: justify;">
>   Its use of AspectJ to eliminate persistence code from your domain model is truly innovative, and on the cutting edge of today&#8217;s Java technologies.
> </p>

<p style="text-align: justify;">
  So Spring Data Neo4J is a AOP-based framework that wraps domain objects in a relatively transparent way, and synchronizes a in-memory graph of objects with a Neo4j transactional data store.
</p>

It&#8217;s aimed to write the persistence layer of the application in a simplified way, similar to Spring Data JPA.

### How does the mapping to a graph database look like {#howdoesthemappingtoagraphdatabaselooklike}

<p style="text-align: justify;">
  It turns out that there is limited mapping needed (<a href="http://docs.spring.io/spring-data/data-neo4j/docs/3.2.0.RELEASE/reference/html/#tutorial_annotations">tutorial</a>). We need for one to mark which classes we want to make persistent, and define a field that will act as an Id:
</p>

<pre class="lang:java decode:true">@NodeEntity
class Movie { 
    @GraphId Long nodeId;
    String id;
    String title;
    int year;
    Set&lt;role&gt; cast;
}
&lt;/role&gt;</pre>

<p style="text-align: justify;">
  There are other annotations (5 more per the docs) for example for defining indexing and relationships with properties, etc. Compared with Hibernate there is only a fraction of the annotations for the same domain model.
</p>

### What does the query language look like? {#whatdoesthequerylanguagelooklike}

The recommended query language is [Cypher](http://www.neo4j.org/learn/cypher), that is an ASCII art based language. A query can look for example like this:

<pre class="lang:java decode:true ">// returns users who rated a movie based on movie title (movieTitle parameter) higher than rating (rating parameter)
    @Query("start movie=node:Movie(title={0}) " +
           "match (movie)&lt;-[r:RATED]-(user) " +
           "where r.stars &gt; {1} " +
           "return user")
     Iterable&lt;user&gt; getUsersWhoRatedMovieFromTitle(String movieTitle, Integer rating);
&lt;/user&gt;</pre>

The query language is very different from JPQL or SQL and implies a learning curve.

<p style="text-align: justify;">
  Still after the learning curve this language allows to write performant queries that usually can be problematic in relational databases.
</p>

### Performance of Queries in Graph vs Relational databases {#performanceofqueriesingraphvsrelationaldatabases}

<p style="text-align: justify;">
  Let&#8217;s compare some frequent query types of queries and see how they should perform in a graph vs relational databases:
</p>

<li style="text-align: justify;">
  lookup by Id: This is implemented for example by doing a binary search on an index tree, finding a match and following a &#8216;pointer&#8217; to the result. This is a (very) simplified description, but it&#8217;s likely identical for both databases. There is no apparent reason why such query would take more time in a graph database than in a relational DB.
</li>
<li style="text-align: justify;">
  lookup parent relations: This is the type of query that relational databases struggle with. Self-joins might result in cartesian products of huge tables, bringing the database to an halt. A graph database can perform those queries in a fraction of that.
</li>
<li style="text-align: justify;">
  lookup by non-indexed column: Here the relational database can scan tables faster due to the physical structure of the table and the fact than one read usually brings along multiple rows. But this type of queries (table scans) are to be avoided in relational databases anyway.
</li>

There is more to say here, but there is no indication (no readilly-available DDD-related public benchmarks) that a graph-based data store would not be appropriate for doing DDD due to query performance.

### Prefer a JPA based solution? {#preferajpabasedsolution}

The Hibernate team has written [Hibernate OGM](http://hibernate.org/ogm/), which provides JPA support for many NoSQL data stores, including Neo4j.

### Conclusions {#conclusions}

<p style="text-align: justify;">
  I personally cannot find any (conceptual) reasons why a transaction-capable graph database would not be an ideal fit for doing Domain Driven Development, as an alternative to a relational database and ORM.
</p>

<p style="text-align: justify;">
  No data store will ever fit perfectly every use case, but we can ask the question if graph databases shouldn&#8217;t become the default for DDD, and relational the exception.
</p>

<p style="text-align: justify;">
  The disappearance of ORM would imply a great reduction of the complexity and the time that it takes to implement a project.
</p>

#### The future of DDD in the enterprise {#thefutureofdddintheenterprise}

<p style="text-align: justify;">
  The removal of the impedance mismatch and the improved performance of certain query types could be the killer features that drive the adoption of a graph based DDD solution.
</p>

<p style="text-align: justify;">
  We can see practical obstacles: operations prefer relational databases, vendor contract lock-in, having to learn a new query language, limited expertise in the labor market, etc.
</p>

<p style="text-align: justify;">
  But the economic advantage is there, and the technology is there also. And when that is case, it&#8217;s usually only a matter of time.
</p>

<p style="text-align: justify;">
  What about you, could you think of any reason why Graph-based DDD would not work? Feel free to chime in on the comments bellow.
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with permission of Aleksey Novik – source <a title="http://blog.jhades.org/solving-orm-complexity-keep-the-o-drop-the-r-no-need-for-the-m/" href="http://blog.jhades.org/solving-orm-complexity-keep-the-o-drop-the-r-no-need-for-the-m/" target="_blank">Solving ORM &#8211; Keep the O, Drop the R, no need for the M</a> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>

<p style="text-align: justify;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://lh6.googleusercontent.com/-nJLCOBcwQyQ/U3PTSOfhw_I/AAAAAAAAABI/w21JxlhW4lo/s498-no/my-blog-53.jpg" alt="Podcastpedia image" /> 
    
    <p id="about_author_header">
      <strong>Aleksey Novik</strong>
    </p>
    
    <div id="author_details" style="text-align: justify;">
      Software developer, likes to learn new technologies, hang out on stackoverflow and blog on tips and tricks on Java/Javascript polyglot enterprise development.
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://blog.jhades.org/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/113901291479894108481/posts" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/JhadesDev" target="_blank"> </a> <a class="icon-github" href="https://github.com/jhades" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>

&nbsp;

&nbsp;
