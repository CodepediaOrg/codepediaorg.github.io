---
id: 2020
title: The madness of layered architecture
date: 2014-10-23T21:20:04+00:00
author: Johannes Brodwall
layout: post
guid: http://www.codingpedia.org/?p=2020
permalink: /jhannes/the-madness-of-layered-architecture-2/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3148060378
fsb_social_facebook:
  - 2
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 5
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - architecture
  - 'C#'
  - java
tags:
  - application architecture
  - layer
  - layered architecture
---
<p style="color: #333333; text-align: justify;">
  I once visited a team that had fifteen layers in their code. That is: If you wanted to display some data in the database in a web page, that data passed through 15 classes in the application. What did these layers do? Oh, nothing much. They just copied data from one object to the next. Or sometimes the “access object layer” would perform a check that objects were valid. Or perhaps the check would be done in the “boundary object layer”. It varied, depending on which part of the application you looked.<!--more-->
</p>

<p style="color: #333333; text-align: justify;">
  Puzzled (and somewhat annoyed), I asked the team why they had constructed their application this way. The answer was simple enough: They had been told so by the expensive consultant who had been hired to advice on the organization’s architecture.
</p>

<p style="color: #333333;">
  I asked the team what rationale the consultant had given. They just shrugged. Who knows?
</p>

<p style="color: #333333; text-align: justify;">
  Today, I often visit teams who have three to five layers in their code. When asked why, the response is usually the same: This is the advice they have been given. From a book, a video or a conference talk. And the rationale remains elusive or muddled at best.
</p>

<h2 style="color: #000000;">
  Why do we construct layered applications?
</h2>

<p style="color: #333333; text-align: justify;">
  There’s an ancient saying in the field of computing: Any problem in computer science can be solved by adding a layer of indirection.
</p>

<p style="color: #333333; text-align: justify;">
  Famously, this is the guiding principle behind our modern network stack. In web services SOAP performs method calls on top of HTTP. HTTP sends requests and receives responses on top of TCP. TCP streams data in two directions on top of IP. IP routes packets of bits through a network on top of physical protocols like Ethernet. Ethernet broadcasts packets of bits with a destination address to all computers on a bus.
</p>

<p style="color: #333333; text-align: justify;">
  Each layer performs a function that lets the higher layer abstract away the complexities of for example resending lost packets or routing packets through a globally interconnected network.
</p>

<p style="color: #333333;">
  The analogy is used to argue for layers in enterprise application architecture.
</p>

<p style="color: #333333;">
  But enterprise applications are not like network protocols. <em>Every layer in most enterprise application operates at the same level of abstraction</em>.
</p>

<p style="color: #333333; text-align: justify;">
  To pick on a popular example: John Papa’s video on Single Page Applications uses the following layers on the server side (and a separate set on the client side): <code>Controllers</code>, <code>UnitOfWork</code>, <code>Repository</code>, <code>Factories</code> and <code>EntityFramework</code>. So for example the <code>AttendanceRepository</code> property in <code>CodeCamperUnitOfWork</code> returns a <code>AttendanceRepository</code> to the <code>AttendanceController</code>, which calls <code>GetBySessionId()</code> method in <code>AttendanceRepository</code> layer, which finally calls <code>DbSet.Where(ps =&gt; ps.SessionId == sessionId)</code> on <code>EntityFramework</code>. And then there’s the RepositoryFactories layers. Whee!
</p>

<p style="color: #333333;">
  And what does it all do? It filters an entity based on a parameter. Wat?!
</p>

<p style="color: #333333; text-align: justify;">
  (A hint that this is going off the rails is that discussion in the video presentation starts with the bottom and builds up to the controllers instead of outside in)
</p>

<p style="color: #333333; text-align: justify;">
  In a similar Java application, I have seen – and feel free to skip these tedious details – the <code>SpeakersController.findByConference</code> calls <code>SpeakersService.findByConference</code>, which calls <code>SpeakersManager.findByConference</code>, which calls <code>SpeakersRepository.findByConference</code>, which constructs a horrific JPAQL query which nobody can understand. JPA returns an <code>@Entity</code> which is mapped to the database, and the <code>Repository</code>, or perhaps the <code>Manager</code>, <code>Service</code> or <code>Controller</code>, or perhaps two or three of these, will transform from Speaker-class to another.
</p>

<h2 style="color: #000000;">
  Why is this a problem?
</h2>

<p style="color: #333333; text-align: justify;">
  The cost of code: A reasonable conjecture would be that the cost of developing and maintaining an application grows with the size of the application. Adding code without value is waste.
