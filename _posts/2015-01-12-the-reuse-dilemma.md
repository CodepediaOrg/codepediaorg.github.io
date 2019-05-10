---
id: 2180
title: The reuse dilemma
date: 2015-01-12T16:48:20+00:00
author: Johannes Brodwall
layout: post
guid: http://www.codingpedia.org/?p=2180
permalink: /jhannes/the-reuse-dilemma/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3377175406
fsb_social_facebook:
  - 0
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - development
tags:
  - code reuse
  - reuse
  - software development
---
<p style="color: #333333; text-align: justify;">
  The first commandment that any young programmer learns is “Thou Shall Not Duplicate”. Thus instructed, whenever we see something that looks like it may be repeated code, we refactor. We create libraries and frameworks. But removing duplication doesn’t come for free.<!--more-->
</p>

<p style="color: #333333; text-align: justify;">
  If I refactor some code so that instead of duplicating some logic in Class A and Class B, these classes share the logic in Class R (for reuse!). Now Classes A and B are indirectly coupled. This is not necessarily a bad thing, but it comes with some consequences, which are often overlooked.
</p>

<p style="color: #333333; text-align: justify;">
  If Class A require the shared functionality to change, we have a choice: We make the change or we stymie Class A. Making the change comes at a cost of breaking Class B. If these classes are in the same Java package (or .NET Namespace), chances are that we will be able to verify that the change didn’t break anything. If the reused functionality is in a library that is used by another library that is used by a Class B, verifying that the change was good is harder.
</p>

<p style="color: #333333; text-align: justify;">
  This is where Continuous Integration comes in. We check in our change to class R (for reuse, remember). A build is triggered on Jenkins, Bamboo, TFS or what have you. The build causes other projects to get built and eventually, we get a failing build where a unit test for Class B breaks. If we did our job right.
</p>

<p style="color: #333333; text-align: justify;">
  Even with this failing test, we’re not out of the woods. A build system for a large enterprise may take up to an hour or even more to run. This means that by trying to improve Class R, the developers of Class A have made a mess for the developers of Class B at least for a good while.
</p>

<p style="color: #333333; text-align: justify;">
  When organizations see repeated build breaks due to changes in dependencies, the reaction is usually the same: Lets limit changes to the shared code. Perhaps we’ll even version it, so that any change that you need will be released in the next version and dependencies can upgrade at their own pace.
</p>

<p style="color: #333333; text-align: justify;">
  Now we’ve introduced another problem: We are no longer able to change the code. Developers of Class A will have to take Class R as it is, or at the very least go through substantial work to change it. If the reused code is very mature, this is fine. After all, this is what we have to deal with when it comes to language standard libraries and open source projects.
</p>

<p style="color: #333333; text-align: justify;">
  Most reused code inside an organization, however, isn’t so mature. The developers of Class A will often be frustrated by the limitations of Class R. If the reused class is something very domain specific, the limitation is even worse.
</p>

<p style="color: #333333; text-align: justify;">
  So the developers do what any reasonable developer would do: They make a copy of Class R (or they fork the repository where it lives), creating a new duplication of the code.
</p>

<p style="color: #333333; text-align: justify;">
  And so it goes: Removing duplication leads to risk of adverse interactions between reusers. Organizations enforce change control on the reused code to avoid changes made in one context from breaking other parts of the system, resulting in paralysis for the reusers who need the reused code to change. The reusers eventually duplicate the reused code to their own branch where they can evolve it, leading to duplication. Until someone gets the urge to remove the duplication.
</p>

<p style="color: #333333; text-align: justify;">
  The dilemma happens in the small and in the large, but the trade-offs are different.
</p>

<p style="color: #333333; text-align: justify;">
  What we should do less is to reuse immature code with little novel value. The cost of the extra coupling often far outweighs the benefit of reuse.
</p>

<p class="note_normal" style="color: #333333; text-align: justify;">
  Published at Codingpedia.org with the permission of Johannes Brodwall – source <a title="http://johannesbrodwall.com/2014/10/10/the-reuse-dilemma/" href="The%20reuse dilemma" target="_blank">The reuse dilemma</a> from <a title="http://johannesbrodwall.com/" href="http://johannesbrodwall.com/" target="_blank">http://johannesbrodwall.com/</a>
</p>

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

<p style="color: #333333; text-align: center;">
  “Image courtesy of Stuart Miles/ FreeDigitalPhotos.net”
</p>
