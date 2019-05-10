---
layout: post
title: How to insert and highlight code in Jekyll blog post on Codepedia.org
description: "Demo post displaying the various ways of inserting and highlighting code in Markdown, when posting on Codepedia.org"
permalink: /ama/how-to-insert-and-highlight-code-in-jekyll-blog-post-on-codingpedia-org
author: ama
modified: 2014-12-23
tags: [sample post, code, highlighting]
categories: [intro]
---
This demo post displays the various ways of inserting and highlighting code in Markdown, when posting on Codepedia.org, because you know
code showing is the main point of Codepedia.org

Syntax highlighting is a feature that displays source code, in different colors and fonts according to the category of terms. This feature facilitates writing in a structured language such as a programming language or a markup language as both structures and syntax errors are visually distinct. Highlighting does not affect the meaning of the text itself; it is intended only for human readers.[^1]

[^1]: <http://en.wikipedia.org/wiki/Syntax_highlighting>

To embed code and highlight the syntax you have several possibilities:

1.  use highlight.js[^2], my preferred way, by simple placing the code inside <code>&lt;pre&gt;&lt;code&gt;</code>; it tries to detect
the language automatically. If automatic detection doesn’t work for you, you can specify the language in the class attribute:<code>&lt;pre&gt;&lt;code class=&quot;html&quot;&gt;...&lt;/code&gt;&lt;/pre&gt;</code>
2.  Jekyll has built in support for syntax highlighting of over 60 languages thanks to [Rouge][][^3], which is a pure-ruby syntax highlighter.
It can highlight 100 different languages, and output HTML or ANSI 256-color text.  Its HTML output is compatible with stylesheets designed for [pygments][].
3.  use code blocks, which are part of the Markdown spec, but syntax highlighting isn't. However, many renderers -- like Github's and Markdown Here -- support syntax highlighting. Which languages are supported and how those language names should be written will vary from renderer to renderer.

[^2]: <https://highlightjs.org/usage/>
[^3]: <http://rouge.jneen.net/>
[pygments]: http://pygments.org/ "Pygments"
[rouge]: http://rouge.jneen.net/

## How the blog post is structured?

Bellow I will list some code samples in various languages hightlighted with the different possibilities.


## CSS

### Highlight.js

#### Input

{% highlight liquid %}
{% raw %}
<pre><code class="css">#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
</code></pre>
{% endraw %}
{% endhighlight %}

#### Result

<pre><code class="css">#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
</code></pre>

### Jekyll liquid templates

#### Input

{% highlight liquid %}
{% raw %}
{% highlight css %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}
{% endraw %}
{% endhighlight %}

#### Result

{% highlight css %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}

There is a second argument to `highlight` called `linenos` that is optional.
Including the `linenos` argument will force the highlighted code to include line
numbers. For instance, the following code block would include line numbers next
to each line:

{% highlight css lineos %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}

### Markdown code block

#### Input

{% highlight liquid %}
{% raw %}
```css
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
```
{% endraw %}
{% endhighlight %}

#### Result

```css
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
```

## HTML

### Highlight.js

#### Input

{% highlight liquid %}
{% raw %}
<pre><code class="html">&lt;form action=&quot;action_page.php&quot;&gt;
  First name:&lt;br&gt;
  &lt;input type=&quot;text&quot; name=&quot;firstname&quot; value=&quot;Mickey&quot;&gt;&lt;br&gt;
  Last name:&lt;br&gt;
  &lt;input type=&quot;text&quot; name=&quot;lastname&quot; value=&quot;Mouse&quot;&gt;&lt;br&gt;&lt;br&gt;
  &lt;input type=&quot;submit&quot; value=&quot;Submit&quot;&gt;
&lt;/form&gt;</code></pre>
{% endraw %}
{% endhighlight %}

#### Result

<pre><code class="html">&lt;form action=&quot;action_page.php&quot;&gt;
  First name:&lt;br&gt;
  &lt;input type=&quot;text&quot; name=&quot;firstname&quot; value=&quot;Mickey&quot;&gt;&lt;br&gt;
  Last name:&lt;br&gt;
  &lt;input type=&quot;text&quot; name=&quot;lastname&quot; value=&quot;Mouse&quot;&gt;&lt;br&gt;&lt;br&gt;
  &lt;input type=&quot;submit&quot; value=&quot;Submit&quot;&gt;
