---
layout: post
title: Syntax Highlighting Post
description: "Demo post displaying the various ways of highlighting code in Markdown."
author: dexter
modified: 2014-12-23
tags: [sample post, code, highlighting]
categories: [intro]
---

Syntax highlighting is a feature that displays source code, in different colors and fonts according to the category of terms. This feature facilitates writing in a structured language such as a programming language or a markup language as both structures and syntax errors are visually distinct. Highlighting does not affect the meaning of the text itself; it is intended only for human readers.[^1]

[^1]: <http://en.wikipedia.org/wiki/Syntax_highlighting>

### Rouge Code Blocks

To modify styling and highlight colors edit `/_sass/_rouge.scss`.

## CSS
### Highlight.js
<pre><code class="css">#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
</code></pre>

### Jekyll
{% highlight css %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}

## HTML
### Highlight.js
<pre><code class="html">&lt;nav class=&quot;pagination&quot; role=&quot;navigation&quot;&gt;
    {% if page.previous %}
        &lt;a href=&quot;{{ site.url }}{{ page.previous.url }}&quot; class=&quot;btn&quot; title=&quot;{{ page.previous.title }}&quot;&gt;Previous article&lt;/a&gt;
    {% endif %}
    {% if page.next %}
        &lt;a href=&quot;{{ site.url }}{{ page.next.url }}&quot; class=&quot;btn&quot; title=&quot;{{ page.next.title }}&quot;&gt;Next article&lt;/a&gt;
    {% endif %}
&lt;/nav&gt;&lt;!-- /.pagination --&gt;
</code></pre>

### Jekyll
{% highlight html %}
{% raw %}
<nav class="pagination" role="navigation">
    {% if page.previous %}
        <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
    {% endif %}
    {% if page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
    {% endif %}
</nav><!-- /.pagination -->
{% endraw %}
{% endhighlight %}

## Ruby
### Highlight.js
<pre><code class="ruby">module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
</code></pre>

### Jekyll
{% highlight ruby %}
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
{% endhighlight %}

### Standard Code Block Without Escaping

    {% raw %}
    <nav class="pagination" role="navigation">
        {% if page.previous %}
            <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
        {% endif %}
        {% if page.next %}
            <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
        {% endif %}
    </nav><!-- /.pagination -->
    {% endraw %}
