---
id: 2633
title: 'Creating Usable JVM Languages: An Overview'
date: 2016-02-07T21:29:49+00:00
author: Toptal Blog
layout: post
guid: http://www.codingpedia.org/?p=2633
permalink: /toptal/creating-usable-jvm-languages-an-overview/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4560086036
fsb_social_facebook:
  - 0
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
  - introduction
  - jvm
  - jvm language
  - programming language
---
<p style="text-align: justify;">
  There are several possible reasons for creating a language, some of which are not immediately obvious. I would like to present them together with an approach to make a language for the <a href="https://en.wikipedia.org/wiki/Java_virtual_machine" target="_blank">Java Virtual Machine</a>(JVM) reusing existing tools as much as possible. In this way we will reduce the development effort and provide a toolchain familiar to the user, making it easier to adopt our new programming language.
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91985/toptal-blog-image-1453386059410-0b9eeb49271c73a977943205df6b374d.jpg" alt="Creating Usable JVM Languages: An Overview" />
</p>

<p style="text-align: justify;">
  In this article, the first of the series, I will present an overview of the strategy and various tools involved in creating our very own programming language for the JVM. in future articles, we will dive into the implementation details.<!--more-->
</p>

<h2 id="why-create-your-jvm-language" style="text-align: justify;">
  Why Create Your JVM Language?
</h2>

<p style="text-align: justify;">
  There are already an infinite number of programming languages. So why bother creating a new one? There are many possible answers to that.
</p>

<p style="text-align: justify;">
  First of all, there are many different kinds of languages: do you want to create a general purpose programming language (GPL) or a domain specific one? The first kind includes languages like <a href="http://www.toptal.com/java">Java</a> or Scala: languages intended to write decent enough solutions to a large set of problems. Domain Specific Languages (DSL) instead focus on solving very well a specific set of problems. Think of HTML or Latex: you could draw on the screen or generate documents in Java but it would be cumbersome, with these DSLs instead you can create documents very easily but they are limited to that specific domain.
</p>

<p style="text-align: justify;">
  So perhaps there is a set of problems on which you work very often and for which it could make sense to create a DSL. A language that would make you very productive while solving the same kinds of problems over and over.
</p>

<p style="text-align: justify;">
  Perhaps instead you want to create a GPL because you had some new ideas, for example to represent <a href="http://tomassetti.me/representing-relationships-as-first-class-citizens-in-an-object-oriented-programming-language/" target="_blank">relationships as first class citizens</a> or <a href="http://tomassetti.me/alternatives-to-global-variables-and-passing-the-same-value-over-a-long-chain-of-calls/" target="_blank">represent context</a>.
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91986/toptal-blog-image-1453386375924-09cd0d18553a682ee03ee2edf40c516a.jpg" alt="" />
</p>

<p style="text-align: justify;">
  Finally, you may want to create a new language because it is fun, cool, and because you are going to learn a lot in the process.
</p>

<p style="text-align: justify;">
  The fact is that if you target the JVM you can obtain a usable language with a reduced effort, that is because:
</p>

<ul style="text-align: justify;">
  <li>
    You just need to generate bytecode and your code will be available on all the platforms where there is a JVM
  </li>
  <li>
    You will be able to leverage all the libraries and frameworks existing for the JVM
  </li>
</ul>

<p style="text-align: justify;">
  So the cost of developing a language is greatly reduced on the JVM and it could make sense to create new languages in scenarios that would be uneconomical outside the JVM.
</p>

<h2 id="what-do-you-need-to-make-it-usable" style="text-align: justify;">
  What Do You Need to Make It Usable?
</h2>

<p style="text-align: justify;">
  There are some tools that you absolutely need to use your language &#8211; a parser and a compiler (or an interpreter) are among these tools. However, this is not enough. To make your language really usable in practice you need to provide many other components of the tool chain, possibly integrating with existing tools.
</p>

<p style="text-align: justify;">
  Ideally you want to be able to:
</p>

<ul style="text-align: justify;">
  <li>
    Manage references to code compiled for the JVM from other languages
  </li>
  <li>
    Edit source files in your favorite IDE with syntax highlight, error identification and auto-completion
  </li>
  <li>
    You want to be able to compile files using your favorite build system: maven, gradle or others
  </li>
  <li>
    You want to be able to write tests and run them as part of your Continuous-Integration solution
  </li>
</ul>

<p style="text-align: justify;">
  If you can do that, adopting your language will be much easier.
</p>

<p style="text-align: justify;">
  So how can we achieve that? In the rest of the post we examine the different pieces we need to make this possible.
</p>

<h2 id="parsing-and-compiling" style="text-align: justify;">
  Parsing and Compiling
</h2>

