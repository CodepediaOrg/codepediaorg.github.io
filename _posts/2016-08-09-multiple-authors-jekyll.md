---
layout: post
title: How to handle multiple authors in Jekyll
description: "In this post I will describe how I currently manage multiple authors in Jekyll"
author: ama
permalink: /ama/how-to-handle-multiple-authors-in-jekyll/
published: true
categories: [jekyll]
tags: [blogging, blog, jekyll, wordpress, migration, codingpedia]
---

One of my main concern when I considered the [migration from Wordpress to Jekyll](//www.codingpedia.org/ama/how-to-migrate-programming-blog-from-wordpress-to-jekyll/) was how would I be able to handle multiple authors, because this plays an important role in the website supporting our [Coding Friend Program](//www.codingpedia.org/friends/). Fear no more, I have found a satisfactory way to handle multiple authors with Jekyll[^1] and in this short post I list the main points concerning that.

[^1]: <https://jekyllrb.com/>

## Permalinks

In Wordpress, the author's username was present in the custom URL format -  __www.codingpedia.org/{author_username}/seo-friendly-url__ - this had to be kept. Good thing in Jekyll you can define custom permalinks[^2] per post. The Jekyll Exporter plugin[^1] did a great job and by exported the permalinks exactly as they were in Wordpress. Here a couple samples from front matters[^3] of different exported posts:

```
permalink: /ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/
permalink: /cwbuecheler/understanding-javascript-callbacks/
permalink: /udemy/git-tutorial-a-comprehensive-guide/
```
[^2]:<https://jekyllrb.com/docs/permalinks/>
[^3]:<https://jekyllrb.com/docs/frontmatter/>

> If I had to start a new multi author blog all over again, I might also consider something like the following http://www.codingpedia.org/authors/{author_username}/posts/seo-friendly-url (more REST like, but a thorough research would be on the table)... Anyway with Jekyll you have the possibility to choose whatever you like, by customizing the permalink in the front matter of each post.

- https://jekyllrb.com/docs/datafiles/
- https://mademistakes.com/articles/using-jekyll-2016/

## Author specific details in posts

Another nice feature I wanted to keep from Wordpress was the author's description (bio + social accounts) at the end of each post. Of course you can copy/paste html or markdown snippets stored somewhere the end of the post, but that's more of a hackle and if you need to change a detail about an author you need to make the change in every post of the author - for the migrated posts is now way around this, because I had the html migrated, but how to go from now on?

> In Wordpress I used a Global Content Blocks Plugin or something, and I had this issue that when editing a global content item in visual mode most of the content would disappear, so I had to be careful to only open them in text mode - hello ONLY code editing in Jekyll :)

Good thing Jekyll supports data files[^4], where you can specify your own custom data that can be accessed via the [Liquid templating system](https://github.com/shopify/liquid/wiki/liquid-for-designers). So I created a `authors.yml` YAML[^5] file in the `_data` folder as required by specification[^4]:

```
ama:
  name:          Adrian Matei
  site:           http://www.codingpedia.org
  avatar:         amacoder.png
  bio:            "Life force expressing itself as a coding capable human being"
  email:          adrianmatei@gmail.com
  # Twitter nick for use in Twitter cards and follow button.
  twitter: codingpedia # if no twitter in this config, the twitter follow button will be removed
  # GitHub nick for use in follow button in author block.
  github: amacoder
  social:
    - title: "github"
      url: "https://github.com/amacoder"
    - title: "linkedin"
      url: "https://www.linkedin.com/in/adrianmatei1983"
    - title: "youtube"
      url: "https://www.youtube.com/channel/UCZT1MatBdHWnVLl0OkcED3Q"
    - title: "facebook"
      url: "https://www.facebook.com/adrian.matei.90"
...........
dexter:
  name:          Dexter from Dexter's Laboratory
  site:           http://www.codingpedia.org
  avatar:         dexter.jpg
  bio:            "A boy-genius and inventor with a secret laboratory, who constantly battles his sister Dee Dee in an attempt to keep her out of the lab."
  email:          adrianmatei@gmail.com
  # Twitter nick for use in Twitter cards and follow button.
  twitter: codingpedia # if no twitter in this config, the twitter follow button will be removed
  # GitHub nick for use in follow button in author block.
  github: amacoder
  social:
    - title: "github"
      url: "https://github.com/amacoder"
    - title: "linkedin"
      url: "https://www.linkedin.com/in/adrianmatei1983"
    - title: "youtube"
      url: "https://www.youtube.com/channel/UCZT1MatBdHWnVLl0OkcED3Q"
    - title: "facebook"
      url: "https://www.facebook.com/adrian.matei.90"
...........      
```

Now the post's YAML front matter I use an `author:` entry, containing a key that matches one in the `authors.yml`, mentioned above. For example, newer posts from me have an entry like `author: ama`.

[^4]: <https://jekyllrb.com/docs/datafiles/>
[^5]: <http://yaml.org/>



### Feed

## What's missing
