## Getting Started
* Make sure you have a [GitHub account](https://github.com/signup/free)
* Fork the repository on GitHub

## Step 1: Set up a working copy on your computer
Firstly you need a local fork of the the [Codingpedia project](https://github.com/Codingpedia/codingpedia.github.io), so go ahead and press the "fork" button in GitHub. This will create a copy of the repository in your own GitHub account and you'll see a note that it's been forked underneath the project name:

Now you need a copy locally, so find the "SSH clone URL" in the right hand column and use that to clone locally using a terminal:
<pre><code class="bash">git clone https://github.com/silviamatei/codingpedia.github.io.git</pre></code>

Change now to your project's directory:
<pre><code class="bash">cd codingpedia.github.io</pre></code>

Finally, in this stage, you need to set up a new remote that points to the original project so that you can grab any changes and bring them into your local copy. Firstly clock on the link to the original repository – it's labeled "Forked from" at the top of the GitHub page. This takes you back to the projects main GitHub page, so you can find the "SSH clone URL" and use it to create the new remote, which we'll call upstream.

<pre><code class="bash">git remote add upstream https://github.com/Codingpedia/codingpedia.github.io.git</pre></code>

You now have two remotes for this project on disk:

1. origin which points to your GitHub fork of the project. You can read and write to this remote.
2. upstream which points to the main project's GitHub repository. You can only read from this remote.

## Step 2: Do some work
Contributing to the project will most likely be contributing a post to Codingpedia.org

But first things first  - Branch!

**The number one rule is to put each piece of work on its own branch.**

There are several steps here
1. add post in the posts directory - you need to conform to the jeykll standard
2. add your author information in the data/authors.yml file and a picture of you in the images/authors directory
3. configure the front-matter of your post
4. write the post; if it's not clear by now, it should be coding related,  see "How to insert url syntax"
5. if you want to republish a post of yours, or someone else's (of course with their consent), you might
want to add a backlink at the end of the post. (actually configure in front-matter) - cool stuff
