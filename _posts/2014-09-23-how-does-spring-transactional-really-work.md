---
id: 1789
title: How does Spring @Transactional Really Work?
date: 2014-09-23T13:51:38+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=1789
permalink: /jhadesdev/how-does-spring-transactional-really-work/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3045692885
fsb_social_facebook:
  - 1
  - 1
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - spring
tags:
  - aop
  - entity manager
  - jpa
  - transaction
  - transaction manager
  - transactional
---
<p style="color: #3a4145; text-align: justify;">
  In this post we will do a deep dive into Spring transaction management. We will go over on how does <code>@Transactional</code> really works under the hood. Other upcoming posts will include:
</p>

<ul style="color: #3a4145;">
  <li>
    how to use features like propagation and isolation
  </li>
  <li>
    what are the main pitfalls and how to avoid them
  </li>
</ul>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#JPA_and_Transaction_Management">JPA and Transaction Management</a>
    </li>
    <li>
      <a href="#Using_Spring_Transactional">Using Spring @Transactional</a>
    </li>
    <li>
      <a href="#What_doesTransactionalmean">What does @Transactional mean?</a>
    </li>
    <li>
      <a href="#When_does_an_EntityManager_span_multiple_database_transactions">When does an EntityManager span multiple database transactions?</a>
    </li>
    <li>
      <a href="#What_defines_the_EntityManager_vs_Transaction_relation">What defines the EntityManager vs Transaction relation?</a>
    </li>
    <li>
      <a href="#How_does_PersistenceContext_work">How does @PersistenceContext work?</a>
    </li>
    <li>
      <a href="#How_does_Transactional_work_then">How does @Transactional work then?</a>
    </li>
    <li>
      <a href="#The_Transactional_Aspect">The Transactional Aspect</a>
    </li>
    <li>
      <a href="#The_Transaction_Manager">The Transaction Manager</a>
    </li>
    <li>
      <a href="#The_EntityManager_proxy">The EntityManager proxy</a>
    </li>
    <li>
      <a href="#Putting_It_All_Together">Putting It All Together</a>
    </li>
    <li>
      <a href="#Conclusion">Conclusion</a>
    </li>
  </ul>
</div>

<h4 id="jpaandtransactionmanagement" style="color: #3a4145;">
  <span id="JPA_and_Transaction_Management">JPA and Transaction Management</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  It&#8217;s important to notice that JPA on itself does not provide any type of declarative transaction management. When using JPA outside of a dependency injection container, transactions need to be handled programatically by the developer:
</p>

<pre class="lang:java decode:true ">UserTransaction utx = entityManager.getTransaction(); 
 
try { 
    utx.begin(); 
 
    businessLogic();
 
    utx.commit(); 
} catch(Exception ex) { 
    utx.rollback(); 
    throw ex; 
}</pre>

<p style="color: #3a4145; text-align: justify;">
  This way of managing transactions makes the scope of the transaction very clear in the code, but it has several disavantages:
</p>

<ul style="color: #3a4145;">
  <li>
    it&#8217;s repetitive and error prone
  </li>
  <li>
    any error can have a very high impact
  </li>
  <li>
    errors are hard to debug and reproduce
  </li>
  <li>
    this decreases the readability of the code base
  </li>
  <li>
    What if this method calls another transactional method?
  </li>
</ul>

<h4 id="usingspringtransactional" style="color: #3a4145;">
  <span id="Using_Spring_Transactional">Using Spring @Transactional</span>
</h4>

<p style="color: #3a4145;">
  With Spring <code>@Transactional</code>, the above code gets reduced to simply this:
</p>

<pre class="lang:java decode:true ">@Transactional
public void businessLogic() {
    ... use entity manager inside a transaction ...
}</pre>

<p style="color: #3a4145; text-align: justify;">
  This is much more convenient and readable, and is currently the recommended way to handle transactions in Spring.
</p>

<p style="color: #3a4145; text-align: justify;">
  By using <code>@Transactional</code>, many important aspects such as transaction propagation are handled automatically. In this case if another transactional method is called by <code>businessLogic()</code>, that method will have the option of joining the ongoing transaction.
</p>

<p style="color: #3a4145; text-align: justify;">
  One potential downside is that this powerful mechanism hides what is going on under the hood, making it hard to debug when things don&#8217;t work.
</p>

<h4 id="whatdoestransactionalmean" style="color: #3a4145;">
  <span id="What_doesTransactionalmean">What does <code>@Transactional</code> mean?</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  One of the key points about <code>@Transactional</code> is that there are two separate concepts to consider, each with it&#8217;s own scope and life cycle:
</p>

<ul style="color: #3a4145;">
  <li>
    the persistence context
  </li>
  <li>
    the database transaction
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  The transactional annotation itself defines the scope of a single database transaction. The database transaction happens inside the scope of a <em>persistence context</em>.
</p>

<p style="color: #3a4145; text-align: justify;">
  The persistence context is in JPA the <code>EntityManager</code>, implemented internally using an Hibernate <code>Session</code> (when using Hibernate as the persistence provider).
</p>

