---
id: 1783
title: Javascript for Java Developers
date: 2014-09-11T09:42:53+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=1783
permalink: /jhadesdev/javascript-for-java-developers/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3007592618
fsb_social_facebook:
  - 5
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - javascript
tags:
  - closure
  - developer
  - enterprise
  - front-end
  - prototype
  - this
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Why_Javascript_in_the_Java_World">Why Javascript in the Java World ?</a>
    </li>
    <li>
      <a href="#Objects_Only_8211_No_Classes">Objects Only &#8211; No Classes</a>
    </li>
    <li>
      <a href="#Functions_Are_Just_Values">Functions Are Just Values</a>
    </li>
    <li>
      <a href="#The_8216this8217_Keyword_Usage">The &#8216;this&#8217; Keyword Usage</a><ul>
        <li>
          <a href="#What_if_we_pass_the_function_around">What if we pass the function around?</a>
        </li>
        <li>
          <a href="#Why_does_this_not_work_anymore">Why does this not work anymore?</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Classic_vs_Prototypal_Inheritance">Classic vs Prototypal Inheritance</a><ul>
        <li>
          <a href="#How_does_prototype_work">How does prototype work?</a>
        </li>
        <li>
          <a href="#How_does_this_compare_with_Java_inheritance">How does this compare with Java inheritance?</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Constructors_vs_Constructor_Functions">Constructors vs Constructor Functions</a><ul>
        <li>
          <a href="#Why_is_this_syntax_not_recommended_then">Why is this syntax not recommended then?</a>
        </li>
        <li>
          <a href="#Is_there_a_recommended_alternative_to_new">Is there a recommended alternative to new?</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Closures_vs_Lambdas">Closures vs Lambdas</a><ul>
        <li>
          <a href="#What_is_the_main_difference_between_Lambdas_and_Closures">What is the main difference between Lambdas and Closures?</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Modules_and_Encapsulation">Modules and Encapsulation</a><ul>
        <li>
          <a href="#How_do_Closures_relate_to_Encapsulation">How do Closures relate to Encapsulation?</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Block_Scope_and_Hoisting">Block Scope and Hoisting</a>
    </li>
    <li>
      <a href="#Conclusion">Conclusion</a>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  This post will go over the Javascript language from the point of view of a Java developer, focusing on the differences between the two languages and the frequent pain points. We will go over the following:
</p>

  * Objects Only, No Classes
  * Functions are just Values
  * The &#8216;this&#8217; Keyword
  * Classic vs Prototypal Inheritance
  * Constructors vs Constructor Functions
  * Closures vs Lambdas
  * Encapsulation and Modules
  * Block Scope and Hoisting

<!--more-->

### <span id="Why_Javascript_in_the_Java_World">Why Javascript in the Java World ?</span> {#whyjavascriptinthejavaworld}

A lot of Java frontend development work is done using Java/XML based frameworks like JSF or GWT. The framework developers themselves need to know Javascript, but in principle the application developers don&#8217;t. However the reality is that:

  * For doing custom component development in for example Primefaces (JSF), it&#8217;s important to know Javascript and jQuery.
  * In GWT, integrating at least some third-party Javascript widgets is common and cost effective.

<p style="text-align: justify;">
  The end result is that Javascript is usually needed to do at least the last 5 to 10% of frontend work, even using Java frameworks. Also it&#8217;s starting to get more and more used for polyglot enterprise development, alongside <a href="http://angularjs.org">Angular</a> for example.
</p>

The good news is that, besides a few gotchas that we will get into, Javascript is a very learneable language for a Java developer.

### <span id="Objects_Only_8211_No_Classes">Objects Only &#8211; No Classes</span> {#objectsonlynoclasses}

One of the most surprising things about Javascript is that although it&#8217;s an object oriented language, there are no classes (although the [new Ecmascript 6 version](http://wiki.ecmascript.org/doku.php?id=harmony:classes) will have them).

Take for example this program, that initializes an empty object and set&#8217;s two properties:

