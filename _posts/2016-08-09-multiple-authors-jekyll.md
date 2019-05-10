---
layout: post
title: How to handle multiple authors in Jekyll
description: "In this post I will describe how I currently manage multiple authors in Jekyll"
author: ama
permalink: /ama/how-to-handle-multiple-authors-in-jekyll/
published: true
categories: [jekyll]
tags: [jekyll, wordpress, blog, migration, codingpedia, blogging]
---

One of my main concern when I considered the [migration from Wordpress to Jekyll](//www.codingpedia.org/ama/how-to-migrate-programming-blog-from-wordpress-to-jekyll/) was how would I be able to handle multiple authors, because this plays an important role in the website supporting our [Coding Friend Program](//www.codingpedia.org/friends/). Fear no more, I have found a satisfactory way to handle multiple authors with Jekyll[^1] and in this short post I list the main points concerning that.

[^1]: <https://jekyllrb.com/>

<!--more-->

## Permalinks

In Wordpress, the author's username was present in the custom URL format -  __www.codingpedia.org/{author_username}/seo-friendly-url__ - this had to be kept. Good thing in Jekyll you can define custom permalinks[^2] per post. The Jekyll Exporter plugin did a great job and by exported the permalinks exactly as they were in Wordpress. Here a couple samples from front matters[^3] of different exported posts:

```
permalink: /ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/
permalink: /cwbuecheler/understanding-javascript-callbacks/
permalink: /udemy/git-tutorial-a-comprehensive-guide/
```
[^2]:<https://jekyllrb.com/docs/permalinks/>
[^3]:<https://jekyllrb.com/docs/frontmatter/>

> If I had to start a new multi author blog all over again, I might also consider something like the following http://www.codingpedia.org/authors/{author_username}/posts/seo-friendly-url (more REST like, but a thorough research would be on the table)... Anyway with Jekyll you have the possibility to choose whatever you like for each post, by customizing the permalink in its front matter.

## Author specific details in posts

Another nice feature I wanted to keep from Wordpress was the author's description (bio + social accounts) at the end of each post. Of course you can __copy/paste__ html or markdown snippets stored somewhere the end of the post, but that's more of a hackle and if you need to change a detail about an author you need to make the change in every post of the author - for the migrated posts is no way around this (because I had the html exported), but how to go from now on?

> In Wordpress I used a Global Content Blocks Plugin or something, and I had this issue that when editing a global content item in visual mode most of the content would disappear, so I had to be careful to only open them in text mode - hello ONLY code editing in Jekyll :)

Good thing Jekyll supports data files[^4], where you can specify your own custom data that can be accessed via the [Liquid templating system](https://github.com/shopify/liquid/wiki/liquid-for-designers). So I created an `authors.yml` YAML[^5] file in the `_data` folder as required by specification[^4]:

``` html
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
      url: "https://github.com/adrianmatei-me"
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
      url: "https://github.com/adrianmatei-me"
    - title: "linkedin"
      url: "https://www.linkedin.com/in/adrianmatei1983"
    - title: "youtube"
      url: "https://www.youtube.com/channel/UCZT1MatBdHWnVLl0OkcED3Q"
    - title: "facebook"
      url: "https://www.facebook.com/adrian.matei.90"
...........      
```

[^4]: <https://jekyllrb.com/docs/datafiles/>
[^5]: <http://yaml.org/>
[^6]: <https://mademistakes.com/articles/using-jekyll-2016/>

Now in the post's YAML front matter I use an `author:` entry, containing a key that matches one in the `authors.yml`, mentioned above. For example, newer posts from me have an entry like `author: ama`.

So to include the author related details at the end of a post I have the following section in `_layouts/post.html`:

{% highlight liquid %}
{% raw %}
....
{% assign author = site.data.authors[page.author] %}
{% if author %}
{% include author.html %}
{% endif %}
....
{% endraw %}
{% endhighlight %}

The `_includes/author.html` includes different author related attributes:

{% highlight liquid %}
{% raw %}
<div class="read-more">
  <div class="read-more-header">
    <a href="{{ author.site }}" class="read-more-btn">About the Author</a>
  </div><!-- /.read-more-header -->
  <div class="read-more-content author-info">
    <h3>{{author.name}}</h3>
    <div class="author-container">
      {% if author.avatar_url %}
        <img class="author-img" src="{{author.avatar_url}}" alt="{{author.name}}" />
      {% else %}
        <img class="author-img" src="{{site.url}}/{{author.avatar}}" alt="{{author.name}}" />
      {% endif %}
      <div class="author-bio">{{author.bio}}</div>
    </div>
    <div class="author-share">
      <ul class="list-inline social-buttons">
        {% for network in author.social %}
          <li><a href="{{ network.url }}" target="_blank"><i class="fa fa-{{ network.title }} fa-fw"></i></a></li>
        {% endfor %}
      </ul>
      {% if author.github %}
        <a aria-label="Follow @{{author.github}} on GitHub" data-style="mega" href="https://github.com/{{author.github}}" class="github-button">Follow @{{author.github}}</a>
      {% endif %}
      <br>
      {% if author.twitter %}
        <a href="https://twitter.com/{{author.twitter}}" class="twitter-follow-button" data-show-count="false" data-size="large">Follow @{{author.twitter}}</a>
      {% endif %}
    </div>
  </div>
  {% endraw %}
  {% endhighlight %}

You can see the result at the end of this post in the **About author** section.

### Feed

The same functionality is valid when generating the entries in the atom [feed.xml]({{ site.owner.name }}/feed.xml) :

{% highlight liquid %}
{% raw %}
{% for post in site.posts limit:20 %}
<entry>
  <title type="html"><![CDATA[{{ post.title | cdata_escape }}]]></title>
 <link rel="alternate" type="text/html" href="{% if post.link %}{{ post.link }}{% else %}{{ site.url }}{{ post.url }}{% endif %}" />
  <id>{{ site.url }}{{ post.id }}</id>
  {% if post.modified %}<updated>{{ post.modified | to_xmlschema }}T00:00:00-00:00</updated>
  <published>{{ post.date | date_to_xmlschema }}</published>
  {% else %}<published>{{ post.date | date_to_xmlschema }}</published>
  <updated>{{ post.date | date_to_xmlschema }}</updated>{% endif %}
  {% assign author = site.data.authors[post.author] %}
  {% if author %}
  <author>
    <name>{{ author.name }}</name>
    <uri>{{ author.site }}</uri>
    {% if author.email %}
    <email>{{ author.email }}</email>
    {% endif %}
  </author>
  {% else %}
  <author>
    <name>{{ post.author }}</name>
    <uri>{{ site.url }}</uri>
    <email>{{ site.owner.email }}</email>
  </author>
  {% endif %}
  <content type="html">
    {{ post.content | xml_escape }}
    {% include feed-footer.html %}
  </content>
</entry>
{% endfor %}
{% endraw %}
{% endhighlight %}

## What's missing

In Wordpress if I clicked on __http://www.codingpedia.org/authors/{author_username}__ I would get all the posts of the name author with pagination if they were more than five, I guess it's a nice feature and once I will implement that I will post it here.

I hope you've found this useful and if you see any improvement possibilities to this approach please leave a comment below, or even better make a pull request to improve the [source code](https://github.com/Codingpedia/codingpedia.github.io) directly - thanks.

## References