</p>

<p style="color: #333333; text-align: justify;">
  Single responsibility principle: In the above example, the <code>SpeakerService</code> will often contain all functionality associated with speakers. So if adding a speaker requires you to select a conference from a drop-down list, the SpeakerService will often have a findAllConferences method, so that <code>SpeakersController</code> doesn’t need to also have a dependency on <code>ConferenceService</code>. However, this makes the classes into functionality magnets. The symptom is low coherence: the methods of one class can be divided into distinct sets that are never used at the same time.
</p>

<p style="color: #333333; text-align: justify;">
  Dumb services: “Service” is a horrible name for a class – a service is a more or less coherent collection of functions. A more meaningful name would be a “repository” for a service that stores and retrieves objects, a <code>Query</code> is a service that selects objects based on a criteria (actually it’s a command, not a service), a <code>Gateway</code> is a service that communicates with another system, a <code>ReportGenerator</code> is a service that creates a report. Of course, the fact that a controller may have references to a repository, a report generator and a gateway should be quite normal if the controller fetches data from the database to generate a report for another system.
</p>

<p style="color: #333333; text-align: justify;">
  Multiple points of extension: If you have a controller that calls a service that calls a manager that calls a repository and you want to add some validation that the object you are saving is consistent, where would you add it? How much would you be willing to bet that the other developers on the team would give the same answer? How much would you be willing to bet that <em>you</em> would give the same answer in a few months?
</p>

<p style="color: #333333; text-align: justify;">
  Catering to the least common denominator: In the conference application we have been playing with, <code>DaysController</code> creates and returns the days available for a conference. The functionality needed for <code>DaysController</code> is dead simple. On the other hand <code>TalksController</code> has a lot more functionality. Even though these controllers have vastly different needs, they both get the same (boring) set of classes: A <code>Controller</code>, a <code>UnitOfWork</code>, a <code>Repository</code>. There is no reason the <code>DaysController</code> couldn’t use <code>EntityFramework</code> directly, other than the desire for consistency.
</p>

<p style="color: #333333; text-align: justify;">
  Most applications have a few functional verticals that contain the meat of the application and a lot of small supporting verticals. Treating them the same only creates more work and more maintenance effort.
</p>

<h2 style="color: #000000;">
  So how can you fix it?
</h2>

<p style="color: #333333; text-align: justify;">
  The first thing you must do is to build your application from the outside in. If your job is to return a set of objects, with .NET <code>EntityFramework</code> you can access the <code>DbSet</code> directly – just inject <code>IDbSet</code> in your controller. With Java JPA, you probably want a <code>Repository</code> with a finder method to hide the JPQL madness. No service, manager, worker, or whatever is needed.
</p>

<p style="color: #333333; text-align: justify;">
  The second thing you must do is to grow your architecture. When you realize that there’s more responsibilities in your controller than deciding what to do with a user request, you must extract new classes. You may for example need a <code>PdfScheduleGenerator</code> to create a printable schedule for your conference. If you’re using .NET entity framework, you many want to create some LINQ extension methods on e.g. <code>IEnumerable</code> (which is extended by <code>IDbSet</code>)
</p>

<p style="color: #333333; text-align: justify;">
  The third and most important thing you must do is to give your classes names that reflect their responsibilities. A service should not just be a place to dump a lot of methods.
</p>

<p style="color: #333333; text-align: justify;">
  Every problem in computer science can be solved by adding a layer of indirection, but most problems in software engineering can be solved by removing a misplaced layer.
</p>

<p style="color: #333333;">
  Let’s build leaner applications!
</p>

<p class="note_normal" style="color: #333333; text-align: justify;">
  Published at Codingpedia.org with the permission of Johannes Brodwall – source <a title="The madness of layered architecture" href="http://johannesbrodwall.com/2014/07/10/the-madness-of-layered-architecture/" target="_blank">The madness of layered architecture</a> from <a title="http://johannesbrodwall.com/" href="http://johannesbrodwall.com/" target="_blank">http://johannesbrodwall.com/</a>
</p>

<p style="color: #333333;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/johannes-brodwall.jpeg" alt="Johannes Brodwall" /> 
    
    <p id="about_author_header">
      <strong>Johannes Brodwall</strong>
    </p>
    
    <div id="author_details" style="text-align: justify;">
      Johannes is the director of software development for the MRM product company BrandMaster. In his spare time he likes to coach teams and developers on better coding, collaboration, planning and product understanding.
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://johannesbrodwall.com/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/jhannes" target="_blank"> </a> <a class="icon-github" href="https://github.com/jhannes" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>
