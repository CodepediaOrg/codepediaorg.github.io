---
id: 2030
title: JVM PermGen – where art thou?
date: 2014-10-25T07:36:17+00:00
author: Abhishek Gupta
layout: post
guid: http://www.codingpedia.org/?p=2030
permalink: /abhirockzz/jvm-permgen-where-art-thou/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3153894421
fsb_social_facebook:
  - 1
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
  - java-core
  - java8
  - jvm
  - PermGen
---
<p style="text-align: justify;">
  This post covers some basics of <em>JVM memory structure </em>and quickly peeks into <em>PermGen</em> to find out where it has disappeared since advent of Java SE 8
</p>

## **Bare Basics**

<p style="text-align: justify;">
  The JVM is just another process running on your system and the magic begins with the java command. Like any OS process, it needs memory for its run time operations. Remember – the JVM itself is a software abstraction of a hardware on top of which Java programs run and boast of <em>OS independence and WORA</em> (write once run anywhere)<!--more-->
</p>

### **Quick coverage of the JVM memory structure**

As per the spec, JVM is divided into 5 virtual memory segments.

  * Heap
  * Method (non heap)
  * JVM Stack
  * Native Stack
  * PC Registers

&nbsp;

[<img class="aligncenter wp-image-847 size-full" src="http://abhirockzz.files.wordpress.com/2014/08/jvm-memory-segments1.jpg?w=960&h=269" alt="jvm-memory-segments" width="640" height="178" />](https://abhirockzz.files.wordpress.com/2014/08/jvm-memory-segments1.jpg)

&nbsp;

**Heap**

<li style="text-align: justify;">
  Every object allocated in your Java program requires to be stored in the memory. The heap is the area where all the instantiated objects get stored. Yes – blame the <em><strong>new</strong></em> operator for filling up your Java heap <span class="wp-smiley wp-emoji wp-emoji-wink" title=";-)">😉</span>
</li>
  * _Shared_ by all threads
  * The JVM throws _<span class="skimlinks-unlinked">java.lang.OutOfMemoryError</span>_ when it’s exhausted
  * Use the _-Xms_ and _-Xmx_ JVM options to tune the Heap size

&nbsp;

[<img class="aligncenter size-full wp-image-904" src="http://abhirockzz.files.wordpress.com/2014/09/out-of-memory-error.jpg?w=640" alt="out-of-memory-error" width="266" height="190" />](https://abhirockzz.files.wordpress.com/2014/09/out-of-memory-error.jpg)

&nbsp;

_Sub-divided into_

<li style="text-align: justify;">
  <em>Eden</em> (Young) – New object or the ones with short life expectancy exist in this area and it is regulated using the -XX:NewSize and -XX:MaxNewSize parameters. <em>GC (garbage collector) minor</em> sweeps this space
</li>
<li style="text-align: justify;">
  <em>Survivor</em> – The objects which are still being referenced manage to survive garbage collection in the Eden space end up in this area. This is regulated via the -XX:SurvivorRatio JVM option
</li>
<li style="text-align: justify;">
  <em>Old</em> (Tenured) – This is for objects which survive long garbage collections in both the Eden and Survivor space (due to lingering references of course). A special garbage collector takes care of this space. Object de-alloaction in the tenured space is taken care of by <em>GC major</em>
</li>

#### **Method Area**

  * Also called the _non heap_ area (in HotSpot JVM implementation)
  * It is divided into 2 major sub spaces

<p style="text-align: justify;">
  <em>Permanent</em> <em>Generation</em> – This area stores class related data from class definitions, structures, methods, field, method (data and code) and constants. Can be regulated using -XX:PermSize and -XX:MaxPermSize. IT can cause <span class="skimlinks-unlinked">java.lang.OutOfMemoryError</span>: PermGen space if it runs out if space
</p>

<p style="text-align: justify;">
  <em>Code</em> <em>Cache</em> – The cache area is used to store compiled code. The compiled code is nothing but <em>native</em> <em>code</em> (hardware specific) and is taken care of by the <em>JIT</em> (Just In Time) compiler which is specific to the Oracle HotSpot JVM
</p>

#### **JVM Stack**

  * Has a lot to do with methods in the Java classes
  * Stores local variables and regulates method invocation, partial result and return values
  * Each thread in Java has its _own (private) copy of the stack_ and is not accessible to other threads.
  * Tuned using -Xss JVM option

#### **Native** **Stack**

  * Used for native methods (non Java code)
  * Per thread allocation

#### **PC** **Registers**

  * Program counter specific to a particular thread
  * Contains addresses for JVM instructions which are being exceuted (undefined in case of native methods)

So, that’s about it for the JVM memory segment basics. Coming to back to the Permanent Generation.

### **So where is PermGen ???**

<p style="text-align: justify;">
  Essentially, the PermGen has <em>been completely remove</em>d and <em>replaced</em> by another memory area known as the <strong><em>Metaspace</em></strong>
</p>

#### **Metaspace – quick facts**

  * It’s part of the _native heap memory_
  * Can be tuned using _-XX:MetaspaceSize_ and _-XX:MaxMetaspaceSize_
  * Clean up initiation driven by XX:MetaspaceSize option i.e. when the MetaspaceSize is reached.
<li style="text-align: justify;">
  <strong><em><span class="skimlinks-unlinked">java.lang.OutOfMemoryError</span>: Metadata</em></strong> space will be received if the native space is exhausted
</li>
<li style="text-align: justify;">
  The PermGen related JVM options i.e. -XX:PermSize and -XX:MaxPermSize will be ignored if present
</li>

<p style="text-align: justify;">
  This was obviously just the tip of the iceberg. For comprehensive coverage of the JVM, there is no reference better than <a href="http://docs.oracle.com/javase/specs/jvms/se8/jvms8.pdf" target="_blank"><em><strong>the specification</strong></em></a> itself <span class="wp-smiley wp-emoji wp-emoji-smile" title=":-)">🙂</span>
</p>

(Additional note – thanks to inputs by **Jon Diamond**)

<p style="text-align: justify;">
  <em><strong>Note</strong></em>: <em>The JVM specification itself is a standard for vendors (who implement the spec to build their own JVM) to follow. It should not be used to figure out vendor specific features (e.g. SAP JVM might implement a spec feature differently than that of the Oracle HotSpot VM). The JVM specification provides scope for innovation by not enforcing each and every minor details w.r.t implementation. In fact, we are living in the age of Polyglot Languages – which compile to byte code supported by the JVM (Scala, Groovy, JPython, JRuby, Kotlin etc). But for most of us out there – it can be leveraged to gain a solid understanding of the Java fundamentals ranging from bytecode to class loading process</em>
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with the permission of Abhishek Gupta – source <a title="https://abhirockzz.wordpress.com/2014/09/06/jvm-permgen-where-art-thou/" href="https://abhirockzz.wordpress.com/2014/09/06/jvm-permgen-where-art-thou/" target="_blank">JVM PermGen – where art thou?</a> from <a title="https://abhirockzz.wordpress.com" href="https://abhirockzz.wordpress.com" target="_blank">https://abhirockzz.wordpress.com</a>
</p>

<p style="text-align: justify;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="http://2.gravatar.com/avatar/eb0d2c5bf9426d7718efc6f9b087efb5?s=96&d=identicon&r=G" alt="Abhishek Gupta" /> 
    
    <p id="about_author_header">
      <strong>Abhishek Gupta</strong>
    </p>
    
    <div id="author_details" style="text-align: justify;">
      Java Platform fanatic and a Consultant in the Identity Management domain
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://abhirockzz.wordpress.com/" target="_blank"> </a> <a class="icon-googleplus" href="http://plus.google.com/103167687788180890717/posts" target="_blank"> </a> <a class="icon-twitter" href="http://twitter.com/abhi_tweeter" target="_blank"> </a> <a class="icon-github" href="https://github.com/abhirockzz" target="_blank"> </a> <a class="icon-linkedin" href="http://in.linkedin.com/pub/abhishek-gupta/27/331/866" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>
