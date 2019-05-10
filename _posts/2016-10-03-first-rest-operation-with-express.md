---
layout: post
title: First REST operation with Express
description: "In this one I will try to implement a simple GET method that will deliver mocked bookmarks."
author: ama
permalink: /ama/first-rest-operation-with-express
published: false
categories: [nodejs]
tags: [codingpedia bookmarks, beginner, nodejs, expressjs, tutorial]
---

In the previous blog post, [Kick starting a project with Express Generator]({% post_url 2016-10-01-kickstarting-a-project-with-express-generator}),
I had setup a started the backend project with Express Generator[^1] to support _www.codingmarks.org_. In this one I will try to implement
a simple GET method that will deliver mocked bookmarks.

[^1]: <https://expressjs.com/en/starter/generator.html>

<!--more-->

Prerequisite for using Express is having NodeJS installed and running on your machine. I personally use Node Version Manager[^4], which is a simple bash script to manage multiple active node.js versions
Now with NodeJS up and running install first the express-generator` globally with the following command:

[^4]: <https://github.com/creationix/nvm>

```bash
$ npm install -g express-generator
```

or

```bash
$ npmig express-generator
```

> The `npmig` is just an alias for `npm install -g`. See my blog post [A developer's guide to using aliases]({% post_url 2016-08-02-a-developers-guide-to-using-aliases %}) to understand what I am talking about and to learn
how you can efficiently use aliases to boost your shell productivity.

The output should will look something like the following:

```bash
/Users/ama/.nvm/versions/node/v6.4.0/bin/express -> /Users/ama/.nvm/versions/node/v6.4.0/lib/node_modules/express-generator/bin/express
/Users/ama/.nvm/versions/node/v6.4.0/lib
└─┬ express-generator@4.13.4
  ├─┬ commander@2.7.1
  │ └── graceful-readlink@1.0.1
  ├─┬ mkdirp@0.5.1
  │ └── minimist@0.0.8
  └── sorted-object@2.0.0
```

Now I changed to the parent directory where I want to place my backend project and execute the following command

```bash
$ express bookmarks-api.codingpedia.org --git
```

with the following output:

```bash
 create : bookmarks-api.codingpedia.org
 create : bookmarks-api.codingpedia.org/package.json
 create : bookmarks-api.codingpedia.org/app.js
 create : bookmarks-api.codingpedia.org/.gitignore
 create : bookmarks-api.codingpedia.org/public
 create : bookmarks-api.codingpedia.org/public/javascripts
 create : bookmarks-api.codingpedia.org/routes
 create : bookmarks-api.codingpedia.org/routes/index.js
 create : bookmarks-api.codingpedia.org/routes/users.js
 create : bookmarks-api.codingpedia.org/public/images
 create : bookmarks-api.codingpedia.org/views
 create : bookmarks-api.codingpedia.org/views/index.jade
 create : bookmarks-api.codingpedia.org/views/layout.jade
 create : bookmarks-api.codingpedia.org/views/error.jade
 create : bookmarks-api.codingpedia.org/public/stylesheets
 create : bookmarks-api.codingpedia.org/public/stylesheets/style.css
 create : bookmarks-api.codingpedia.org/bin
 create : bookmarks-api.codingpedia.org/bin/www

 install dependencies:
   $ cd bookmarks-api.codingpedia.org && npm install

 run the app:
   $ DEBUG=bookmarks-api.codingpedia.org:* npm start

```

> the `--git` parameter generates a .gitignore` file for the project. Learn more about other options by running `express -h` command:


```bash
$ express -h

  Usage: express [options] [dir]

  Options:

    -h, --help          output usage information
    -V, --version       output the version number
    -e, --ejs           add ejs engine support (defaults to jade)
        --hbs           add handlebars engine support
    -H, --hogan         add hogan.js engine support
    -c, --css <engine>  add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
        --git           add .gitignore
    -f, --force         force on non-empty directory
```

Now do as the man says - install the dependencies

```bash
 $ cd bookmarks-api.codingpedia.org && npm install
```

and run the application

```bash
 $ DEBUG=bookmarks-api.codingpedia.org:* npm start
```

You should see now that the server is listening at port 3000:

```bash
 $ DEBUG=bookmarks-api.codingpedia.org:* npm start
> bookmarks-api.codingpedia.org@0.0.0 start /Users/ama/projects/codingpedia-bookmarks/bookmarks-api.codingpedia.org
> node ./bin/www

  bookmarks-api.codingpedia.org:server Listening on port 3000 +0ms
```

And when I type in the browser http://localhost:3000 I get a _Welcome To Express_ page - Yeeey, success!!!

Now add a `README.md` file and push it to GitHub before my computer breaks down :):

```bash
echo "# bookmarks-api.codingpedia.org" >> README.md
git init
git add --all
git commit -m "first commit"
git remote add origin https://github.com/Codingpedia/bookmarks-api.codingpedia.org.git
git push -u origin master
```

That was it for the intial step. Next thing is to import the project in IntelliJ and get some actual work done :)

## References
