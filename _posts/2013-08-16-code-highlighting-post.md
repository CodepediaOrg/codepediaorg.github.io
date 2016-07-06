---
layout: post
title: Syntax Highlighting Post
description: "Demo post displaying the various ways of highlighting code in Markdown."
author: ama
modified: 2014-12-23
tags: [sample post, code, highlighting]
categories: [intro]
---

Syntax highlighting is a feature that displays source code, in different colors and fonts according to the category of terms. This feature facilitates writing in a structured language such as a programming language or a markup language as both structures and syntax errors are visually distinct. Highlighting does not affect the meaning of the text itself; it is intended only for human readers.[^1]

[^1]: <http://en.wikipedia.org/wiki/Syntax_highlighting>

To highlight the syntax you have two possibilities:

1.  either use highlight.js[^2], my preferred way, by simple placing the code inside <code>&lt;pre&gt;&lt;code&gt;</code>; it tries to detect
the language automatically. If automatic detection doesn’t work for you, you can specify the language in the class attribute:<code>&lt;pre&gt;&lt;code class=&quot;html&quot;&gt;...&lt;/code&gt;&lt;/pre&gt;</code>
2.  or use [Rouge][][^3], which is a pure-ruby syntax highlighter.
It can highlight 100 different languages, and output HTML or ANSI 256-color text.  Its HTML output is compatible with stylesheets designed for [pygments][].

[^2]: <https://highlightjs.org/usage/>
[^3]: <http://rouge.jneen.net/>
[pygments]: http://pygments.org/ "Pygments"
[rouge]: http://rouge.jneen.net/

Bellow I will list some samples in different languages both hightlighted with both highglight.js[^2] and rouge[^3]

## CSS

### Highlight.js
<pre><code class="css">#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
</code></pre>

### Rouge code block
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

> One drawback of using highlight.js is that you need to escape the code snippets (especially html and xml), so that it doesn't get interpreted
by the browsers, but on the other hand the posts "feel" more like html standard; if you are using IntelliJ, like I am, you can use
the String Manipulation Plugin[^4], or FreeFormater[^5] is a great online tool also for code escaping

[^4]: <https://plugins.jetbrains.com/plugin/2162?pr=idea/>
[^5]: <http://www.freeformatter.com/string-escaper.html/>

### Rouge code block
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


## Java

### Highlight.js
<pre><code class="java">/* HelloWorld.java
 */

public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
</code></pre>

### Rouge code block
{% highlight java %}
public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
{% endhighlight %}

## Javascript

### Highlight.js
<pre><code class="javascript"> var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
</code></pre>

### Rouge code block
{% highlight javascript %}
 var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
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

### Rouge code block
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