<p style="color: #3a4145; text-align: justify;">
  The persistence context is just a synchronizer object that tracks the state of a limited set of Java objects and makes sure that changes on those objects are eventually persisted back into the database.
</p>

<p style="color: #3a4145; text-align: justify;">
  This is a very different notion than the one of a database transaction. One Entity Manager <strong>can be used across several database transactions</strong>, and it actually often is.
</p>

<h4 id="whendoesanentitymanagerspanmultipledatabasetransactions" style="color: #3a4145;">
  <span id="When_does_an_EntityManager_span_multiple_database_transactions">When does an EntityManager span multiple database transactions?</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  The most frequent case is when the application is using the Open Session In View pattern to deal with lazy initialization exceptions, see this previous blog post for it&#8217;s <a href="http://blog.jhades.org/open-session-in-view-pattern-pros-and-cons/">pros and cons</a>.
</p>

<p style="color: #3a4145; text-align: justify;">
  In such case the queries that run in the view layer are in separate database transactions than the one used for the business logic, but they are made via the same entity manager.
</p>

<p style="color: #3a4145; text-align: justify;">
  Another case is when the persistence context is marked by the developer as <code>PersistenceContextType.EXTENDED</code>, which means that it can survive multiple requests.
</p>

<h4 id="whatdefinestheentitymanagervstransactionrelation" style="color: #3a4145;">
  <span id="What_defines_the_EntityManager_vs_Transaction_relation">What defines the EntityManager vs Transaction relation?</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  This is actually a choice of the application developer, but the most frequent way to use the JPA Entity Manager is with the &#8220;Entity Manager per application transaction&#8221; pattern. This is the most common way to inject an entity manager:
</p>

<pre class="lang:java decode:true ">@PersistenceContext
private EntityManager em;</pre>

<p style="color: #3a4145; text-align: justify;">
  Here we are by default in &#8220;Entity Manager per transaction&#8221; mode. In this mode, if we use this Entity Manager inside a <code>@Transactional</code> method, then the method will run in a single database transaction.
</p>

<h4 id="howdoespersistencecontextwork" style="color: #3a4145;">
  <span id="How_does_PersistenceContext_work">How does @PersistenceContext work?</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  One question that comes to mind is, how can <code>@PersistenceContext</code> inject an entity manager only once at container startup time, given that entity managers are so short lived, and that there are usually multiple per request.
</p>

<p style="color: #3a4145; text-align: justify;">
  The answer is that it can&#8217;t: <code>EntityManager</code> is an interface, and what gets injected in the spring bean is not the entity manager itself but <em>a context aware proxy</em> that will delegate to a concrete entity manager at runtime.
</p>

<p style="color: #3a4145; text-align: justify;">
  Usually the concrete class used for the proxy is <code>SharedEntityManagerInvocationHandler</code>, this can be confirmed with the help a debugger.
</p>

<h4 id="howdoestransactionalworkthen" style="color: #3a4145;">
  <span id="How_does_Transactional_work_then">How does @Transactional work then?</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  The persistence context proxy that implements <code>EntityManager</code> is not the only component needed for making declarative transaction management work. Actually three separate components are needed:
</p>

<ul style="color: #3a4145;">
  <li>
    The EntityManager Proxy itself
  </li>
  <li>
    The Transactional Aspect
  </li>
  <li>
    The Transaction Manager
  </li>
</ul>

<p style="color: #3a4145;">
  Let&#8217;s go over each one and see how they interact.
</p>

<h4 id="thetransactionalaspect" style="color: #3a4145;">
  <span id="The_Transactional_Aspect">The Transactional Aspect</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  The Transactional Aspect is an &#8216;around&#8217; aspect that gets called both before and after the annotated business method. The concrete class for implementing the aspect is <code>TransactionInterceptor</code>.
</p>

<p style="color: #3a4145;">
  The Transactional Aspect has two main responsibilities:
</p>

<ul style="color: #3a4145;">
  <li style="text-align: justify;">
    At the &#8216;before&#8217; moment, the aspect provides a hook point for determining if the business method about to be called should run in the scope of an ongoing database transaction, or if a new separate transaction should be started.
  </li>
  <li style="text-align: justify;">
    At the &#8216;after&#8217; moment, the aspect needs to decide if the transaction should be committed, rolled back or left running.
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  At the &#8216;before&#8217; moment the Transactional Aspect itself does not contain any decision logic, the decision to start a new transaction if needed is delegated to the Transaction Manager.
</p>

<h4 id="thetransactionmanager" style="color: #3a4145;">
  <span id="The_Transaction_Manager">The Transaction Manager</span>
</h4>

<p style="color: #3a4145;">
  The transaction manager needs to provide an answer to two questions:
</p>

<ul style="color: #3a4145;">
  <li>
    should a new Entity Manager be created?
  </li>
  <li>
    should a new database transaction be started?
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  This needs to be decided at the moment the Transactional Aspect &#8216;before&#8217; logic is called. The transaction manager will decide based on:
</p>

<ul style="color: #3a4145;">
  <li>
    the fact that one transaction is already ongoing or not
  </li>
  <li>
    the propagation attribute of the transactional method (for example <code>REQUIRES_NEW</code> always starts a new transaction)
  </li>
