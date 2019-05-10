---
id: 2388
title: 'A fresh look on accessing database on JVM platform: Slick from Typesafe'
date: 2015-06-20T17:41:18+00:00
author: Andriy Redko
layout: post
guid: http://www.codingpedia.org/?p=2388
permalink: /aredko/a-fresh-look-on-accessing-database-on-jvm-platform-slick-from-typesafe/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3864602135
fsb_social_facebook:
  - 1
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - development
tags:
  - database
  - jpa
  - scala
  - slick
---
<p style="text-align: justify;">
  In today&#8217;s post we are going to open our mind, step away from traditional <a href="http://www.oracle.com/technetwork/java/javaee/tech/index-jsp-142185.html">Java EE</a> / <a href="http://www.oracle.com/technetwork/java/javase/overview/index.html">Java SE</a> <a href="https://jcp.org/en/jsr/detail?id=338">JPA</a>-based stack (which I think is great) and take a refreshing look on how to access database in your Java applications using the new kid on the block: <a href="http://slick.typesafe.com/">Slick 2.1</a> from <a href="http://slick.typesafe.com/">Typesafe</a>. So if <a href="https://jcp.org/en/jsr/detail?id=338">JPA</a> is so great, why bother? Well, sometimes you need to do very simple things and there is no need to bring the complete, well modeled persistence layer for that. In here <a href="http://slick.typesafe.com/">Slick</a> shines.<!--more-->
</p>

<p style="text-align: justify;">
  In the essence, <a href="http://slick.typesafe.com/">Slick</a> is database access library, not an <a href="http://en.wikipedia.org/wiki/Object-relational_mapping">ORM</a>. Though it is written in <a href="http://www.scala-lang.org/">Scala</a>, the examples we are going to look at do not require any particular knowledge of this excellent language (although, it is just <a href="http://www.scala-lang.org/">Scala</a> that made <a href="http://slick.typesafe.com/">Slick</a> possible to exist). Our relational database schema will have only two tables, <b>customers</b> and <b>addresses</b>, linked by one-to-many relationships. For simplicity, <a href="http://www.h2database.com/">H2</a> has been picked as an in-memory database engine.
</p>

<p style="text-align: justify;">
  The first question which comes up is defining database tables (schema) and, naturally, database specific <a href="http://en.wikipedia.org/wiki/Data_definition_language">DDLs</a> are the standard way of doing that. Can we do something about it and try another approach? If you are using <a href="http://slick.typesafe.com/">Slick 2.1</a>, the answer is yes, absolutely. Let us just describe our tables as <a href="http://www.scala-lang.org/">Scala</a> classes:
</p>

<pre class="lang:scala decode:true ">// The 'customers' relation table definition
class Customers( tag: Tag ) extends Table[ Customer ]( tag, "customers" ) {
  def id = column[ Int ]( "id", O.PrimaryKey, O.AutoInc )

  def email = column[ String ]( "email", O.Length( 512, true ), O.NotNull )
  def firstName = column[ String ]( "first_name", O.Length( 256, true ), O.Nullable )
  def lastName = column[ String ]( "last_name", O.Length( 256, true ), O.Nullable )

  // Unique index for customer's email
  def emailIndex = index( "idx_email", email, unique = true )
}</pre>

<p style="text-align: justify;">
  Very easy and straightforward, resembling a lot typical <b>CREATE TABLE</b> construct. The <b>addresses</b> table is going to be defined the same way and reference <b>users</b> table by foreign key.
</p>

<pre class="lang:scala decode:true">// The 'customers' relation table definition
class Addresses( tag: Tag ) extends Table[ Address ]( tag, "addresses" ) {
  def id = column[ Int ]( "id", O.PrimaryKey, O.AutoInc )

  def street = column[ String ]( "street", O.Length( 100, true ), O.NotNull )
  def city = column[ String ]( "city", O.Length( 50, true ), O.NotNull )
  def country = column[ String ]( "country", O.Length( 50, true ), O.NotNull )