&lt;/form&gt;</code></pre>

> As you can notice, one drawback of using highlight.js is that you need to escape the code snippets (especially html and xml), so that it doesn't get interpreted
by the browsers, but on the other hand the posts "feel" more like standard html directly in markdown; if you are using IntelliJ, like I am, you can use
the String Manipulation Plugin[^4], or FreeFormater[^5] is a great online tool also for code escaping

[^4]: <https://plugins.jetbrains.com/plugin/2162?pr=idea/>
[^5]: <http://www.freeformatter.com/string-escaper.html/>

### Jekyll liquid templates

#### Input
{% highlight liquid %}
{% raw %}
{% highlight html %}
<form action="action_page.php">
  First name:<br>
  <input type="text" name="firstname" value="Mickey"><br>
  Last name:<br>
  <input type="text" name="lastname" value="Mouse"><br><br>
  <input type="submit" value="Submit">
</form>
{% endhighlight %}
{% endraw %}
{% endhighlight %}

#### Result

{% highlight html %}
<form action="action_page.php">
  First name:<br>
  <input type="text" name="firstname" value="Mickey"><br>
  Last name:<br>
  <input type="text" name="lastname" value="Mouse"><br><br>
  <input type="submit" value="Submit">
</form>
{% endhighlight %}

## Java

### Highlight.js

#### Input

{% highlight liquid %}
{% raw %}
<pre><code class="java">/* HelloWorld.java
 */

public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
</code></pre>
{% endraw %}
{% endhighlight %}

#### Result

<pre><code class="java">/* HelloWorld.java
 */

public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
</code></pre>

### Jekyll liquid templates

#### Input

{% highlight liquid %}
{% raw %}
{% highlight java %}
public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
{% endhighlight %}
{% endraw %}
{% endhighlight %}

#### Result
{% highlight java %}
public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
{% endhighlight %}

#### Markdown
```java
public class HelloWorld
{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
```

## Javascript

### Highlight.js

#### Input

{% highlight liquid %}
{% raw %}
<pre><code class="javascript"> var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
</code></pre>
{% endraw %}
{% endhighlight %}

#### Result

<pre><code class="javascript"> var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
</code></pre>

### Jekyll liquid templates

#### Input

{% highlight liquid %}
{% raw %}
{% highlight javascript %}
 var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
{% endhighlight %}
{% endraw %}
{% endhighlight %}

#### Result

{% highlight javascript %}
 var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
{% endhighlight %}


### Markdown

#### Input

{% highlight liquid %}
{% raw %}
```javascript
 var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
```
{% endraw %}
{% endhighlight %}

#### Result

```javascript
 var hoisting = "global variable";
    (function(){
        confirm("\"" + hoisting + "\"" + " click OK" );
        var hoisting = "local variable";
        alert(hoisting);
    })(); //self-executing function
```

## Ruby

### Highlight.js

#### Input

{% highlight liquid %}
{% raw %}
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
{% endraw %}
{% endhighlight %}


#### Result

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

### Jekyll liquid templates

#### Input

{% highlight liquid %}
{% raw %}
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
{% endraw %}
{% endhighlight %}

#### Result

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

### Markdown

#### Input

{% highlight liquid %}
{% raw %}
```ruby
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
```
{% endraw %}
{% endhighlight %}

#### Result

```ruby
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
```

## Jekyll

> When you want to include Jekyll code that won't get interpret use the liquid templates

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

## Gist

Use the `gist` tag to easily embed a GitHub Gist onto your site. This works
with public or secret gists:

### Input

{% highlight liquid %}
{% raw %}
{% gist amacoder/ed963f13f536cc1935f6c238db28da24 %}
{% endraw %}
{% endhighlight %}

### Result

{% gist amacoder/ed963f13f536cc1935f6c238db28da24 %}

You may optionally specify a `filename` after the `gist_id`, if you want to embed only one file:

### Input
{% highlight liquid %}
{% raw %}
{% gist amacoder/ed963f13f536cc1935f6c238db28da24 Object.Prototype.js %}
{% endraw %}
{% endhighlight %}

### Result

{% gist amacoder/ed963f13f536cc1935f6c238db28da24 Object.Prototype.js %}
