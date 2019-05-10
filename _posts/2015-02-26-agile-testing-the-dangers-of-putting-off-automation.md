---
id: 2299
title: Agile Testing – The Dangers of putting off Automation
date: 2015-02-26T17:16:03+00:00
author: Joe Colantonio
layout: post
guid: http://www.codingpedia.org/?p=2299
permalink: /joecolantonio/agile-testing-the-dangers-of-putting-off-automation/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 0
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3550579400
categories:
  - testing
tags:
  - agile
  - automation
  - testing
---
**More Agile Testing**

A few weeks ago on one of my [TestTalks](http://www.testtalks.com/) podcasts, I interviewed Janet Gregory, co-author of some of the most popular books on Agile Testing:

  * [Agile Testing; A Practical Guide for Testers and Agile Teams](http://www.amazon.com/gp/product/0321534468/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321534468&linkCode=as2&tag=joecol05-20&linkId=WM3VDYONNRD6DSLU)
  * [More Agile Testing: Learning Journeys for the whole team](http://www.amazon.com/gp/product/0321967054/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321967054&linkCode=as2&tag=joecol05-20&linkId=ARZ2OC23K3JQ7UYR)

Janet shared some of her tips and tricks on how teams can succeed with all their Agile testing efforts.

The three points from the interview that I want to focus on in this post are:

  * The Agile Mindset
  * The Agile Testing Quadrants
  * The Dangers of putting off automation

<!--more-->

[<img class="alignnone size-full wp-image-2147" src="http://www.joecolantonio.com/wp-content/uploads/2015/01/012215_2026_AgileTestin1.png" alt="012215_2026_AgileTestin1.png" width="234" height="241" />](http://www.joecolantonio.com/wp-content/uploads/2015/01/012215_2026_AgileTestin1.png)
  
**Agile Testing is a Mindset
  
** 

First it takes a certain mindset, with respect to how you approach testing, to get _everyone_ involved in testing. On an Agile team, everyone is now in it together; programmers test and testers should do whatever it takes to help their team create the best possible application.

So how do we make it so that Agile testing becomes part of our company’s culture? You need to make sure that the whole team responsible for delivering a feature understands how much testing is actually involved.

“Testing” not only refers to testing the software after it has been delivered to the tester. It’s also about testing ideas, testing assumptions, and getting prompt feedback all the way through.

To get your whole team thinking about testing, you have to raise awareness. You have to make testing visible. You have to start be able to talk about what testing actually means.

Having these conversations early on in the development process is one of the most powerful things your team can do to create an Agile testing mindset.

One way to aid in creating conversations and building an Agile testing mindset is to use the Agile Testing Quadrants.

**Agile Testing Quadrants
  
** 

In the book [_More Agile Testing_](http://www.amazon.com/gp/product/0321967054/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321967054&linkCode=as2&tag=joecol05-20&linkId=ARZ2OC23K3JQ7UYR),
  
_“the agile testing quadrants (the Quadrants) are based on a matrix Brian Marick developed in 2003 to describe types of testing used in Extreme Programming.”_

Basically, the authors take this quadrant model and make some small tweaks to reflect how it works within Agile.

The quadrant model is separated into four main areas:

  * Q1 – Technical tests that guide development
  * Q2 – Business tests that guide development
  * Q3 – Business tests that evaluate the product
  * Q4 – Technical tests that evaluate the product

[<img class="alignnone size-full wp-image-2148" src="http://www.joecolantonio.com/wp-content/uploads/2015/01/012215_2026_AgileTestin2.png" alt="012215_2026_AgileTestin2.png" width="502" height="350" />](http://www.joecolantonio.com/wp-content/uploads/2015/01/012215_2026_AgileTestin2.png)

These quadrants are a nice way to show Agile teams the types of testing that needs to be thought of during development. This helps makes things more visible and helps to start conversations around how much test coverage and efforts you are actually putting into testing a feature.

One of the things that people struggle with (and one of the reasons why_More Agile Testing_ was written) is how to include all these types of testing into their definitions of done and still have time to complete everything.

Janet recommends that we start thinking of things in layers:

  * Story Level
  * Feature Level
  * System Level

What can we test at the “story” level to get that story to done? If we start thinking at a higher level we have the idea of a feature, which has many stories. The “feature” level is what a business user really cares about. At the top is the “system” level. So, some of the tests in the quadrants belong to the story level while other tests belong at the feature level — not at every story level.

The teams are responsible to have the _conversations_ to get them to start thinking about how, where and at what level some of these quadrant- level concerns will be addressed.

Also, the sooner the teams are aware of the types of testing that may need to be performed, developers can start to think about the types of hooks that will need to be added into order to make their code testable/automatable.

For an in-depth discussion on these quadrants, check out Chapter Seven of [_More Agile Testing_](http://www.amazon.com/gp/product/0321967054/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321967054&linkCode=as2&tag=joecol05-20&linkId=ARZ2OC23K3JQ7UYR).

[<img class="alignnone size-full wp-image-2149" src="http://www.joecolantonio.com/wp-content/uploads/2015/01/012215_2026_AgileTestin3.png" alt="012215_2026_AgileTestin3.png" width="341" height="331" />](http://www.joecolantonio.com/wp-content/uploads/2015/01/012215_2026_AgileTestin3.png)

**The Dangers of putting off Automation
  
** 
  
I started my conversation with Janet around test automation by asking her how one knows what to automate and when it makes sense _not_ to automate something. She repeated a term that was used throughout our talk: “technical awareness”. One needs to be technically aware enough to be able to articulate the benefits and cost of test automation.

If we as testers can’t articulate these types of things, it makes it hard to go to our managers and tell them why something can or cannot be automated — But that doesn’t mean we should ignore automation altogether.

For some reason, some testers still resist test automation. While some of their concerns may be valid, Janet believes that an Agile team is sustainable _over time_ if you don’t have some type of automation. It might be just having a base of really good unit tests, but you have to have something, because you want that timely feedback.

In Agile development, everything changes so fast that unless we have the safety net of a regression test suite running every day, we really don’t know what we broke. That’s why it is so dangerous to put off automation.

The reality is that you cannot ignore test automation without it coming back to haunt you later on.

_More Agile Testing_ actually has a really vivid example of this in Matt Barcomb’s Legend of the Test Automation Volcano. I don’t want to spoil it for those of you who haven’t read it yet, but the crux of it is that eventually _everyone_ pays for putting off test automation!

**From More Agile Testing Awesomeness
  
** 

To hear my full conversation with Janet Gregory, check out [TestTalks Episode 34](http://www.testtalks.com/34). And be sure to add her two must-read books to your testing library:

  * [_Agile Testing: A Practical Guide for Testers and Agile Teams_](http://www.amazon.com/gp/product/0321534468/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321534468&linkCode=as2&tag=joecol05-20&linkId=WM3VDYONNRD6DSLU)_
  
_ 
  * [_More Agile Testing: Learning Journeys for the Whole Team_](%20Learning%20Journeys%20for%20the%20Whole%20Team)

<p class="note_normal" style="color: #333333;">
  Published at Codingpedia.org with the permission of Joe Colantonio – source <a title="http://www.joecolantonio.com/2015/01/20/agile-testing-the-dangers-of-putting-off-automation/" href="http://www.joecolantonio.com/2015/01/20/agile-testing-the-dangers-of-putting-off-automation/" target="_blank">Agile Testing – The Dangers of putting off Automation</a> from <a title="http://www.joecolantonio.com/" href="http://www.joecolantonio.com/" target="_blank">http://www.joecolantonio.com/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="http://joecolantonio.com/testtalks/wp-content/uploads/2014/12/TestTalksItunesCover.png" alt="Joe Colantonio" /> 
  
  <p id="about_author_header">
    Joe Colantonio
  </p>
  
  <div id="social_logos_up">
    <a class="icon-earth" href="http://www.joecolantonio.com/about/" target="_blank"> </a> <a class="icon-twitter" href="http://twitter.com/jcolantonio" target="_blank"> </a>
  </div>
  
  <div id="author_details" style="text-align: justify;">
    I find software test automation fun. In fact, I love it. But I also believe there is nothing worse than a boring technical blog. If I ever do get laid off, and any of you potential employers out there happen across my resume out there in the blogosphere, I’m keeping my fingers crossed that the good Karma I’ve built up by publishing such automation awesomeness will not go on unrewarded.
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div class="clear">
    </div>
  </div>
</div>