<pre class="lang:js decode:true">// create an empty object - no class was needed !!
var superhero = {};
 
superhero.name = 'Superman'; 
superhero.strength = 100;</pre>

Javascript objects are just like a Java HashMap of related properties, where the keys are Strings only. The following would be the &#8216;equivalent&#8217; Java code:

<pre class="lang:java decode:true ">Map&lt;String,Object&gt; superhero = new HashMap&lt;&gt;();
 
superhero.put("name","Superman"); 
superhero.put("strength", 100);</pre>

This means that a Javascript object is just a multi-level &#8216;hash map&#8217; of key/value pairs, with no class definition needed.

### <span id="Functions_Are_Just_Values">Functions Are Just Values</span> {#functionsarejustvalues}

Functions in Javascript are just values of type `Function`, it&#8217;s a simple as that! Take for example:

<pre class="lang:js decode:true ">var flyFunction = function() { 
    console.log('Flying like a bird!');
};
 
superhero.fly = flyFunction;</pre>

This creates a function (a value of type `Function`) and assigns it to a variable `flyFunction`. A new property named `fly` is then created in the superhero object, that can be invoked like this:

<pre class="lang:js decode:true">// prints 'Flying like a bird!' to the console
superhero.fly();</pre>

Java does **not** have the equivalent of the Javascript `Function` type, but almost. Take for example the `SuperHero` class that takes a `Power` function:

<pre class="lang:java decode:true ">public interface Power { 
    void use();
}
 
public class SuperHero {
 
    private Power flyPower;
 
    public void setFly(Power flyPower) {
        this.flyPower = flyPower;
    }
 
    public void fly() {
        flyPower.use();
    }
}</pre>

This is how to pass `SuperHero` a function in Java 7 and 8:

<pre class="lang:java decode:true">// Java 7 equivalent
Power flyFunction = new Power() { 
    @Override
    public void use() {
        System.out.println("Flying like a bird ...");
    }
};
 
// Java 8 equivalent
superman.setFly( 
    ()-&gt;System.out.println("Flying like a bird ..."));
 
superman.fly();</pre>

So although a `Function` type does not exist in Java 8, this ends up not preventing a &#8216;Javascript-like&#8217; functional programming style.

But if we pass functions around, what happens to the meaning of the
  
`this` keyword?

### <span id="The_8216this8217_Keyword_Usage">The &#8216;this&#8217; Keyword Usage</span> {#thethiskeywordusage}

What Javascript allows to do with `this` is quite surprising compared to the Java world. Let&#8217;s start with an example:

<pre class="lang:js decode:true  ">var superman = {
 
  heroName: 'Superman', 
 
  sayHello: function() {
      console.log("Hello, I'm " + this.heroName );
  } 
};
 
superman.sayHello();</pre>

This program creates an object `superman` with two properties: a String
  
`heroName` and a `Function` named `sayHello`. Running this program outputs as expected `Hello, I'm Superman`.

#### <span id="What_if_we_pass_the_function_around">What if we pass the function around?</span> {#whatifwepassthefunctionaround}

By passing around `sayHello`, we can easily end up in a context where there is no `heroName` property:

<pre class="lang:js decode:true">var failThis = superman.sayHello;
 
failThis();</pre>

&nbsp;

Running this snippet would give as output: `Hello, I'm undefined`.

#### <span id="Why_does_this_not_work_anymore">Why does <code>this</code> not work anymore?</span> {#whydoesthisnotworkanymore}

This is because the variable `failThis` belongs to the global scope, which contains no member variable named `heroName`. To solve this:

> In Javascript the value of the `this` keyword is completely overridable to be anything that we want!

<pre class="lang:js decode:true">// overrides 'this' with superman
hello.call(superman);
</pre>

The snippet above would print again `Hello, I'm Superman`. This means that the value of `this` depends on both the context on which the function is called, and on _how_ the function is called.

### <span id="Classic_vs_Prototypal_Inheritance">Classic vs Prototypal Inheritance</span> {#classicvsprototypalinheritance}

