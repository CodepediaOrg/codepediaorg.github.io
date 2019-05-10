---
id: 1817
title: How I reduced my Java batch application’s code by 80% using Easy Batch!
date: 2014-09-14T14:01:53+00:00
author: Mahmoud Ben Hassine
layout: post
guid: http://www.codingpedia.org/?p=1817
permalink: /mahmoudbenhassine/how-i-reduced-my-java-batch-applications-code-by-80-using-easy-batch/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3016625006
fsb_social_facebook:
  - 2
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
tags:
  - batch
  - batchprocessing
  - easybatch
---
<p style="text-align: justify;">
  In this post, I will show how can <a title="Easy Batch" href="http://www.easybatch.org" target="_blank">Easy Batch</a> tremendously simplify your batch applications by reducing plumbing code considerably. This will make your applications code more readable, understandable and maintainable!
</p>

<p style="text-align: justify;">
  The use case is a typical production application that loads data from an input CSV flat file to a database.<!--more-->
</p>

Consider the following CSV input file containing products data:

<div>
  Could not embed GitHub Gist 8547399: Not Found
</div>

<p style="text-align: justify;">
  Suppose we have a JPA EntityManager that will be used to persist Product objects to the database. We would like to map each record of this file to an instance of the following Product POJO :
</p>

<div>
  Could not embed GitHub Gist 8548282: Not Found
</div>

Before persisting products to the database, we should validate data to ensure that:

  * product id and name are specified
  * product price is not negative
  * product last update date is in the past

<p style="text-align: justify;">
  Finally, we should skip records starting with # from being processed, mainly the header record (there could be other records starting with # like a trailer record that marks the end of data in the file).
</p>

To keep the example simple, we will write products data to the standard output and not to a database.

So let’s get started!

The following listing is a possible solution that I have seen a handred times in production systems:

<div>
  Could not embed GitHub Gist 8547412: Not Found
</div>

<p style="text-align: justify;">
  This solution actually works perfectly and implements the requirements above. But it is evident that it is a maintainance nightmare! It could be worse if the Product POJO contained dozen of fields, which is often the case in production.
</p>

In this 95 lines solution, there is only one line which represents the batch business logic. Did you guessed it?

It is line 78 : <span class="skimlinks-unlinked">System.out.println(“product</span> = ” + product);// or in production, it would be persisting the object to the database

<p style="text-align: justify;">
  ALL the rest is plumbing : reading, filtering, parsing and validating data, mapping records to Product instances and reporting some statistics at the end of execution.
</p>

<p style="text-align: justify;">
  This is where Easy Batch comes to play to handle ALL of this plumbing for you! With Easy Batch, you concentrate only on your batch business logic. So let’s see how would be the solution with Easy Batch.
</p>

First, we will create a RecordProcessor that will implement our batch business logic :

<div>
  Could not embed GitHub Gist 8547436: Not Found
</div>

<p style="text-align: justify;">
  Then, we will DECLARE (not implement like in the above solution!) data validation constraints on our Product POJO with the elegant <a title="JSR303" href="http://beanvalidation.org/1.1/spec/" target="_blank">Bean Validation API</a> as follows:
</p>

<div>
  Could not embed GitHub Gist 8544434: Not Found
</div>

Finally, we should just configure Easy Batch to:

  * Read data from the flat file <span class="skimlinks-unlinked">products.csv</span>
  * Filter records starting with #
  * Map each CSV record to an instance of the Product POJO
  * Validate products data
  * and process each record using our ProductProcessor implementation

This can be done with the following snippet:

<div>
  Could not embed GitHub Gist 8547426: Not Found
</div>

<p style="text-align: justify;">
  That’s all! Except implementing the core batch business logic, all we have done is providing configuration metadata that Easy Batch cannot guess! The framework will handle all the plumbing of reading, filtering, parsing, validation and mapping data to the domain object Product.
</p>

<p style="text-align: justify;">
  Now let’s do the count of lines of code. Both solutions use the Product POJO and their respective Main classes have 6 lines of imports, so we will not take into account these imports neither the number of line of code of the Product POJO.
</p>

<li style="text-align: justify;">
  The first solution WithoutEasyBatchLauncher has 82 lines of code (empty lines have been ignored). Note that I have inlined all variables in this solution and tried to make it as compact as possible, this is for those who will think I had delibarately maximized the number of lines of code of this solution <span class="wp-smiley emoji emoji-smile" title=":-)">🙂</span>
</li>
  * For the second solution we have:

  1. 5 lines for the ProductProcessor class (empty lines ignored)
  2. 4 lines for Bean Validation API annotations we added on the Product POJO
  3. and 9 lines for the Main class WithEasyBatchLauncher

In sum, 82LOC vs 18LOC, which is 78% less than the first solution!

I am pretty sure there are some readers who will say, but it is not 80% like in the title of the post <span class="wp-smiley emoji emoji-wink" title=";-)">😉</span>

Actually I had to put 90% or even 95% because this is not all what Easy Batch have done for us, in addition to all what we have seen above, Easy Batch

<li style="text-align: justify;">
  provides a more detailed report about processing time for each record, the average record processing time and a percentage of filtered, ignored, rejected and processed records
</li>
  * allows to monitor the batch execution with JMX at runtime

<p style="text-align: justify;">
  So, at the end of this post, this is what Easy Batch is all about, making your life easier when you have to deal with batch applications in Java by letting you concentrate on your batch business logic and handling ALL the rest for you.
</p>

<p style="text-align: justify;">
  Even though the sample I used in this post is about processing a flat file, Easy Batch can also handle the plumbing of processing data from a database or an xml file, which can be more complex than the plumbing code of processing data from a flat file.
</p>

You can check out Easy Batch tutorials <a title="Easy Batch tutorials" href="http://www.easybatch.org/tutorials/index.html" target="_blank">here</a>!

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with permission of <a href="http://www.codingpedia.org/author/mahmoudbenhassine" target="_blank">Mahmoud Ben Hassine</a> – source  &#8220;<a title="http://mahmoudbenhassine.wordpress.com/2014/01/21/how-i-reduced-my-java-batch-applications-code-by-80-using-easy-batch/" href="http://mahmoudbenhassine.wordpress.com/2014/01/21/how-i-reduced-my-java-batch-applications-code-by-80-using-easy-batch/" target="_blank">How I reduced my Java batch application’s code by 80% using Easy Batch!</a>&#8221; from <a title="http://www.mahmoudbenhassine.com/" href="http://www.mahmoudbenhassine.com/" target="_blank">http://www.<wbr />mahmoudbenhassine.com/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://pbs.twimg.com/profile_images/682858329220190208/YhIK6TIE_400x400.jpg" alt="Mahmoud Ben Hassine" /> 
  
  <p id="about_author_header">
   Mahmoud Ben Hassine
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Passionate Software Engineer who loves to learn and teach. As an open source advocate, I love to create and contribute to open source projects and to share my experience with other developers around the globe. If you are a chess Junkie like me, it will be a pleasure to challenge you on a chess board!
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://benas.github.io/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/_benas_" target="_blank"> </a> <a class="icon-github" href="https://github.com/benas" target="_blank"> </a> <a class="icon-linkedin" href="http://fr.linkedin.com/in/mahmoudbenhassine/" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>