<p style="text-align: justify;">
  The first thing you need to do to transform your source files in a program is to parse them, obtaining an Abstract-Syntax-Tree (AST) representation of the information contained in the code. At that point you will need to validate the code: are there syntactical errors? Semantic errors? You need to find all of them and report them to the user. If everything goes smoothly you still need to resolve symbols. For example, does “List” refer to <em>java.util.List</em> or <em>java.awt.List</em>? When you invoke an overloaded method, which one are you invoking? Finally, you need to generate bytecode for your program.
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91987/toptal-blog-image-1453386432479-a1f1e632a9a0056c6cf27dfce0a1943b.jpg" alt="" />
</p>

<p style="text-align: justify;">
  So, from the source code to the compiled bytecode there are three main phases:
</p>

<ol style="text-align: justify;">
  <li>
    Building an AST
  </li>
  <li>
    Analyzing and transforming the AST
  </li>
  <li>
    Producing the bytecode from the AST
  </li>
</ol>

<p style="text-align: justify;">
  Let’s see those phases in details.
</p>

<p style="text-align: justify;">
  <em>Building an AST</em>: parsing is a sort of solved problem. There are many frameworks out there but I suggest you use ANTLR. It is well known, well maintained and it has some features which make it easier to specify grammars (it handles less recursive rules &#8211; you do not need to understand that but be thankful it does!).
</p>

<p style="text-align: justify;">
  <em>Analyzing and transforming the AST</em>: writing a type system, validation and symbol resolution could be challenging and require quite a lot of work. This topic alone would require a separate post. For now consider that this is the part of your compiler on which you are going to spend most of the effort.
</p>

<p style="text-align: justify;">
  <em>Producing the bytecode from the AST</em>: this last phase is actually not that difficult. You should have resolved symbols in the previous phase and prepared the terrain so that basically you can translate single nodes of your transformed AST to one or few bytecode instructions. Control structures could require some extra work because you are going to translate your for-loops, switches, ifs and so on in a sequence of conditional and unconditional jumps (yes, below your beautiful language there will still be a bunch of gotos). You need to learn how the JVM works internally, but the actual implementation is not that hard.
</p>

<h2 id="integration-with-other-languages" style="text-align: justify;">
  Integration with Other Languages
</h2>

<p style="text-align: justify;">
  When you will have obtained world domination for your language, all code will be written using it exclusively. However as an intermediate step your language will probably be used along other JVM languages. Perhaps someone will start writing a couple of classes or a small modules in your language inside a larger project. It is reasonable to expect to be able to mix several JVM languages. So, how does it affect your language tools?
</p>

<p style="text-align: justify;">
  You need to consider two different scenarios:
</p>

<ul style="text-align: justify;">
  <li>
    Your language and the others live in modules compiled separately
  </li>
  <li>
    Your language and the others live in the same modules and are compiled together
  </li>
</ul>

<p style="text-align: justify;">
  In the first scenario your code only needs to use compiled code written in other languages. For example some dependencies like Guava or modules in the same project can be compiled separately. This kind of integration requires two things: first, you should be able to interpret class files produced by other languages to resolve symbols to them and generate the bytecode for invoking those classes. The second point is specular to the first one: other modules may want to reuse the code written in your language after it has been compiled. Now, normally that is not a problem because Java can interact with most class files. However you could still manage to write class files which are valid for the JVM but cannot be invoked from Java (for example because you use identifiers which are not valid in Java).
</p>

<p style="text-align: justify;">
  The second scenario is more complicated: suppose you have a class A defined in Java code and a class B written in your language. Suppose the two classes refer to each other (for example A could extend B and B could accept A as a parameter for same method). Now the point is that the Java compiler cannot process the code in your language, so you have to provide it a class file for class B. However to compile class B you need to insert references to class A. So what you need to do is to have a sort of partial Java compiler, which given a Java source file is able to interpret it and produce a model of it which you can use to compile your class B. Note that this requires you to be able to parse Java code (using something like JavaParser) and solve symbols. If you have no idea where to start from take a look at <a href="https://github.com/ftomassetti/java-symbol-solver" target="_blank">java-symbol-solver</a>.
</p>

<h2 id="tools-gradle-maven-test-frameworks-ci" style="text-align: justify;">
  Tools: Gradle, Maven, Test Frameworks, CI
</h2>

<p style="text-align: justify;">
  The good news is that you can make the fact that they’re using a module written in your language totally transparent to the user by developing a plugin for gradle or maven. You can instruct the build system to compile files in your programming language. The user will keep running mvn compile or gradle assemble and not notice any difference.
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91988/toptal-blog-image-1453386461243-c11d60e6e6838a057334ff7a3fae661e.jpg" alt="" />
</p>

<p style="text-align: justify;">
  The bad news is that writing Maven plugins is not easy: the documentation is very poor, not intelligible and mostly outdated or simply <strong>wrong</strong>. Yes, it does not sound comforting. I have not yet written gradle plugins but it seems much easier.
</p>

<p style="text-align: justify;">
  Note that you should also consider how tests can be run using the build system. For supporting tests you should think of a very basic framework for <a href="http://www.toptal.com/blog/tags/unittesting">unit testing</a> and you should integrate it with the build system, so that running maven test looks for tests in your language, compile and run them reporting the output to the user.
</p>

<p style="text-align: justify;">
  My advice is to look at the examples available: one of them is the Maven plugin for the <a href="https://github.com/ftomassetti/turin-maven-plugin" target="_blank">Turin programming language</a>.
</p>

<p style="text-align: justify;">
  Once you have implemented it, everyone should be able to compile easily source files written in your language and use that in Continuous-Integration services like Travis.
</p>

<h2 id="ide-plugin" style="text-align: justify;">
  IDE Plugin
</h2>

<p style="text-align: justify;">
  A plugin for an IDE will be the most visible tool for your users and something that will affect greatly the perception of your language. A good plugin can help the user to learn the language by providing smart auto-completion, contextual errors and suggested refactorings.
</p>

<p style="text-align: justify;">
  Now, the most common strategy is to pick one IDE (typically Eclipse or IntelliJ IDEA) and develop a specific plugin for it. This is probably the most complex piece of your toolchain. This is the case for several reasons: first of all you can not reasonably reuse the work you will spend developing your plugin for one IDE for the others. Your Eclipse and your IntelliJ plugin are going to be totally separate. The second point is that IDE plugin development is something not very common, so there is not much documentation and the community is small. It means you will have to spend a lot of time figuring out things for yourself. I personally developed plugins for Eclipse and for IntelliJ IDEA. My questions on Eclipse forums remained unanswered for months or years. On the IntelliJ forums I had better luck, and sometimes I got an answer from the developers. However the user base of plugin developers is smaller and the API are very byzantine. Prepare to suffer.
</p>

<p style="text-align: justify;">
  There is an alternative to all of this, and it is to use <a href="https://www.eclipse.org/Xtext/" target="_blank">Xtext</a>. Xtext is a framework for developing plugins for Eclipse, IntelliJ IDEA and the web. It has been born on Eclipse and it has been just recently extended to support the other platforms, so there is not so much experience on that but it could be an alternative worthy to be considered. Let me get this straight: the only way to develop a very good plugin is to develop it using the native API of each IDE. However with Xtext you can have something reasonably decent with a fraction of the effort &#8211; you just give it to the syntax of your language and you get syntax errors/completion for free. Still, you have to implement symbol resolution and the hard parts, but this is a very interesting starting point; however, the hard bits are the integration with the platform specific libraries to solve Java symbols so this is not really going to solve all of your problems.
</p>

<h2 id="conclusions" style="text-align: justify;">
  Conclusions
</h2>

<p style="text-align: justify;">
  There are many ways you could lose potential users who showed an interest in your language. Adopting a new language is a challenge because it requires learning it and adapting our development habits. By reducing as much as possible the attrition and leverage the ecosystem already known to your users, you can keep users from giving up before they learn and fall in love with your language.
</p>

<p style="text-align: justify;">
  In the ideal scenario, your user could clone a simple project written in your language, and build it using the <a href="http://www.toptal.com/software/creating-jvm-languages-an-overview">standard tools</a> (Maven or Gradle) without noticing any difference. If he wants to edit the project he could open it in its favorite editor and the plugin will help point out to him errors and provide smart completions. This is a scenario much different than having to figure out how to invoke your compiler and edit files using notepad. The ecosystem around your language can really make the difference, and nowadays it can be built with a reasonable effort.
</p>

<p style="text-align: justify;">
  My advice is to be creative in your language, but not in your tools. Reduce the initial difficulties people have to face to adopt your language by using familiar standards.
</p>

<p style="text-align: justify;">
  Happy language designing!
</p>

<p style="text-align: justify;" class="note_normal">
  <span>Published at Codingpedia.org with the permission of Toptal – source </span><a title="Are you getting worked up over code duplication?" href="http://www.toptal.com/software/creating-jvm-languages-an-overview" target="_blank">Creating Usable JVM Languages: An Overview</a><span> from </span><a title="http://www.toptal.com/blog" href="http://www.toptal.com/blog" target="_blank">http://www.toptal.com/blog</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/toptal-logo.png" alt="Toptal-logo" /> 
  
  <p id="about_author_header">
    <strong>Toptal Blog</strong>
  </p>
  
  <div id="social_logos_up">
    <a class="icon-earth" href="http://www.toptal.com/blog" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/toptalllc" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/Toptal-141928212544793/" target="_blank"> </a> <a class="icon-gplus" href="https://plus.google.com/+Toptalllc/posts" target="_blank"> </a>
  </div>
  
  <div id="author_details" style="text-align: justify;">
    The Toptal Engineering Blog is a hub for in-depth development tutorials and new technology announcements created by professional freelance software engineers in the Toptal network.
  </div>
  
  <div class="clear">
  </div>
</div>