In Javascript, there is no class inheritance, instead objects can inherit directly from other objects. The way this works is that each object has an implicit property that points to a &#8216;parent&#8217; object.

That property is called `__proto__`, and the parent object is called the object&#8217;s **prototype**, hence the name Prototypal Inheritance.

#### <span id="How_does_prototype_work">How does <code>prototype</code> work?</span> {#howdoesprototypework}

When looking up a property, Javascript will try to find the property in the object itself. If it does not find it then it tries in it&#8217;s prototype, and so on. For example:

<pre class="lang:js decode:true ">var avengersHero = { 
    editor: 'Marvel'
};
 
var ironMan = {};
 
ironMan.__proto__ = avengersHero;
 
console.log('Iron Man is copyrighted by ' + ironMan.editor);</pre>

This snippet will output `Iron Man is copyrighted by Marvel`.

As we can see, although the `ironMan` object is empty, it&#8217;s prototype does contain the property `editor`, which get&#8217;s found.

#### <span id="How_does_this_compare_with_Java_inheritance">How does this compare with Java inheritance?</span> {#howdoesthiscomparewithjavainheritance}

Let&#8217;s now say that the rights for the Avengers where bought by DC Comics:

<pre class="lang:js decode:true ">avengersHero.editor = 'DC Comics';</pre>

If we call `ironMan.editor` again, we now get `Iron Man is copyrighted by DC Comics`. All the existing object instances with the `avengersHero` prototype now see `DC Comics` _without_ having to be recreated.

This mechanism is very simple and very powerful. Anything that can be done with class inheritance can be done with prototypal inheritance. But what about constructors?

### <span id="Constructors_vs_Constructor_Functions">Constructors vs Constructor Functions</span> {#constructorsvsconstructorfunctions}

In Javascript an attempt was made to make object creation similar to languages like Java. Let&#8217;s take for example:

<pre class="lang:js decode:true ">function SuperHero(name, strength) { 
    this.name = name;
    this.strength = strength;
}</pre>

Notice the capitalized name, indicating that it&#8217;s a constructor function. Let&#8217;s see how it can be used:

<pre class="lang:js decode:true ">var superman = new SuperHero('Superman', 100);
 
console.log('Hello, my name is ' + superman.name);</pre>

This code snippet outputs `Hello, my name is Superman`.

You might think that this looks just like Java, and that is exactly the point! What this `new` syntax really does is to it creates a new empty object, and then calls the constructor function by forcing `this` to be the newly created object.

#### <span id="Why_is_this_syntax_not_recommended_then">Why is this syntax not recommended then?</span> {#whyisthissyntaxnotrecommendedthen}

Let&#8217;s say that we want to specify that all super heroes have a
  
`sayHello` method. This could be done by putting the `sayHello` function in a common prototype object:

<pre class="lang:js decode:true ">function SuperHero(name, strength) { 
    this.name = name;
    this.strength = strength;
}
 
SuperHero.prototype.sayHello = function() { 
    console.log('Hello, my name is ' + this.name);
}
 
var superman = new SuperHero('Superman', 100); 
superman.sayHello();</pre>

This would output `Hello, my name is Superman`.

But the syntax `SuperHero.prototype.sayHello` looks anything but Java like! The `new` operator mechanism sort of half looks like Java but at the same time is completely different.

#### <span id="Is_there_a_recommended_alternative_to_new">Is there a recommended alternative to <code>new</code>?</span> {#istherearecommendedalternativetonew}

The recommended way to go is to ignore the Javascript `new` operator altogether and use `Object.create`:

<pre class="lang:js decode:true ">var superHeroPrototype = { 
   sayHello: function() {
        console.log('Hello, my name is ' + this.name);
    }
};
 
var superman = Object.create(superHeroPrototype); 
superman.name = 'Superman';</pre>

Unlike the `new` operator, one thing that Javascript absolutely got right were Closures.

### <span id="Closures_vs_Lambdas">Closures vs Lambdas</span> {#closuresvslambdas}