</ul>

<p style="color: #3a4145;">
  If the transaction manager decides to create a new transaction, then it will:
</p>

<ul style="color: #3a4145;">
  <li>
    create a new entity manager
  </li>
  <li>
    bind the entity manager to the current thread
  </li>
  <li>
    grab a connection from the DB connection pool
  </li>
  <li>
    bind the connection to the current thread
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  The entity manager and the connection are both bound to the current thread using <a href="http://docs.oracle.com/javase/6/docs/api/java/lang/ThreadLocal.html">ThreadLocal</a> variables.
</p>

<p style="color: #3a4145; text-align: justify;">
  They are stored in the thread while the transaction is running, and it&#8217;s up to the Transaction Manager to clean them up when no longer needed.
</p>

<p style="color: #3a4145; text-align: justify;">
  Any parts of the program that need the current entity manager or connection can retrieve them from the thread. One program component that does exactly that is the EntityManager proxy.
</p>

<h4 id="theentitymanagerproxy" style="color: #3a4145;">
  <span id="The_EntityManager_proxy">The EntityManager proxy</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  The EntityManager proxy (that we have introduced before) is the last piece of the puzzle. When the business method calls for example<br /> <code>entityManager.persist()</code>, this call is not invoking the entity manager directly.
</p>

<p style="color: #3a4145; text-align: justify;">
  Instead the business method calls the proxy, which retrieves the current entity manager from the thread, where the Transaction Manager put it.
</p>

<p style="color: #3a4145; text-align: justify;">
  Knowing now what are the moving parts of the <code>@Transactional </code>mechanism, let&#8217;s go over the usual Spring configuration needed to make this work.
</p>

<h4 id="puttingitalltogether" style="color: #3a4145;">
  <span id="Putting_It_All_Together">Putting It All Together</span>
</h4>

<p style="text-align: justify;">
  Let&#8217;s go over how to setup the three components needed to make the transactional annotation work correctly. We start by defining the entity manager factory.
</p>

This will allow the injection of Entity Manager proxies via the persistence context annotation:

<pre class="lang:java decode:true ">@Configuration
public class EntityManagerFactoriesConfiguration {
    @Autowired
    private DataSource dataSource;
 
    @Bean(name = "entityManagerFactory")
    public LocalContainerEntityManagerFactoryBean emf() {
        LocalContainerEntityManagerFactoryBean emf = ...
        emf.setDataSource(dataSource);
        emf.setPackagesToScan(
            new String[] {"your.package"});
        emf.setJpaVendorAdapter(
            new HibernateJpaVendorAdapter());
        return emf;
    }
}</pre>

<p style="text-align: justify;">
  <span style="color: #3a4145;">The next step is to configure the Transaction Manager and to apply the Transactional Aspect in </span><code style="color: #3a4145;">@Transactional</code><span style="color: #3a4145;"> annotated classes:</span>
</p>

<pre class="lang:java decode:true ">@EnableTransactionManagement
public class TransactionManagersConfig {
    @Autowired
    EntityManagerFactory emf;
    @Autowired
    private DataSource dataSource;
 
    @Bean(name = "transactionManager")
    public PlatformTransactionManager transactionManager() {
        JpaTransactionManager tm = 
            new JpaTransactionManager();
            tm.setEntityManagerFactory(emf);
            tm.setDataSource(dataSource);
        return tm;
    }
}</pre>

<p style="color: #3a4145; text-align: justify;">
  The annotation <code>@EnableTransactionManagement</code> tells Spring that classes with the <code>@Transactional</code> annotation should be wrapped with the Transactional Aspect. With this the <code>@Transactional</code> is now ready to be used.
</p>

<h4 id="conclusion" style="color: #3a4145;">
  <span id="Conclusion">Conclusion</span>
</h4>

<p style="color: #3a4145; text-align: justify;">
  The Spring declarative transaction management mechanism is very powerful, but it can be misused or wrongly configured easily.
</p>

<p style="color: #3a4145; text-align: justify;">
  Understanding how it works internally is helpful when troubleshooting situations when the mechanism is not at all working or is working in an unexpected way.
</p>

<p style="color: #3a4145; text-align: justify;">
  The most important thing to bear in mind is that there are really two concepts to take into account: the database transaction and the persistence context, each with it&#8217;s own not readily apparent life cycle.
</p>

<p style="color: #3a4145; text-align: justify;">
  A future post will go over the most frequent pitfalls of the transactional annotation and how to avoid them.
</p>

<p class="note_normal" style="color: #3a4145; text-align: justify;">
  Published at Codingpedia.org with permission of <a title="http://www.codingpedia.org/jhadesdev/" href="http://www.codingpedia.org/jhadesdev/" target="_blank">Aleksey Novik</a> &#8211; source <a title="http://blog.jhades.org/how-does-spring-transactional-really-work/" href="http://blog.jhades.org/how-does-spring-transactional-really-work/" target="_blank">How does Spring @Transactional Really Work?</a> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>

<p style="color: #3a4145; text-align: justify;">
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

<p style="color: #3a4145;">
