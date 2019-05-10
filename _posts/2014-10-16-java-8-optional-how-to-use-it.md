---
id: 1998
title: 'Java 8 Optional: How to Use it'
date: 2014-10-16T21:08:13+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=1998
permalink: /jhadesdev/java-8-optional-how-to-use-it/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3123471499
fsb_social_facebook:
  - 2
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
tags:
  - feature
  - java-core
  - java8
  - optional
---
<p style="text-align: justify;">
  Java 8 comes with a new <code>Optional</code> type, similar to what is available in other languages. This post will go over how this new type is meant to be used, namely what is it&#8217;s main use case.
</p>

<h4 id="whatistheoptionaltype" style="color: #3a4145;">
  What is the Optional type?
</h4>

<!--more-->

<p style="color: #3a4145; text-align: justify;">
  Optional is a new container type that wraps a single value, if the value is available. So it&#8217;s meant to convey the meaning that the value might be absent. Take for example this method:
</p>

<pre class="lang:java decode:true ">public Optional&lt;Customer&gt; findCustomerWithSSN(String ssn) {
    ...
}</pre>

<p style="text-align: justify;">
  Returning <code>Optional</code> adds explicitly the possibility that there might <strong>not</strong>be a customer for that given social security number.
</p>

<p style="color: #3a4145; text-align: justify;">
  This means that the caller of the method is <strong>explicitly forced by the type system</strong> to think about and deal with the possibility that there might not be a customer with that SSN.
</p>

<p style="color: #3a4145;">
  The caller will have to to something like this:
</p>

<pre class="lang:java decode:true ">Optional&lt;Customer&gt; optional = findCustomerWithSSN(ssn);

if (optional.isPresent()) {
    Customer customer = maybeCustomer.get();
    ... use customer ...
}
else {
    ... deal with absence case ...
}</pre>

<span style="color: #3a4145;">Or otherwise provide a default value:</span>

<pre class="lang:java decode:true ">Long value = findOptionalLong(ssn).orElse(0L);</pre>

This use of optional is somewhat similar to the more familiar case of throwing checked exceptions. By throwing a checked exception, we use the compiler to enforce callers of the API to somehow handle an exceptional case.

<h4 id="whatisoptionaltryingtosolve" style="color: #3a4145;">
  What is Optional trying to solve?
</h4>

Optional is an attempt to reduce the number of null pointer exceptions in Java systems, by adding the possibility to build more expressive APIs that account for the possibility that sometimes return values are missing.

If Optional was there since the beginning, most libraries and applications would likely deal better with missing return values, reducing the number of null pointer exceptions and the overall number of bugs in general.

<h4 id="howshouldoptionalbeusedthen" style="color: #3a4145;">
  How should Optional be used then?
</h4>

<p style="color: #3a4145;">
  Optional <strong>should be used as the return type</strong> of functions that might not return a value.
</p>

<p style="color: #3a4145;">
  This is a <a href="http://mail.openjdk.java.net/pipermail/jdk8-dev/2013-September/003274.html">quote from OpenJDK mailing list</a>:
</p>

<p style="font-style: italic; padding-left: 30px;">
  The JSR-335 EG felt fairly strongly that Optional should not be on any more than needed to support the optional-return idiom only.
</p>

<p style="font-style: italic; padding-left: 30px;">
  Someone suggested maybe even renaming it to OptionalReturn&#8221;
</p>

In the context of domain driver development, this means that Optional should be used as the return type of certain service, repository or utility methods such as the one shown above.

<h4 id="whatisoptionalnottryingtosolve" style="color: #3a4145;">
  What is Optional not trying to solve
</h4>

Optional is not meant to be a mechanism to avoid all types of null pointers. The mandatory input parameters of methods and constructors still have to be tested for example.

Like when using null, Optional does not help with conveying the_meaning_ of an absent value. In a similar way that null can mean many different things (value not found, etc.), so can an absent Optional value.

The caller of the method will still have to check the javadoc of the method for understanding the meaning of the absent Optional, in order to deal with it properly.

Also in a similar way that a checked exception can be caught in an empty block, nothing prevents the caller of calling `get()` and moving on.

<h4 id="whatiswrongwithjustreturningnull" style="color: #3a4145;">
  What is wrong with just returning null?
</h4>

The problem is that the caller of the function might not have read the javadoc for the method, and forget about handling the null case.

<p style="color: #3a4145;">
  This happens frequently and is one of the main causes of null pointer exceptions, although not the only one.
</p>

<h4 id="howshouldoptionalnotbeused" style="color: #3a4145;">
  How should Optional NOT be used?
</h4>

<p style="color: #3a4145;">
  Optional is not meant to be used in these contexts, as it won&#8217;t buy us anything:
</p>

<ul style="color: #3a4145;">
  <li>
    in the domain model layer (not serializable)
  </li>
  <li>
    in DTOs (same reason)
  </li>
  <li>
    in input parameters of methods
  </li>
  <li>
    in constructor parameters
  </li>
</ul>

<h4 id="howdoesoptionalhelpwithfunctionalprogramming" style="color: #3a4145;">
  How does Optional help with functional programming?
</h4>

In chained function calls, Optional provides method `ifPresent()`, that allows to chain functions that might not return values:

<pre class="lang:java decode:true ">findCustomerWithSSN(ssn).ifPresent(() -&gt; System.out.println("customer exists!"));</pre>

<h4 id="usefullinks" style="color: #3a4145;">
  Useful Links
</h4>

This blog post from Oracle goes further into `Optional`and it&#8217;s uses, comparing it with similar functionality in other languages &#8211; [Tired of Null Pointer Exceptions?](http://www.oracle.com/technetwork/articles/java/java8-optional-2175753.html)

This cheat sheet provides a thorough overview of Optional &#8211; [Optional in Java 8 Cheat Sheet](http://java.dzone.com/articles/optional-java-8-cheat-sheet)

<p class="note_normal">
  Published at Codingpedia.org with permission of Aleksey Novik &#8211; source <a title="http://blog.jhades.org/java-8-how-to-use-optional/" href="http://blog.jhades.org/java-8-how-to-use-optional/" target="_blank">Java 8 Optional: How to Use it</a> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>

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