Javascript Closures are not that different from Java anonymous inner classes used in a certain way. take for example the `FlyingHero` class:

<pre class="lang:java decode:true ">public interface FlyCommand { 
    public void fly();
}
 
public class FlyingHero {
 
    private String name;
 
    public FlyingHero(String name) {
        this.name = name;
    }
 
    public void fly(FlyCommand flyCommand) {
        flyCommand.fly();
    }
}</pre>

We can can pass it a fly command like this in Java 8:

<pre class="lang:java decode:true ">String destination = "Mars"; 
superMan.fly(() -&gt; System.out.println("Flying to " + 
    destination ));</pre>

The output of this snippet is `Flying to Mars`. Notice that the `FlyCommand` lambda had to &#8216;remember&#8217; the variable `destination`, because it needs it for executing the `fly` method later.

This notion of a function that remembers about variables outside it&#8217;s block scope for later use is called a **Closure** in Javascript. For further details, have a look at this blog post [Really Understanding Javascript Closures](http://blog.jhades.org/really-understanding-javascript-closures/).

#### <span id="What_is_the_main_difference_between_Lambdas_and_Closures">What is the main difference between Lambdas and Closures?</span> {#whatisthemaindifferencebetweenlambdasandclosures}

In Javascript a closure looks like this:

<pre class="lang:js decode:true ">var destination = 'Mars';
 
var fly = function() { 
    console.log('Fly to ' + destination);
}
 
fly();</pre>

The Javascript closure, unlike the Java Lambda does not have the constraint that the `destination` variable must be immutable (or effectively immutable since Java 8).

This seemingly innocuous difference is actually a &#8216;killer&#8217; feature of Javascript closures, because it allows them to be used for creating encapsulated modules.

### <span id="Modules_and_Encapsulation">Modules and Encapsulation</span> {#modulesandencapsulation}

There are no classes in Javascript and no `public`/ `private` modifiers, but then again take a look at this:

<pre class="lang:js decode:true ">function createHero(heroName) {
 
    var name = heroName;
 
    return  {
        fly: function(destination) {
          console.log(name + ' flying to ' + destination);
        }
    };
}</pre>

Here a function `createHero` is being defined, which returns an object which has a function `fly`. The `fly` function &#8216;remembers&#8217; `name` when needed.

#### <span id="How_do_Closures_relate_to_Encapsulation">How do Closures relate to Encapsulation?</span> {#howdoclosuresrelatetoencapsulation}

<span style="color: #3a4145;">When the </span><code style="color: #3a4145;">createHero</code><span style="color: #3a4145;"> function returns, noone else will ever be able to directly access </span><code style="color: #3a4145;">name</code><span style="color: #3a4145;">, except via </span><code style="color: #3a4145;">fly</code><span style="color: #3a4145;">. Let&#8217;s try this out:</span>

<pre class="lang:js decode:true ">var superman = createHero('SuperMan');
 
superman.fly('The Moon');</pre>

<span style="color: #3a4145;">The output of this snippet is </span><code style="color: #3a4145;">SuperMan flying to The Moon</code><span style="color: #3a4145;">. But happens if we try to access </span><code style="color: #3a4145;">name</code><span style="color: #3a4145;"> directly ?</span>

<pre class="lang:js decode:true ">console.log('Hero name = ' + superman.name);</pre>

<p style="color: #3a4145;">
  The result is <code>Hero name = undefined</code>. The function <code>createHero</code> is said to a be a Javascript encapsulated <strong>module</strong>, with closed &#8216;private&#8217; member variables and a &#8216;public&#8217; interface returned as an object with functions.
</p>

<h3 id="blockscopeandhoisting" style="color: #3a4145;">
  <span id="Block_Scope_and_Hoisting">Block Scope and Hoisting</span>
</h3>

<p style="color: #3a4145;">
  Understanding block scope in Javascript is simple: there is no block scope! Take a look at this example:
</p>

