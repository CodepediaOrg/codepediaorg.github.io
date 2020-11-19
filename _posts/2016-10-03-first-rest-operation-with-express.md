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
I had setup a started the backend project with Express Generator[^1] to support [www.bookmarks.dev](https://www.bookmarks.dev). In this one I will try to implement
a simple GET method that will deliver mocked bookmarks.

[^1]: <https://expressjs.com/en/starter/generator.html>

{% include source-code-bookmarks.dev.html %}

<!--more-->

Prerequisite for using Express is having NodeJS installed and running on your machine. I personally use Node Version Manager[^4],
 which is a simple bash script to manage multiple active node.js versions.
Now with NodeJS up and running install first the express-generator globally with the following command:

[^4]: <https://github.com/creationix/nvm>

```shell
$ npm install -g express-generator
```

or

```shell
$ npmig express-generator
```

> The `npmig` is just an alias for `npm install -g`. See my blog post [A developer's guide to using aliases]({% post_url 2016-08-02-a-developers-guide-to-using-aliases %})
> to understand what I am talking about and to learn how you can efficiently use aliases to boost your shell productivity.

The output should look something like the following:

```shell
/Users/ama/.nvm/versions/node/v10.15.0/bin/express -> /Users/ama/.nvm/versions/node/v10.15.0/lib/node_modules/express-generator/bin/express-cli.js
+ express-generator@4.16.1
added 10 packages from 13 contributors in 2.064s
```

Now I change to the parent directory, where I want to place my backend project and execute the following command

```shell
$ express bookmarks.dev/backend --git
```

with the following output:

```shell
  warning: the default view engine will not be jade in future releases
   warning: use `--view=jade' or `--help' for additional options


    create : bookmarks.dev/backend/
    create : bookmarks.dev/backend/public/
    create : bookmarks.dev/backend/public/javascripts/
    create : bookmarks.dev/backend/public/images/
    create : bookmarks.dev/backend/public/stylesheets/
    create : bookmarks.dev/backend/public/stylesheets/style.css
    create : bookmarks.dev/backend/routes/
    create : bookmarks.dev/backend/routes/index.js
    create : bookmarks.dev/backend/routes/users.js
    create : bookmarks.dev/backend/views/
    create : bookmarks.dev/backend/views/error.jade
    create : bookmarks.dev/backend/views/index.jade
    create : bookmarks.dev/backend/views/layout.jade
    create : bookmarks.dev/backend/.gitignore
    create : bookmarks.dev/backend/app.js
    create : bookmarks.dev/backend/package.json
    create : bookmarks.dev/backend/bin/
    create : bookmarks.dev/backend/bin/www

    change directory:
      $ cd bookmarks.dev/backend

    install dependencies:
      $ npm install

    run the app:
      $ DEBUG=backend:* npm start
```

> the `--git` parameter generates a .gitignore` file for the project. Learn more about other options by running `express -h` command:


```shell
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

```shell
$ cd bookmarks.dev/backend && npm install
```

and run the application

```shell
$ DEBUG=backend:* npm start
```

You should see now that the server is listening at port 3000:

```shell
 $ DEBUG=backend:* npm start
> backend@0.0.0 start /Users/ama/projects/dev/personal/testing/bookmarks.dev/backend
> node ./bin/www

  backend:server Listening on port 3000 +0ms
```

And when I type in the browser http://localhost:3000 I get a _Welcome To Express_ page - Yeeey, success!!!

Now add a `README.md` file and push it to GitHub before my computer breaks down :):

```shell
echo "# bookmarks-api.codepedia.org" >> README.md
git init
git add --all
git commit -m "first commit"
git remote add origin https://github.com/BookmarksDev/bookmarks.dev.codepedia.org.git
git push -u origin master
```

That was it for the initial step. Next thing is to import the project in IntelliJ and get some actual work done :)

## References
