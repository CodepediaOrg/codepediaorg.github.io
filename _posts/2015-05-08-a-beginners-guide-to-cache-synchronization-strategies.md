---
id: 2377
title: A beginner’s guide to Cache synchronization strategies
date: 2015-05-08T06:39:09+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=2377
permalink: /vladmihalcea/a-beginners-guide-to-cache-synchronization-strategies/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3746199068
fsb_social_facebook:
  - 0
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 1
categories:
  - database
  - Java EE
tags:
  - cache
  - caching
  - database
  - db
  - jpa
  - system of record
---
## Introduction

<p style="text-align: justify;">
  A <a href="http://en.wikipedia.org/wiki/System_of_record">system of record</a> is the authoritative data source when information is scattered among various data providers. When we introduce a <a href="http://vladmihalcea.com/2015/04/16/things-to-consider-before-jumping-to-enterprise-caching/">caching solution</a>, we automatically duplicate our data. To avoid inconsistent reads and data integrity issues, it’s very important to synchronize the database and the cache (whenever a change occurs into the system).
</p>

<p style="text-align: justify;">
  There are various ways to keep the cache and the underlying database in sync and this article will present some of the most common cache synchronization strategies.<!--more-->
</p>

## Cache-aside

<p style="text-align: justify;">
  The application code can manually manage both the database and the cache information. The application logic inspects the cache before hitting the database and it updates the cache after any database modification.
</p>

[<img class="alignnone size-large wp-image-4399" src="https://vladmihalcea.files.wordpress.com/2015/04/cacheaside.png?w=788&h=484" alt="CacheAside" width="788" height="484" />](https://vladmihalcea.files.wordpress.com/2015/04/cacheaside.png)

<p style="text-align: justify;">
  Mixing caching management and application is not very appealing, especially if we have to repeat these steps in every data retrieval method. Leveraging an <a href="http://vladmihalcea.com/2014/12/01/spring-request-level-memoization/">Aspect-Oriented caching interceptor</a> can mitigate the cache leaking into the application code, but it doesn’t exonerate us from making sure that both the database and the cache are properly synchronized.
</p>

## Read-through

<p style="text-align: justify;">
  Instead of managing both the database and the cache, we can simply delegate the database synchronization to the cache provider. All data interactions is therefore done through the cache abstraction layer.
</p>

[<img class="alignnone size-large wp-image-4401" src="https://vladmihalcea.files.wordpress.com/2015/04/cachereadthrough.png?w=788&h=414" alt="CacheReadThrough" width="788" height="414" />](https://vladmihalcea.files.wordpress.com/2015/04/cachereadthrough.png)

<p style="text-align: justify;">
  Upon fetching a cache entry, the Cache verifies the cached element availability and loads the underlying resource on our behalf. The application uses the cache as the <em>system of record</em> and the cache is able to auto-populate on demand.
</p>

## Write-through

<p style="text-align: justify;">
  Analogous to the <em>read-through</em> data fetching strategy, the cache can update the underlying database every time a cache entry is changed.
</p>

[<img class="alignnone size-large wp-image-4403" src="https://vladmihalcea.files.wordpress.com/2015/04/cachewritethrough.png?w=788&h=192" alt="CacheWriteThrough" width="788" height="192" />](https://vladmihalcea.files.wordpress.com/2015/04/cachewritethrough.png)

<p style="text-align: justify;">
  Although the database and the cache are updated synchronously, we have the liberty of choosing the transaction boundaries according to our current business requirements.
</p>

<li style="text-align: justify;">
  If strong consistency is mandatory and the cache provider offers an <a href="http://docs.oracle.com/javaee/7/api/javax/transaction/xa/XAResource.html">XAResource</a> we can then enlist the cache and the database in the same global transaction. The database and the cache are therefore updated in a <a href="http://vladmihalcea.com/2014/01/05/a-beginners-guide-to-acid-and-database-transactions/">single atomic unit-of-work</a>
</li>
<li style="text-align: justify;">
  If consistency can be weaken, we can update the cache and the database sequentially, without using a global transaction. Usually the cache is changed first and if the database update fails, the cache can use a compensating action to roll-back the current transaction changes
</li>

## Write-behind

<p style="text-align: justify;">
  If strong consistency is not mandated, we can simply enqueue the cache changes and periodically flush them to the database.
</p>

[<img class="alignnone size-large wp-image-4405" src="https://vladmihalcea.files.wordpress.com/2015/04/cachewritebehind.png?w=788&h=598" alt="CacheWriteBehind" width="788" height="598" />](https://vladmihalcea.files.wordpress.com/2015/04/cachewritebehind.png)

<p style="text-align: justify;">
  This strategy is employed by the <a href="http://vladmihalcea.com/2014/08/07/a-beginners-guide-to-jpahibernate-flush-strategies/">Java Persistence </a><a href="http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html"><em>EntityManager</em></a> (first-level cache), all <a href="http://vladmihalcea.com/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/">entity state transitions</a> being flushed towards the end of the current running transaction (or when a query is issued).
</p>

<p style="text-align: justify;">
  Although it breaks transaction guarantees, the <em>write-behind</em> caching strategy can outperform the <em>write-through</em> policy, because <a href="http://vladmihalcea.com/2015/03/18/how-to-batch-insert-and-update-statements-with-hibernate/">database updates can be batched</a> and the number of <em>DML</em> transactions is also reduced.
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with permission of <a title="A beginner’s guide to Cache synchronization strategies" href="A%20beginner’s guide to Cache synchronization strategies" target="_blank">Vlad Mihalcea</a> – source <a title="http://vladmihalcea.com/2015/04/20/a-beginners-guide-to-cache-synchronization-strategies/" href="http://vladmihalcea.com/2015/04/20/a-beginners-guide-to-cache-synchronization-strategies/" target="_blank">A beginner’s guide to Cache synchronization strategies</a> from <a title="http://vladmihalcea.com/" href="http://vladmihalcea.com/" target="_blank">http://vladmihalcea.com/</a>
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