  // Foreign key to 'customers' table
  def customerId = column[Int]( "customer_id", O.NotNull )
  def customer = foreignKey( "customer_fk", customerId, Customers )( _.id )
}</pre>

<p style="text-align: justify;">
  Great, leaving off some details, that is it: we have defined two database tables in pure <a href="http://www.scala-lang.org/">Scala</a>. But details are important and we are going to look closely on following two declarations: <b>Table[ Customer ]</b> and <b>Table[ Address ]</b>. Essentially, each table could be represented as a tuple with as many elements as column it has defined. For example, <b>customers</b> is a tuple of <b>(Int, String, String, String)</b>, while <b>addresses</b> table is a tuple of <b>(Int, String, String, String, Int)</b>. Tuples in <a href="http://www.scala-lang.org/">Scala</a> are great but they are not very convenient to work with. Luckily, <a href="http://slick.typesafe.com/">Slick</a> allows to use case classes instead of tuples by providing so called <a href="http://slick.typesafe.com/doc/2.1.0/introduction.html#lifted-embedding">Lifted Embedding</a>technique. Here are our <b>Customer</b> and <b>Address</b> case classes:
</p>

<pre class="lang:scala decode:true ">case class Customer( id: Option[Int] = None, email: String, 
  firstName: Option[ String ] = None, lastName: Option[ String ] = None)

case class Address( id: Option[Int] = None,  street: String, city: String, 
  country: String, customer: Customer )</pre>

