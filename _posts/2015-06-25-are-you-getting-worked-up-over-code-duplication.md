---
id: 2403
title: Are you getting worked up over code duplication?
date: 2015-06-25T20:50:05+00:00
author: Johannes Brodwall
layout: post
guid: http://www.codingpedia.org/?p=2403
permalink: /jhannes/are-you-getting-worked-up-over-code-duplication/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3878407580
fsb_social_facebook:
  - 2
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 3
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - code
  - development
tags:
  - code duplication
  - code reuse
---
<p style="text-align: justify;">
  As programmers, we have long learned that Duplication is the Ultimate Sin of programming. Even considering to duplicate something is almost unthinkable.
</p>

<p style="text-align: justify;">
  But removing duplication introduces dependencies. If you and I use the reuse the same piece of code instead of duplicating it, changes I make may affect you. This effect can anything from beneficial (I fixed a bug you also needed fixing) to benign (I added a new feature that you’re not using) to detrimental (I want it to work in a way that’s no good for you).
</p>

<p style="text-align: justify;">
  When we have dependencies, we have to think: “Perhaps I shouldn’t add that feature – what if breaks something for someone else?” “Damn the torpedoes, I’m hacking it in!” or “Perhaps I’ll just make a fork for my changes and we’ll merge later”.<!--more-->
</p>

<p style="text-align: justify;">
  Sometimes benign, sometimes harmfull.
</p>

<p style="text-align: justify;">
  I recently discussed with a friend the case of being innovative in the face of legacy code. Remember: Legacy code is code that you don’t want to touch, because it’s dangerous to change and it gives value to the business now.
</p>

<p style="text-align: justify;">
  We want to gradually build a new platform. So it seems like we have two choices: We could make the new code call functionality on the old platform (ick! because, you know, “<a href="http://en.wikipedia.org/wiki/Law_of_holes">stop digging</a>“) or we could build a new service and change the old system to use it (OMG! because, you know, “dangerous to change”).
</p>

<p style="text-align: justify;">
  If we allow the new system to duplicate as much functionality as needed from the old system, this false dicotomy goes away.
</p>

<p style="text-align: justify;">
  Duplication isn’t a cardinal sin. It’s a negative property, but in many cases, it could be your best option.
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with the permission of Johannes Brodwall – source <a title="Are you getting worked up over code duplication?" href="http://johannesbrodwall.com/2015/06/10/are-you-getting-worked-up-over-code-duplication/" target="_blank">Are you getting worked up over code duplication?</a> from <a title="http://johannesbrodwall.com/" href="http://johannesbrodwall.com/" target="_blank">http://johannesbrodwall.com/</a>
</p>

<p style="text-align: justify;">
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

<p style="text-align: center;">
  “Image courtesy of Victor Habbick/ FreeDigitalPhotos.net”
</p>