<pre class="lang:js decode:true ">function counterLoop() {
 
    console.log('counter before declaration = ' + i); 
 
    for (var i = 0; i &lt; 3 ; i++) {
        console.log('counter = ' + i); 
    }
 
    console.log('counter after loop = ' + i); 
}
 
counterLoop();</pre>

<p style="color: #3a4145;">
  By looking at this coming from Java, you might expect:
</p>

<ul style="color: #3a4145;">
  <li>
    error at line 3: &#8216;variable i does not exist&#8217;
  </li>
  <li>
    values 0, 1, 2 are printed
  </li>
  <li>
    error at line 9: &#8216;variable i does not exist&#8217;
  </li>
</ul>

<p style="color: #3a4145;">
  It turns out that only one of these three things is true, and the output is actually this:
</p>

<pre style="color: #3a4145;"><code class=" hljs makefile" style="color: #333333;">counter before declaration = undefined  
&lt;span class="hljs-constant">counter&lt;/span> = 0 
&lt;span class="hljs-constant">counter&lt;/span> = 1 
&lt;span class="hljs-constant">counter&lt;/span> = 2 
counter after loop = 3 
</code></pre>

<p style="color: #3a4145;">
  Because there is no block scope, the loop variable i is visible for the <strong>whole</strong> function. This means:
</p>

<ul style="color: #3a4145;">
  <li>
    line 3 sees the variable declared but not initialized
  </li>
  <li>
    line 9 sees i after the loop has terminated
  </li>
</ul>

<p style="color: #3a4145;">
  What might be the most puzzling is that line 3 actually sees the variable declared but undefined, instead of throwing <code>i is not defined</code>.
</p>

<p style="color: #3a4145;">
  This is because the Javascript interpreter first scans the function for a list of variables, and then goes back to interpret the function code lines one by one.
</p>

<p style="color: #3a4145;">
  The end result is that it&#8217;s like the variable i was <strong>hoisted</strong> to the top, and this is what the Javascript runtime actually &#8216;sees&#8217;:
</p>

<pre class="lang:js decode:true ">function counterLoop() {
 
    var i; // i is 'seen' as if declared here! 
 
    console.log('counter before declaration = ' + i); 
 
    for (i = 0; i &lt; 3 ; i++) {
        console.log('counter = ' + i); 
    }
 
    console.log('counter after loop:  ' + i); 
}</pre>

<p style="color: #3a4145;">
  To prevent surprises caused by hoisting and lack of block scoping, it&#8217;s a recommended practice to declare variables always at the top of functions.
</p>

<p style="color: #3a4145;">
  This makes hoisting explicit and visible by the developer, and helps to avoid bugs. The next version of Javascript (Ecmascript 6) will include a<a href="http://wiki.ecmascript.org/doku.php?id=harmony:let">new keyword &#8216;let&#8217; to allow block scoping</a>.
</p>

<h3 id="conclusion" style="color: #3a4145;">
  <span id="Conclusion">Conclusion</span>
</h3>

<p style="color: #3a4145; text-align: justify;">
  The Javascript language shares a lot of similarities with Java, but also some huge differences. Some of the differences like inheritance and constructor functions are important, but much less than one would expect for day to day programming.
</p>

<p style="color: #3a4145; text-align: justify;">
  Some of these features are needed mostly by library developers, and not necessarily for day to day application programming. This is unlike some of their Java counterparts which are needed every day.
</p>

<p style="color: #3a4145;">
  So if you are hesitant to give it a try, don&#8217;t let some of these features prevent you from going further into the language.
</p>

<p style="color: #3a4145;">
  One thing is for sure, at least <em>some</em> Javascript is more or less inevitable when doing Java frontend development, so it&#8217;s really worth to give it a try.
</p>

<p class="note_normal" style="color: #3a4145;">
  Published at Codingpedia.org with permission of Aleksey Novik &#8211; source <em><a title="http://blog.jhades.org/javascript-for-java-developers/" href="http://blog.jhades.org/javascript-for-java-developers/" target="_blank">Javascript for Java Developers</a></em> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
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

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;