<p style="text-align: justify;">
  The last but not least question is how <a href="http://slick.typesafe.com/">Slick</a> is going to convert from tuples to case classes and vice-versa? It would be awesome to have such conversion out-of-the box but at this stage <a href="http://slick.typesafe.com/">Slick</a> needs a bit of help. Using <a href="http://slick.typesafe.com/">Slick</a> terminology, we are going to shape<b>*</b> table projection (which corresponds to <b>SELECT * FROM &#8230;</b> <a href="http://en.wikipedia.org/wiki/SQL">SQL</a> construct). Let see how it looks like for <b>customers</b>:
</p>

<pre class="lang:scala decode:true ">// Converts from Customer domain instance to table model and vice-versa
def * = ( id.?, email, firstName.?, lastName.? ).shaped &lt;&gt; ( 
  Customer.tupled, Customer.unapply )</pre>

<p style="text-align: justify;">
  For <b>addresses</b> table, shaping looks a little bit more verbose due to the fact that <a href="http://slick.typesafe.com/">Slick</a> does not have a way to refer to <b>Customer</b>case class instance by foreign key. Still, it is pretty straightforward, we just construct temporary <b>Customer</b> from its identifier.
</p>

<pre class="lang:scala decode:true">// Converts from Customer domain instance to table model and vice-versa
def * = ( id.?, street, city, country, customerId ).shaped &lt;&gt; ( 
  tuple =&gt; {
    Address.apply(
      id = tuple._1,
      street = tuple._2,
      city = tuple._3,
      country = tuple._4,
      customer = Customer( Some( tuple._5 ), "" )
    )
  }, {
    (address: Address) =&gt;
      Some { (
        address.id,
        address.street,
        address.city,
        address.country,
        address.customer.id getOrElse 0 
      )
    }
  }
)</pre>

<p style="text-align: justify;">
  Now, when all details have been explained, how can we materialize our <a href="http://www.scala-lang.org/">Scala</a> table definitions into the real database tables? Thankfully to <a href="http://slick.typesafe.com/">Slick</a>, it is a easy as that:
</p>

<pre class="lang:scala decode:true">implicit lazy val DB = Database.forURL( "jdbc:h2:mem:test", driver = "org.h2.Driver" )
  
DB withSession { implicit session =&gt;
  ( Customers.ddl ++ Addresses.ddl ).create
}</pre>

<p style="text-align: justify;">
  <a href="http://slick.typesafe.com/">Slick</a> has many ways to query and update data in database. The most beautiful and powerful one is just using pure functional constructs of the <a href="http://www.scala-lang.org/">Scala</a> language. The easiest way of doing that is by defining companion object and implement typical <a href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete">CRUD</a>operations in it. For example, here is the method which inserts new <b>customer</b> record into <b>customers</b> table:
</p>

<pre class="lang:default decode:true ">object Customers extends TableQuery[ Customers ]( new Customers( _ ) ) {
  def create( customer: Customer )( implicit db: Database ): Customer = 
    db.withSession { implicit session =&gt;
      val id = this.autoIncrement.insert( customer )
      customer.copy( id = Some( id ) )
    } 
}</pre>

And it could be used like this:

<pre class="lang:default decode:true">Customers.create( Customer( None, "tom@b.com",  Some( "Tom" ), Some( "Tommyknocker" ) ) )
Customers.create( Customer( None, "bob@b.com",  Some( "Bob" ), Some( "Bobbyknocker" ) ) )</pre>

<p style="text-align: justify;">
  Similarly, the family of <b>find</b> functions could be implemented using regular <a href="http://www.scala-lang.org/">Scala</a> <b>for</b> comprehension:
</p>

<pre class="lang:default decode:true ">def findByEmail( email: String )( implicit db: Database ) : Option[ Customer ] = 
  db.withSession { implicit session =&gt;
    ( for { customer &lt;- this if ( customer.email === email.toLowerCase ) } 
      yield customer ) firstOption
  }
   
def findAll( implicit db: Database ): Seq[ Customer ] = 
  db.withSession { implicit session =&gt;      
    ( for { customer &lt;- this } yield customer ) list
  }</pre>

And here are usage examples:

<pre class="lang:default decode:true ">val customers = Customers.findAll
val customer = Customers.findByEmail( "bob@b.com" )</pre>

<p style="text-align: justify;">
  Updates and deletes are a bit different though very simple as well, let us take a look on those:
</p>

<pre class="lang:default decode:true ">def update( customer: Customer )( implicit db: Database ): Boolean = 
  db.withSession { implicit session =&gt;
    val query = for { c &lt;- this if ( c.id === customer.id ) } 
      yield (c.email, c.firstName.?, c.lastName.?)
    query.update(customer.email, customer.firstName, customer.lastName) &gt; 0
  }
  
def remove( customer: Customer )( implicit db: Database ) : Boolean = 
  db.withSession { implicit session =&gt;
    ( for { c &lt;- this if ( c.id === customer.id ) } yield c ).delete &gt; 0
  }</pre>

Let us see those two methods in action:

<pre class="lang:default decode:true ">Customers.findByEmail( "bob@b.com" ) map { customer =&gt;
  Customers.update( customer.copy( firstName = Some( "Tommy" ) ) )
  Customers.remove( customer )
}</pre>

<p style="text-align: justify;">
  Looks very neat. I am personally still learning <a href="http://slick.typesafe.com/">Slick</a> however I am pretty excited about it. It helps me to have things done much faster, enjoying the beauty of <a href="http://www.scala-lang.org/">Scala</a> language and functional programming. No doubts, the upcoming version <b>3.0</b> is going to bring even more interesting features, I am looking forward to it.
</p>

<p style="text-align: justify;">
  This post is just an introduction into world of <a href="http://slick.typesafe.com/">Slick</a>, a lot of implementation details and use cases have been left aside to keep it short and simple. However <a href="http://slick.typesafe.com/doc/2.1.0/index.html">Slick&#8217;s documentation</a> is pretty good and please do not hesitate to consult it.
</p>

<p style="text-align: justify;">
  The complete project is available on <a href="https://github.com/reta/db-scala-slick">GitHub</a>.
</p>

<p class="note_normal" style="text-align: justify;">
  Published on Codingpedia.org with the permission of Andriy RedkoAndriy Redko</a> – source <a title="http://aredko.blogspot.ch/2015/02/a-fresh-look-on-accessing-database-on.html" href="http://aredko.blogspot.ch/2015/02/a-fresh-look-on-accessing-database-on.html" target="_blank">A fresh look on accessing database on JVM platform: Slick from Typesafe</a> from <a title="http://aredko.blogspot.com" href="http://aredko.blogspot.com/" target="_blank">http://aredko.blogspot.com</a>
</p>
