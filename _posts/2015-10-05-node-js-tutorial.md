---
id: 2472
title: Node.js Tutorial
date: 2015-10-05T06:56:45+00:00
author: Udemy tutorials
layout: post
guid: http://www.codingpedia.org/?p=2472
permalink: /udemy/node-js-tutorial/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4194770021
fsb_social_facebook:
  - 5
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 10
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - angular
  - angularJS
  - backend
  - beginner
  - expressjs
  - javascript
  - mean stack
  - mongoDB
  - monk
  - node
  - nodejs
  - nodemon
  - sublimetext
  - tutorial
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Nodejs_Tutorial_A_Beginners_Guide">Node.js Tutorial: A Beginner’s Guide</a>
    </li>
    <li>
      <a href="#So_What_Is_Node">So, What Is Node?</a>
    </li>
    <li>
      <a href="#The_Job_Market">The Job Market</a>
    </li>
    <li>
      <a href="#I_Love_Node_Because">I Love Node Because…</a>
    </li>
    <li>
      <a href="#When_to_Use_Node">When to Use Node?</a>
    </li>
    <li>
      <a href="#The_MEAN_Stack">The MEAN Stack</a>
    </li>
    <li>
      <a href="#Setting_Up_the_Environment">Setting Up the Environment</a><ul>
        <li>
          <a href="#Install_Sublime_Text">Install Sublime Text</a>
        </li>
        <li>
          <a href="#Install_Mongo">Install Mongo</a>
        </li>
        <li>
          <a href="#Install_Node">Install Node</a>
        </li>
        <li>
          <a href="#Install_Express_Generator">Install Express Generator</a>
        </li>
        <li>
          <a href="#Scaffold_a_Project">Scaffold a Project</a>
        </li>
        <li>
          <a href="#Install_Dependencies">Install Dependencies</a>
        </li>
        <li>
          <a href="#Install_Nodemon">Install Nodemon</a>
        </li>
        <li>
          <a href="#Install_Monk">Install Monk</a>
        </li>
        <li>
          <a href="#Run_the_Application">Run the Application</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Displaying_All_Videos_in_the_Database">Displaying All Videos in the Database</a><ul>
        <li>
          <a href="#Step_1_Populating_the_Database">Step 1: Populating the Database</a>
        </li>
        <li>
          <a href="#Step_2_Creating_an_API_with_Express">Step 2: Creating an API with Express</a>
        </li>
        <li>
          <a href="#Step_3_Adding_Angular">Step 3: Adding Angular</a>
        </li>
        <li>
          <a href="#Step_4_Rebuilding_the_Home_Page_using_Angular">Step 4: Rebuilding the Home Page using Angular</a>
        </li>
        <li>
          <a href="#Step_5_Implementing_the_Controller">Step 5: Implementing the Controller</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Adding_a_New_Video">Adding a New Video</a><ul>
        <li>
          <a href="#Step_1_Creating_an_API_Endpoint">Step 1: Creating an API Endpoint</a>
        </li>
        <li>
          <a href="#Step_2_Creating_a_Form">Step 2: Creating a Form</a>
        </li>
        <li>
          <a href="#Step_3_Adding_Bootstrap">Step 3: Adding Bootstrap</a>
        </li>
        <li>
          <a href="#Step_4_Implementing_the_Controller">Step 4: Implementing the Controller</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Editing_a_Video">Editing a Video</a><ul>
        <li>
          <a href="#Step_1_Creating_the_API_Endpoints">Step 1: Creating the API Endpoints</a>
        </li>
        <li>
          <a href="#Step_2_Creating_the_Edit_Page">Step 2: Creating the Edit Page</a>
        </li>
        <li>
          <a href="#Step_3_Implementing_the_Controller">Step 3: Implementing the Controller</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Deleting_a_Video">Deleting a Video</a><ul>
        <li>
          <a href="#Step_1_Building_the_API_Endpoint">Step 1: Building the API Endpoint</a>
        </li>
        <li>
          <a href="#Step_2_Building_the_Delete_Page">Step 2: Building the Delete Page</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Wrapping_Up">Wrapping Up</a>
    </li>
  </ul>
</div>

## <span id="Nodejs_Tutorial_A_Beginners_Guide"><strong>Node.js Tutorial: A Beginner’s Guide</strong></span>

<p style="text-align: justify;">
  In this tutorial, I’ll teach you the basics of Node.js. Not only will you learn what Node is and what you can do with it, but you’ll see Node in action. We’ll build a simple application for a video rental store using Node, Express, Angular and MongoDB.
</p>

<p style="text-align: justify;">
  At a minimum, I’m assuming you have some web development experience. So you should know a bit of Javascript, HTML, and CSS, and have some familiarity with a web application framework such as ASP.NET MVC, PHP, Python or Rails. As far as Node, Express, Angular and MongoDB are concerned, I’m assuming you’re a beginner and that’s why you’re reading this tutorial. So, I’ll cover all these technologies from the ground up. If you do happen to have some experience with Angular and MongoDB, you can read this tutorial faster.<!--more-->
</p>

## <span id="So_What_Is_Node">So, What Is Node?</span>

<p style="text-align: justify;">
  Node is an open-source, cross-platform runtime environment for executing Javascript code. It’s built on the Google V8 Javascript engine, which is the execution engine for Google Chrome and is extremely fast. V8 compiles Javascript code to native machine code.
</p>

<p style="text-align: justify;">
  Before Node, Javascript was only executed in browsers. In 2009, Ryan Dahl used the open-source Google V8 Javascript engine to build Node as a runtime environment for Javascript outside a browser. This made it possible for Javascript developers to use Javascript on the server, mostly in building real-time Web APIs.
</p>

## <span id="The_Job_Market">The Job Market</span>

<p style="text-align: justify;">
  Node is gaining a lot of popularity these days and many big companies (such as IBM, Microsoft, Yahoo, LinkedIn, PayPal) have started adopting it.
</p>

<p style="text-align: justify;">
  As you can see from the following job trends on Indeed.com, demand for Node.js developers is increasing very fast.
</p>

[<img class="aligncenter size-full wp-image-147321" src="https://blog.udemy.com/wp-content/uploads/2015/09/image092.png" alt="image09" width="540" height="300" />](https://blog.udemy.com/wp-content/uploads/2015/09/image092.png)

<p style="text-align: justify;">
  Note that in terms of volume, Node developers are not as in demand as Ruby on Rails and many other frameworks, but I think that will change soon.
</p>

<p style="text-align: justify;">
  In 2013,<span class="Apple-converted-space"> </span><a href="https://www.paypal-engineering.com/2013/11/22/node-js-at-paypal/">as explained on their blog</a>, PayPal moved from Java to Node.js. LinkedIn also<span class="Apple-converted-space"> </span><a href="http://highscalability.com/blog/2012/10/4/linkedin-moved-from-rails-to-node-27-servers-cut-and-up-to-2.html">moved from Rails to Node</a>.
</p>

<p style="text-align: justify;">
  I personally think the future of web will be Node. These days we’re getting more and more connected and real-time systems and less need for the users to refresh their browsers and wait for seconds when switching from one page to another. With support from the open-source community, Node is and will be the preferred technology for building fast, connected and real-time applications.
</p>

## <span id="I_Love_Node_Because">I Love Node Because…</span>

<p style="text-align: justify;">
  I’m one of the early adopters of ASP.NET MVC and been developing web applications using this framework for several years. One thing that has always put me off about ASP.NET MVC is the radical shift of the language, programming style and conventions from the server-side to the client-side code. While C# and Javascript are both C-like languages, programming with C# is fundamentally different from Javascript. That’s why a lot of ASP.NET web developers are often more skilled at one of these two languages but not both, and so they classify themselves as more of a “back-end” or “front-end” developer. The same applies to Ruby on Rails, PHP, Python, etc.
</p>

<p style="text-align: justify;">
  With Node, however, you use Javascript everywhere, on the server and on the client. This means cleaner and more consistent code base and fewer conversions and mappings. Also, one good Javascript developer can write both the server-side and the client-side code equally well.
</p>

## <span id="When_to_Use_Node">When to Use Node?</span>

<p style="text-align: justify;">
  Node is not a silver bullet. It’s designed for I/O-intensive operations and shines in building fast, scalable and real-time network applications. Examples of these applications are online games, collaboration tools, chat systems, etc. With Node, you can serve a great many clients with minimal system resources, and that’s why it’s designed for high scalability.
</p>

<p style="text-align: justify;">
  Node is also an excellent choice for building APIs on top of a document database like MongoDB. You store your documents as JSON objects in Mongo and expose them via a RESTful API. No need to convert between JSON and other types when reading or writing from your database. In this tutorial, we’ll build a Node application with this style of architecture.
</p>

<p style="text-align: justify;">
  Node, due to its architecture, should not be used for CPU-intensive operations. For this tutorial, since my focus is to show Node in action, I won’t get into architecture of Node and why it shouldn’t be used for CPU-intensive applications. That’s something that I’ll cover in my comprehensive Node course that I’ll publish on Udemy soon. If you’re interested, be sure to subscribe to my mailing list (details are at the end of this tutorial).
</p>

## <span id="The_MEAN_Stack">The MEAN Stack</span>

<p style="text-align: justify;">
  There are many choices for building Node applications, but the MEAN stack has increased in popularity lately. MEAN stands for:
</p>

  * **MongoDB**: the database engine we use to store the data.
  * **Express.js**: the server-side framework for building web applications, similar to ASP.NET MVC or Rails.
  * **Angular**: the front-end framework for building web applications
  * **Node.js**: the Javascript runtime environment

<p style="text-align: justify;">
  With MEAN, you store your documents in a JSON-like format in MongoDB and expose them via a RESTful API that you build with Express and Node. You build a client with Angular to consume this API and render views to the user. This means that you use a single, unified language (Javascript) throughout your code, which results in more consistent and maintainable code. Plus, you spend less time dealing with conversions from JSON to other types. All of this results in better performance and increased developer productivity.
</p>

<p style="text-align: justify;">
  If you’ve never worked with MongoDB or Angular, don’t worry at all. I’ll teach you the basics of these technologies as part of this tutorial.
</p>

Okay, enough theory. I believe it’s time to get our hands dirty in the code and build an application using the MEAN stack.

## <span id="Setting_Up_the_Environment">Setting Up the Environment</span>

<p style="text-align: justify;">
  Before we get started, there are a few tools you need to install in order to develop MEAN applications. Installing all these tools takes 15 – 20 minutes. Once you do it, you’ll have a proper environment for developing MEAN applications and you never have to go through these steps ever again.
</p>

<p style="text-align: justify;">
  So go ahead and install each tool in the order I’ve listed here. If you already have some of these tools (e.g., Sublime Text, MongoDb), you can skip that section.
</p>

### <span id="Install_Sublime_Text">Install Sublime Text</span>

<p style="text-align: justify;">
  Sublime Text is a very lightweight and sophisticated code editor. You can use any editor to build node applications, but if you haven’t used Sublime, I recommend you to give it a try. To get Sublime, go to: <a href="http://sublimetext.com/">http://sublimetext.com</a>
</p>

### <span id="Install_Mongo">Install Mongo</span>

<p style="text-align: justify;">
  Go to<span class="Apple-converted-space"> </span><a href="http://mongodb.org/">http://mongodb.org</a><span class="Apple-converted-space"> </span>and click<span class="Apple-converted-space"> </span><b>Download MongoDB</b>. Follow their documentation for installing Mongo. It’s very easy and unlike SQL Server, which takes half an hour or so, installing Mongo takes only a minute or two.
</p>

<p style="text-align: justify;">
  Once you install Mongo, there a few simple steps you need to follow to run it. You need to create a directory where Mongo will store database files. This directory should have write permissions for the current user. Then, you need to run MongoD (Mongo Daemon), which is a background process that handles data requests.
</p>

<p style="text-align: justify;">
  By default, MongoD expects the data directory to be<span class="Apple-converted-space"> </span><b>/data/db<span class="Apple-converted-space"> </span></b>in the root of your drive. I’d suggest that you keep the defaults for the purpose of this tutorial. But if you really have to change it, check out their complete instructions on their website on the download page.
</p>

<p style="text-align: justify;">
  Let’s stick to the defaults and run Mongo:
</p>

**For Windows users:**

Open Command Prompt as an administrator:

<pre class="lang:sh decode:true">&gt; md \data\db
&gt; cd “C:\Program Files\MongoDB\Server\3.0\bin” (or wherever you installed MongoDB)
&gt; mongod</pre>

<p style="text-align: justify;">
  Note that you can run MongoD as a Windows Service, so you don’t have to run it from the Command Prompt every time. The details are in the instructions on MongoDB website.
</p>

<p style="text-align: justify;">
  You may get a pop-up about MongoD listening for network connections. Give access to MongoD.
</p>

**For Mac users:**

Open Terminal:

<pre class="lang:sh decode:true">$ sudo mkdir -p /data/dbmd 
$ whoami
moshfeghhamedani
$ sudo chown moshfeghhamedani /data/db
$ mongod</pre>

You should see MongoD running in the Command Prompt or Terminal and waiting for connections.

[<img class="aligncenter size-full wp-image-147323" src="https://blog.udemy.com/wp-content/uploads/2015/09/image073.png" alt="image07" width="922" height="624" />](https://blog.udemy.com/wp-content/uploads/2015/09/image073.png)

<p style="text-align: justify;">
  If you encounter any issues running MongoD, it’s best to check MongoDB’s website for complete installation.
</p>

### <span id="Install_Node">Install Node</span>

<p style="text-align: justify;">
  Simply hit<span class="Apple-converted-space"> </span><a href="https://nodejs.org/">https://nodejs.org</a><span class="Apple-converted-space"> </span>and click the Install button. Whether you’re using Windows or Mac, it’s very straightforward and you won’t have any problems.
</p>

<p style="text-align: justify;">
  As part of installing Node, you’ll get NPM (Node Package Manager) on your machine. NPM in Node is similar to Ruby Gems in Ruby and NuGet in .NET. We use NPM (or any package managers) to download and install open-source reusable packages / modules in our applications.
</p>

### <span id="Install_Express_Generator">Install Express Generator</span>

<p style="text-align: justify;">
  Express Generator is a Node module that we use to scaffold an application. To install Express Generator, open another<span class="Apple-converted-space"> </span><b>Terminal</b><span class="Apple-converted-space"> </span>on Mac or<span class="Apple-converted-space"> </span><b>Command Prompt </b>on Windows and run:
</p>

<pre class="lang:sh decode:true ">npm install -g express-generator</pre>

The -g switch here instructs NPM to install this package at the global level so it’ll be accessible to all your applications.

### <span id="Scaffold_a_Project">Scaffold a Project</span>

<p style="text-align: justify;">
  We got all the tooling ready. Now let’s build an application skeleton using Express Generator. We’re going to call our video rental store application<span class="Apple-converted-space"> </span><b>Vidzy.<span class="Apple-converted-space"> </span></b>So, from the<span class="Apple-converted-space"> </span><b>Command Prompt</b><span class="Apple-converted-space"> </span>or<span class="Apple-converted-space"> </span><b>Terminal</b>, go to a folder on your disk where you create your projects, and run the following command:
</p>

<pre class="lang:sh decode:true">express Vidzy</pre>

Express Generator will scaffold an application under a new folder called Vidzy.

<p style="text-align: justify;">
  Now, open the Vidzy folder in your favorite code editor. If you’re using Sublime, you can simply drag and drop this folder into Sublime.
</p>

Here is the generated app structure:

  * bin 
      * www
  * public 
      * images
      * javascripts
      * stylesheets
  * routes 
      * index.js
      * users.js
  * views 
      * error.jade
      * index.jade
      * layout.jade
  * app.js
  * package.son

<p style="padding-left: 30px; text-align: justify;">
  <b>public</b><span class="Apple-converted-space"> </span>is where we store public assets of our website, such as javascript files, stylesheets, images, etc.
</p>

<p style="padding-left: 30px; text-align: justify;">
  <b>routes</b><span class="Apple-converted-space"> </span>includes a number of Javascript files, each defining routes and their handlers for a given module in your application. A route defines an endpoint in your application where you receive requests. We’ll take a look at routes soon.
</p>

<p style="padding-left: 30px; text-align: justify;">
  <b>views</b><span class="Apple-converted-space"> </span>includes the view files for your application. Express supports many popular template engines such as Jade, Haml, EJS, Handlebars, etc. Jade is the default template engine.
</p>

<p style="padding-left: 30px; text-align: justify;">
  <b>app.js<span class="Apple-converted-space"> </span></b>is the entry point to your application. It’s where your application is defined and configured.
</p>

<p style="padding-left: 30px; text-align: justify;">
  <b>package.json</b>: every Node application has a package.json file, which includes application metadata and identifies application dependencies.
</p>

Let’s open package.json. Here you see a JSON object like the following:

<pre class="lang:js decode:true">{
  "name": "Vidzy",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.13.2",
    "cookie-parser": "~1.3.5",
    "debug": "~2.2.0",
    "express": "~4.13.1",
    "jade": "~1.11.0",
    "morgan": "~1.6.1",
    "serve-favicon": "~2.3.0"
  }
}</pre>

<p style="text-align: justify;">
  As you see, here we specify the name and the version of our application as well as its dependencies. All of these dependencies are Node modules.
</p>

### <span id="Install_Dependencies">Install Dependencies</span>

<p style="text-align: justify;">
  When you generate an application with Express Generator, these dependencies are not installed. They are only referenced in<span class="Apple-converted-space"> </span><b>package.json</b>. You need to separately install these dependencies.
</p>

<p style="text-align: justify;">
  To install the dependencies, go back into the<span class="Apple-converted-space"> </span><b>Terminal</b><span class="Apple-converted-space"> </span>or<span class="Apple-converted-space"> </span><b>Command Prompt</b><span class="Apple-converted-space"> </span>and run the following commands:
</p>

<pre class="lang:js decode:true ">cd Vidzy
npm install</pre>

<p style="text-align: justify;">
  This is going to take a little while to run. NPM will look at<span class="Apple-converted-space"> </span><b>package.json</b><span class="Apple-converted-space"> </span>to identify the dependencies. Then, it will download each of these dependencies from NPM directory and store them in a folder called<span class="Apple-converted-space"> </span><b>node_modules</b>. Let’s take a look at one of these modules.
</p>

<p style="text-align: justify;">
  Inside<span class="Apple-converted-space"> </span><b>Vidzy</b><span class="Apple-converted-space"> </span>folder, go to<span class="Apple-converted-space"> </span><b>node_modules > express</b>. Note that here we have another<span class="Apple-converted-space"> </span><b>package.json</b>, which identifies the dependencies for Express.js. Also, there is another<span class="Apple-converted-space"> </span><b>node_modules</b><span class="Apple-converted-space"> </span>folder, which is where these dependencies are stored. This is a common structure of Node applications. Every module or package has a <b>package.json</b><span class="Apple-converted-space"> </span>file and a<span class="Apple-converted-space"> </span><b>node_modules</b><span class="Apple-converted-space"> </span>folder.
</p>

### <span id="Install_Nodemon">Install Nodemon</span>

<p style="text-align: justify;">
  When you start a Node application, a basic web server starts on port 3000 and accepts incoming requests. If you make any changes to your application code, these changes are not visible in your web server until you restart it. Doing so is a bit tedious as you have to kill the server and start it again from the command prompt. To solve this problem, we use<span class="Apple-converted-space"> </span><b>Nodemon</b>, which is yet another Node module that automatically restarts your web server whenever it detects any changes in your source files.
</p>

To install Nodemon:

<pre class="lang:js decode:true ">npm install nodemon -g</pre>

### <span id="Install_Monk">Install Monk</span>

Monk is a Node module for reading and writing documents in MongoDB.

To install monk:

<pre class="lang:js decode:true ">npm install monk --save</pre>

<p style="text-align: justify;">
  The –save switch here instructs NPM to add this dependency in your <b>package.json</b>. The benefit of this is that if you push this application to a repository, and someone else checks it out (or it could be yourself at some other time), all dependencies of your application are referenced in package.json. Then, all you have to do is simply run<b><span class="Apple-converted-space"> </span>npm install</b>, and this will automatically install all the referenced modules. This is exactly what we did earlier once we generated our application skeleton.
</p>

### <span id="Run_the_Application">Run the Application</span>

<p style="text-align: justify;">
  Congratulations! We’ve installed all the required tools. Now, it’s time to run our sample application. From the<span class="Apple-converted-space"> </span><b>Command Prompt</b><span class="Apple-converted-space"> </span>or<span class="Apple-converted-space"> </span><b>Terminal</b>, run the following command inside the Vidzy folder:
</p>

<pre class="lang:sh decode:true ">nodemon</pre>

<p style="text-align: justify;">
  Nodemon will start your web server on port 3000. You may receive a pop-up warning you that Node is going to access incoming connections. Make sure to allow access.
</p>

Now, launch your browser and navigate to:

[http://localhost:3000](http://localhost:3000/)

Here is your first express application.

[<img class="aligncenter size-full wp-image-147326" src="https://blog.udemy.com/wp-content/uploads/2015/09/image122.png" alt="image12" width="1291" height="775" />](https://blog.udemy.com/wp-content/uploads/2015/09/image122.png)

Over the next few sections, we’ll build various features for our video rental store app.

## <span id="Displaying_All_Videos_in_the_Database">Displaying All Videos in the Database</span>

<p style="text-align: justify;">
  First, let’s start by implementing a simple feature: displaying all videos in the database on the home page. There are different ways to implement this. We can start from the front end and work all the way up to the back end, or vice versa. There is no right or wrong way but in this tutorial, I prefer to start from the back end for educational reasons.
</p>

We’re going to implement this feature in a few steps:

  1. We’ll create a database in Mongo and populate it with a few video documents.
  2. Then, we’ll create an API with Express to expose the videos in the database.
  3. Finally, we’ll use Angular to consume the API and display the list of videos.

<p style="text-align: justify;">
  If you don’t have any experience with any of these technologies, that’s perfectly fine. In this section, I’ll cover all of these from the basics. However, depending on your level of experience, there might be a bit of learning curve. Please be patient, and once we get to implement the next few features, everything will become easier as you re-apply these concepts.
</p>

### <span id="Step_1_Populating_the_Database">Step 1: Populating the Database</span>

<p style="text-align: justify;">
  How do we populate our MongoDB database with some documents? MongoDB has a shell that you can access using<span class="Apple-converted-space"> </span><b>Terminal</b><span class="Apple-converted-space"> </span>on Mac or<span class="Apple-converted-space"> </span><b>Command Prompt<span class="Apple-converted-space"> </span></b>on Windows. However, working with shell is not very user-friendly. So, to make our job easier, we’re going to use a free tool called<span class="Apple-converted-space"> </span><b>RoboMongo</b>. Head over to <a href="http://robomongo.org/">http://robomongo.org</a><span class="Apple-converted-space"> </span>and download RoboMongo for your operating system.
</p>

Lunch RoboMongo. You’ll see a dialog box for connecting to a MongoDB server.

[<img class="aligncenter size-full wp-image-147327" src="https://blog.udemy.com/wp-content/uploads/2015/09/image032.png" alt="image03" width="782" height="550" />](https://blog.udemy.com/wp-content/uploads/2015/09/image032.png)

Click the<span class="Apple-converted-space"> </span>**Create<span class="Apple-converted-space"> </span>**link on the top.

Change the name of this connection from<span class="Apple-converted-space"> </span>**New Connection<span class="Apple-converted-space"> </span>**to<span class="Apple-converted-space"> </span>**localhost**. Note the address of the connection. It points to**<span class="Apple-converted-space"> </span>localhost:27017**. By default, MongoDB starts on port<span class="Apple-converted-space"> </span>**27017**.

[<img class="aligncenter size-full wp-image-147328" src="https://blog.udemy.com/wp-content/uploads/2015/09/image013.png" alt="image01" width="731" height="473" />](https://blog.udemy.com/wp-content/uploads/2015/09/image013.png)

<p style="text-align: justify;">
  If you click the<span class="Apple-converted-space"> </span><b>Test</b><span class="Apple-converted-space"> </span>button, you may get an error like “Authorization skipped by you”. Don’t worry about it. You’ll be able to connect to your local MongoDB regardless.
</p>

Save the connection settings. Back in the<span class="Apple-converted-space"> </span>**Connect**<span class="Apple-converted-space"> </span>dialog box, connect to **localhost**.

From the<span class="Apple-converted-space"> </span>**View**<span class="Apple-converted-space"> </span>menu, tick<span class="Apple-converted-space"> </span>**Explorer**<span class="Apple-converted-space"> </span>if it’s not already ticked. Now your RoboMongo should look like this:

[<img class="aligncenter size-full wp-image-147329" src="https://blog.udemy.com/wp-content/uploads/2015/09/image023.png" alt="image02" width="1403" height="818" />](https://blog.udemy.com/wp-content/uploads/2015/09/image023.png)

<p style="text-align: justify;">
  In the<span class="Apple-converted-space"> </span><b>Explorer</b><span class="Apple-converted-space"> </span>pane, right-click on<span class="Apple-converted-space"> </span><b>localhost</b><span class="Apple-converted-space"> </span>and select<span class="Apple-converted-space"> </span><b>Create Database</b>. Name the database<span class="Apple-converted-space"> </span><b>vidzy</b>. Expand<span class="Apple-converted-space"> </span><b>vidzy</b>, right-click<span class="Apple-converted-space"> </span><b>Collections</b><span class="Apple-converted-space"> </span>and click<span class="Apple-converted-space"> </span><b>Create Collection…</b><span class="Apple-converted-space"> </span>A collection in MongoDB database is similar to a table in a relational database. Name this collection<span class="Apple-converted-space"> </span><b>videos</b>. The new collection should appear in the list.
</p>

<p style="text-align: justify;">
  Next, right-click the<span class="Apple-converted-space"> </span><b>videos</b><span class="Apple-converted-space"> </span>collection and select<b><span class="Apple-converted-space"> </span>Insert Document</b>. A document in MongoDB is kind of similar to a record in a relational database. However, a MongoDb document, unlike a relational record, can contain other documents. In Mongo, we use the JSON format to represent a document. Copy and paste the following code into the dialog to add a video document:
</p>

<pre class="lang:js decode:true ">{
 "title" : "Terminator Genisys",
 "genre" : "SciFi",
 "description" : "When John Connor, leader of the human resistance, sends Sgt. Kyle Reese back to 1984 to protect Sarah Connor and safeguard the future, an unexpected turn of events creates a fractured timeline."
}</pre>

<p style="text-align: justify;">
  <b>Note</b>: Make sure to clear the content of the dialog first before pasting this document, as otherwise you’ll get an invalid JSON object.
</p>

Repeat this step and add two more documents to the videos collection:

<pre class="lang:js decode:true ">{
 "title" : "The Lord of the Rings",
 "genre" : "Fantasy",
 "description" : "A meek hobbit of the Shire and eight companions set out on a journey to Mount Doom to destroy the One Ring and the dark lord Sauron."
}</pre>

<pre class="lang:js decode:true ">{
 "title" : "Apollo 13",
 "genre" : "Drama",
 "description" : "NASA must devise a strategy to return Apollo 13 to Earth safely after the spacecraft undergoes massive internal damage putting the lives of the three astronauts on board in jeopardy."
}</pre>

Now, right-click the<span class="Apple-converted-space"> </span>**videos**<span class="Apple-converted-space"> </span>collection, and select<span class="Apple-converted-space"> </span>**View Documents**. You should see three documents in the<span class="Apple-converted-space"> </span>**videos**<span class="Apple-converted-space"> </span>collection.

[<img class="aligncenter size-full wp-image-147330" src="https://blog.udemy.com/wp-content/uploads/2015/09/image063.png" alt="image06" width="1403" height="818" />](https://blog.udemy.com/wp-content/uploads/2015/09/image063.png)

Note that each document has an ID that is automatically generated by MongoDB.

Done! Our database is ready. Now, let’s create an API using Express to expose these video documents.

### <span id="Step_2_Creating_an_API_with_Express">Step 2: Creating an API with Express</span>

<p style="text-align: justify;">
  In this step, you’re going to learn about Node module system, Express routes and getting data from MongoDB using Monk.
</p>

<p style="text-align: justify;">
  In your favorite code editor, open<span class="Apple-converted-space"> </span><b>app.js<span class="Apple-converted-space"> </span></b>in the root of the project folder. The first section includes a number of<span class="Apple-converted-space"> </span><b>require</b><span class="Apple-converted-space"> </span>calls. The<span class="Apple-converted-space"> </span><b>require<span class="Apple-converted-space"> </span></b>method is one of built-in methods in Node that we use to import modules defined in other files:
</p>

<pre class="lang:js decode:true ">var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');</pre>

<p style="text-align: justify;">
  The second section imports our route module. A route module defines one or more related endpoints and their handlers. In the sample application generated by Express Generator, we have two route modules:<span class="Apple-converted-space"> </span><b>index</b><span class="Apple-converted-space"> </span>and<span class="Apple-converted-space"> </span><b>users</b>:
</p>

<pre class="lang:js decode:true ">var routes = require('./routes/index');
var users = require('./routes/users');</pre>

Let’s take a look at one of these route modules. Open<span class="Apple-converted-space"> </span>**routes > index.js.<span class="Apple-converted-space"> </span>**You should see the following code:

<pre class="lang:js decode:true ">var express = require('express');
var router = express.Router();</pre>

<pre class="lang:js decode:true ">/* GET home page. */
router.get('/', function(req, res, next) {
 res.render('index', { title: 'Express' });
});</pre>

<pre class="lang:js decode:true ">module.exports = router;</pre>

Let me break this down for you.

<p style="text-align: justify;">
  With the first line, we import Express into this module. When using the<span class="Apple-converted-space"> </span><b>require </b>method, depending on how the target module is implemented, the<span class="Apple-converted-space"> </span><b>require</b><span class="Apple-converted-space"> </span>method may return an object or a method. In this case, the<span class="Apple-converted-space"> </span><b>express</b><span class="Apple-converted-space"> </span>variable is an object. It exposes a method called<span class="Apple-converted-space"> </span><b>Router</b>, which we call on the second line, to get access to the router object in Express. We use a router to define endpoints for our application. These are the endpoints where we receive requests. Each endpoint will be associated with a route handler, which is responsible for handling a request that is received in that endpoint.
</p>

Now look at the next line for an example of a route configuration.

<pre class="lang:js decode:true ">router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});</pre>

<p style="text-align: justify;">
  Here, we use the<span class="Apple-converted-space"> </span><b>get</b><span class="Apple-converted-space"> </span>method on the<span class="Apple-converted-space"> </span><b>router</b><span class="Apple-converted-space"> </span>to define a route and its handler. The first argument is the endpoint; in this case,<span class="Apple-converted-space"> </span><b>‘/’</b><span class="Apple-converted-space"> </span>represents the root of the site or the home page. The second argument is the route handler.
</p>

<p style="text-align: justify;">
  In Express, all route handlers have the same signature. The first parameter is the <b>request</b><span class="Apple-converted-space"> </span>object, the second is the<span class="Apple-converted-space"> </span><b>response</b>, and the third references the<span class="Apple-converted-space"> </span><b>next </b>handler in the chain. Express uses middleware functions that are chained together. When building middleware with Express, sometimes you may want to call the next middleware function in the chain. You can use the<span class="Apple-converted-space"> </span><b>next<span class="Apple-converted-space"> </span></b>variable for that. But when working with routes, we hardly ever need to do this, so you can safely delete the<span class="Apple-converted-space"> </span><b>next</b><span class="Apple-converted-space"> </span>variable here.
</p>

<p style="text-align: justify;">
  Now, look at the body of this method. The<span class="Apple-converted-space"> </span><b>res</b><span class="Apple-converted-space"> </span>variable represents the response. The response object has a number of useful methods:
</p>

  * **render**: to render a view
  * **send**: to send plain text content to the client
  * **json**: to send a json object to the client
  * **redirect**: to redirect the client to a new address

Here, we render the index view, which is defined in<span class="Apple-converted-space"> </span>**views > index.jade**.

<p style="text-align: justify;">
  This is the basic structure of a route. Now, we need to create a RESTful API for our videos. We’re going to expose our videos at an endpoint like<span class="Apple-converted-space"> </span><b>/api/videos.</b>
</p>

<p style="text-align: justify;">
  Create a new route module called<span class="Apple-converted-space"> </span><b>videos.js<span class="Apple-converted-space"> </span></b>under<span class="Apple-converted-space"> </span><b>routes<span class="Apple-converted-space"> </span></b>folder, and type the following code into it. Don’t worry, as I’ll explain it line by line.
</p>

<pre class="lang:js decode:true">var express = require('express');
var router = express.Router();

var monk = require('monk');
var db = monk('localhost:27017/vidzy');

router.get('/', function(req, res) {
    var collection = db.get('videos');
    collection.find({}, function(err, videos){
        if (err) throw err;
      	res.json(videos);
    });
});

module.exports = router;</pre>

<p style="text-align: justify;">
  The first two lines are like before. We’re importing Express and getting the router object.
</p>

<p style="text-align: justify;">
  Next, we import Monk, which is a persistence module over MongoDB. There is another popular persistence module for working with Mongo called<span class="Apple-converted-space"> </span><b>Mongoose.</b>But in this tutorial, I preferred to use Monk as it’s easier than Mongoose.
</p>

<p style="text-align: justify;">
  Earlier I told you that depending on how a module is implemented, the<span class="Apple-converted-space"> </span><b>require</b>method may return an object or a method. Here, when we import<span class="Apple-converted-space"> </span>Monk, we receive a method, and not an object. So, the<span class="Apple-converted-space"> </span><b>monk</b><span class="Apple-converted-space"> </span>variable is a method that we call to get access to our database:
</p>

<pre class="lang:js decode:true ">var db = monk('localhost:27017/vidzy');</pre>

Now, look at the implementation of our route handler.

<pre class="lang:js decode:true ">function(req, res) {
    var collection = db.get('videos');
    collection.find({}, function(err, videos){
        if (err) throw err;
      	res.json(videos);
    });
}</pre>

First we call the<span class="Apple-converted-space"> </span>**get**<span class="Apple-converted-space"> </span>method on the<span class="Apple-converted-space"> </span>**db**<span class="Apple-converted-space"> </span>object, passing the name of the collection (**videos**). It returns a collection object. This collection object provides a number of methods to work with documents in that collection.

  * insert
  * find
  * findOne
  * update
  * remove

<p style="text-align: justify;">
  Here, we use the<span class="Apple-converted-space"> </span><b>find</b><span class="Apple-converted-space"> </span>method to get all videos in the collection. The first argument to this method is an object that determines the criteria for filtering. Since we want all videos, we pass an empty object as the argument. The second argument is a callback method that is executed when the result is returned from the database. This method follows the “error-first” callback pattern, which is the standard protocol for callback methods in Node. With this pattern, the first argument of a callback method should be an error object, and the second should be the result (if any). As you develop more applications with Node, you’re going to see more of this callback pattern.
</p>

<p style="text-align: justify;">
  Inside this callback, first we check if the<span class="Apple-converted-space"> </span><b>err</b><span class="Apple-converted-space"> </span>object is set. If there are no errors as part of getting the video documents,<span class="Apple-converted-space"> </span><b>err</b><span class="Apple-converted-space"> </span>will be<span class="Apple-converted-space"> </span><b>null</b>; otherwise it will be set. We throw the<span class="Apple-converted-space"> </span><b>err</b><span class="Apple-converted-space"> </span>here to stop the execution of the program and report an error to the user. If there are no errors, however, we simply return a JSON object by calling<b>res.json<span class="Apple-converted-space"> </span></b>method.
</p>

Finally, look at the last line of this module:

<pre class="lang:js decode:true ">module.exports = router;</pre>

<p style="text-align: justify;">
  With this line, we specify the object (or method) that is returned when we<span class="Apple-converted-space"> </span><b>require </b>this module in another module. In this case, we’re returning the router object in Express. So, the purpose of this module is to get the router, register a few routes on it, and then return it.
</p>

<p style="text-align: justify;">
  There is one tiny step left. We built a module for configuring routes for our new API, but we haven’t used this module anywhere. Open up<span class="Apple-converted-space"> </span><b>app.js<span class="Apple-converted-space"> </span></b>again and look for the following code near the top:
</p>

<pre class="lang:js decode:true ">var routes = require('./routes/index');
var users = require('./routes/users');</pre>

This is where we import our route modules into the application module. Add a new line to this section:

<pre class="lang:js decode:true ">var videos = require('./routes/videos');</pre>

We’re importing our new module and storing it in a variable called<span class="Apple-converted-space"> </span>**videos**. Next, we need to use it. Scroll down in<span class="Apple-converted-space"> </span>**app.js<span class="Apple-converted-space"> </span>**a little bit and find the following section:

<pre class="lang:js decode:true ">app.use('/', routes);
app.use('/users', users);</pre>

Add a new line to this section:

<pre class="lang:default decode:true ">app.use('/api/videos', videos);</pre>

With this line we tell Express to use the<span class="Apple-converted-space"> </span>**videos**<span class="Apple-converted-space"> </span>module for any routes starting with**/api/videos.**

We’re done. Let’s quickly test our new API. Open up your browser, and navigate to [**http://localhost:3000/api/videos**](http://localhost:3000/api/videos)**.<span class="Apple-converted-space"> </span>**You should see all videos represented as JSON.

[<img class="aligncenter size-full wp-image-147336" src="https://blog.udemy.com/wp-content/uploads/2015/09/image082.png" alt="image08" width="1394" height="883" />](https://blog.udemy.com/wp-content/uploads/2015/09/image082.png)

I’m using the<span class="Apple-converted-space"> </span>**JSONView<span class="Apple-converted-space"> </span>**plugin in my Google Chrome to highlight JSON objects.

Over the next few steps, we’ll build the front end using Angular to display these videos.

### <span id="Step_3_Adding_Angular">Step 3: Adding Angular</span>

<p style="text-align: justify;">
  In this step, you’re going to learn the basics of Angular. If you’re already familiar with Angular, you can skip through the explanations, but be sure to apply all code changes.
</p>

<p style="text-align: justify;">
  Angular is a popular front-end framework for building Single Page Applications (SPA). It provides routing, dependency injection, testability and clean separation of concerns by adhering to MVC architectural pattern. If all this sounds too geeky, don’t worry at all. You’re going to see most of these capabilities in this section.
</p>

<p style="text-align: justify;">
  First, we need to add Angular scripts to our application. Go to<span class="Apple-converted-space"> </span><b>views > layout.jade </b>and add these three script references at the end of the<span class="Apple-converted-space"> </span><b>head<span class="Apple-converted-space"> </span></b>section.
</p>

<pre class="lang:js decode:true ">script(src='https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular.js')
script(src='https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-resource.js')
script(src='https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-route.js')</pre>

<p style="text-align: justify;">
  Make sure they are at the same indentation level as previous lines since Jade (the default templating engine in Express) is very sensitive about white space. The Jade views generated by Express Generator use two white space characters for indenting. So, you need to follow the same and you cannot mix this with tabs in the same view. Otherwise, you’re going to get a runtime error.
</p>

<p style="text-align: justify;">
  What are these scripts? The first one is the main Angular framework, the second one (<b>angular-resource</b>) is used for consuming RESTful APIs, and the third one (<b>angular-route</b>) is used for routing. With routing we define what the user should see as they navigate through the application.
</p>

<p style="text-align: justify;">
  Next, create a new file called<span class="Apple-converted-space"> </span><b>vidzy.js<span class="Apple-converted-space"> </span></b>under<span class="Apple-converted-space"> </span><b>public > javascripts.<span class="Apple-converted-space"> </span></b>We’ll write all our Javascript code here.
</p>

<p style="text-align: justify;">
  Add a reference to<span class="Apple-converted-space"> </span><b>vidzy.js<span class="Apple-converted-space"> </span></b>in<span class="Apple-converted-space"> </span><b>layout.jade<span class="Apple-converted-space"> </span></b>just after Angular scripts:
</p>

<pre class="lang:default decode:true ">script(src='/javascripts/vidzy.js')</pre>

Again, make sure that this line is at the same indentation level as other script references in the head section.

Now that we’ve got the necessary scripts in place, it’s time to add Angular to our application. Adding Angular involves two steps:

  * First, we add the<span class="Apple-converted-space"> </span>**ng-app<span class="Apple-converted-space"> </span>**attribute to our HTML element. When the Angular script is loaded, it’ll look for this attribute in the DOM and if it finds it, it’ll hook itself into your application.
<li style="text-align: justify;">
  Next, we create an Angular module for our application. Angular applications consist of one or more modules. With a simple application, you’ll have one module called<span class="Apple-converted-space"> </span><b>app.</b><span class="Apple-converted-space"> </span>But as your application grows, you may want to divide various functionalities into different modules for better organization and maintainability.
</li>

With**<span class="Apple-converted-space"> </span>layout.jade<span class="Apple-converted-space"> </span>**still open in your code editor, add<span class="Apple-converted-space"> </span>**ng-app<span class="Apple-converted-space"> </span>**to the<span class="Apple-converted-space"> </span>**html<span class="Apple-converted-space"> </span>**element near the top:

<pre class="lang:xhtml decode:true ">doctype html
html(ng-app='Vidzy')</pre>

<p style="text-align: justify;">
  In Jade, we use parentheses for adding attributes to an HTML tag. When this line is rendered by Jade template engine, we’ll get an HTML element like the following:
</p>

<pre class="lang:xhtml decode:true ">&lt;html ng-app=’Vidzy’&gt;</pre>

The value we set for<span class="Apple-converted-space"> </span>**ng-app<span class="Apple-converted-space"> </span>**is the name of our application module that kicks off the application. Now we need to create this module.

Open<span class="Apple-converted-space"> </span>**vidzy.js<span class="Apple-converted-space"> </span>**and write this code:

<pre class="lang:js decode:true ">var app = angular.module('Vidzy', []);</pre>

<p style="text-align: justify;">
  The<span class="Apple-converted-space"> </span><b>angular</b><span class="Apple-converted-space"> </span>object is available in the global space, so it’s everywhere. The<span class="Apple-converted-space"> </span><b>module </b>method can be used to define a new module or get a reference to an existing one. The first argument is the name of the module. This is the same name we used with <b>ng-app</b><span class="Apple-converted-space"> </span>above. The second argument is an array of dependencies. If you provide this argument, the<span class="Apple-converted-space"> </span><b>module<span class="Apple-converted-space"> </span></b>method will define a new module and return a reference to it. If you exclude it, it’ll attempt to get a reference to an existing module. Here, I’m passing an empty array to indicate that this module does not depend on any other modules.
</p>

<p style="text-align: justify;">
  That was all we had to do to hook Angular into our application. Next, we’re going to re-build our home page using Angular to display all videos in the database.
</p>

<h3 style="text-align: justify;">
  <span id="Step_4_Rebuilding_the_Home_Page_using_Angular">Step 4: Rebuilding the Home Page using Angular</span>
</h3>

<p style="text-align: justify;">
  The default application generated by Express Generator uses Jade as the view engine. These Jade views are parsed and rendered on the server, and HTML markup is returned to the client. This is how a lot of web frameworks work out of the box. But in this application, we’ll be using a different architectural style. Instead of returning HTML markup to the client, we’ll return plain JSON objects and let the client (Angular) render the view. Let me explain why.
</p>

<p style="text-align: justify;">
  Earlier, in the section about “When to use Node”, I mentioned that a common scenario that Node excelled at was building RESTful APIs over a document database. With this architectural style, we don’t have any overhead of data conversion. We store JSON objects in Mongo, expose them via a RESTful API and consume them directly on the client (in Angular). JSON is native to Javascript and MongoDB. So, by using it throughout the stack, we reduce the overhead of mapping or converting it to other types. By returning JSON objects from our API and rendering views on the client, we can improve performance and scalability because servers’ CPUs will not be wasted to render views for lots of concurrent users. Plus, we can reuse the same API to build another client, like an iPhone or Android app.
</p>

<p style="text-align: justify;">
  In this step, we’re going to retire our default home page that is built with a Jade view, and instead use an Angular view.
</p>

Create a new folder under<span class="Apple-converted-space"> </span>**public<span class="Apple-converted-space"> </span>**called<span class="Apple-converted-space"> </span>**partials.<span class="Apple-converted-space"> </span>**We will store all our views here. Create a new file under this folder called<span class="Apple-converted-space"> </span>**home.html.<span class="Apple-converted-space"> </span>**In this file, simply type:

<pre class="lang:xhtml decode:true ">&lt;h1&gt;Home Page&lt;/h1&gt;</pre>

Now, we need to tell Angular to render this view when the user navigates to the home page. We use Angular routes for this.

In<span class="Apple-converted-space"> </span>**vidzy.js,<span class="Apple-converted-space"> </span>**change the declaration of<span class="Apple-converted-space"> </span>**app<span class="Apple-converted-space"> </span>**module as follows:

<pre>var app = angular.module('Vidzy', ['ngRoute']);</pre>

I added a reference to<span class="Apple-converted-space"> </span>**ngRoute<span class="Apple-converted-space"> </span>**module in the array of dependencies.<span class="Apple-converted-space"> </span>**ngRoute**<span class="Apple-converted-space"> </span>is one of the built-in Angular modules for configuring routes.

Write the following code after declaration of the<span class="Apple-converted-space"> </span>**app<span class="Apple-converted-space"> </span>**module:

<pre><code class="hljs php">app.config([&lt;span class="hljs-string">'$routeProvider'&lt;/span>, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(&lt;span class="hljs-variable">$routeProvider&lt;/span>)&lt;/span>&lt;/span>{
    &lt;span class="hljs-variable">$routeProvider&lt;/span>
        .when(&lt;span class="hljs-string">'/'&lt;/span>, {
            templateUrl: &lt;span class="hljs-string">'partials/home.html'&lt;/span>
        })
        .otherwise({
            redirectTo: &lt;span class="hljs-string">'/'&lt;/span>
        });
}]);
</code></pre>

<p style="text-align: justify;">
  Let me break this down for you. We’re using the<span class="Apple-converted-space"> </span><b>config<span class="Apple-converted-space"> </span></b>method of the<span class="Apple-converted-space"> </span><b>app<span class="Apple-converted-space"> </span></b>module to provide configuration for our application. This code will be run as soon as Angular detects<span class="Apple-converted-space"> </span><b>ng-app<span class="Apple-converted-space"> </span></b>and tries to start up. The<span class="Apple-converted-space"> </span><b>config<span class="Apple-converted-space"> </span></b>method expects an array:
</p>

<pre>app.config([]);</pre>

<p style="text-align: justify;">
  This array can have zero or more dependencies, and a function for implementing configuration. Here, we have a dependency on<span class="Apple-converted-space"> </span><b>$routeProvider</b>, which is a service defined in the<span class="Apple-converted-space"> </span><b>ngRoute<span class="Apple-converted-space"> </span></b>module. That’s why we changed our app module declaration to depend on<span class="Apple-converted-space"> </span><b>ngRoute.  </b>Our configuration function receives <b>$routeProvider<span class="Apple-converted-space"> </span></b>as a parameter.
</p>

<pre>app.config(['$routeProvider', function($routeProvider){
}]);</pre>

Inside our configuration function, we use the<span class="Apple-converted-space"> </span>**when<span class="Apple-converted-space"> </span>**method of<span class="Apple-converted-space"> </span>**$routeProvider<span class="Apple-converted-space"> </span>**to configure a route.

<pre><code class="hljs perl">&lt;span class="hljs-variable">$routeProvider&lt;/span>
        .&lt;span class="hljs-keyword">when&lt;/span>(&lt;span class="hljs-string">'/'&lt;/span>, {
            templateUrl: &lt;span class="hljs-string">'partials/home.html'&lt;/span>
        })
</code></pre>

<p style="text-align: justify;">
  The first argument (<b>‘/’</b>) is the relative path. The second argument is an object that specifies the path to the view (via<span class="Apple-converted-space"> </span><b>templateUrl</b>). We can have multiple calls to the <b>when<span class="Apple-converted-space"> </span></b>method, each specifying a different route. Finally, we use the<span class="Apple-converted-space"> </span><b>otherwise </b>method to indicate that if user navigates to any other URLs, they should be redirected to the root (<b>‘/’</b>).
</p>

<p style="text-align: justify;">
  We’re almost there. We just need to make a few small changes to the Jade view for the home page. So, open<span class="Apple-converted-space"> </span><b>views > index.jade<span class="Apple-converted-space"> </span></b>and change the content of this file as follows:
</p>

<pre>extends layout

block content
  div(ng-view)</pre>

<p style="text-align: justify;">
  I removed the existing content in this view (Welcome to Express) and instead added a<span class="Apple-converted-space"> </span><b>div</b><span class="Apple-converted-space"> </span>with the attribute<span class="Apple-converted-space"> </span><b>ng-view</b>. This attribute tells Angular where to render views. With this, the first time the user hits the home page, our Jade view will be rendered on the server and returned to the client. In a real-world application, this view will have the basic template for the site (e.g., navigation bars, logos, etc.). It will also have a content area (indicated by<span class="Apple-converted-space"> </span><b>ng-view</b>) for Angular to render views. As the user navigates through the application, Angular will replace the content area with a different Angular view. This prevents a full-page reload and results in better performance. That’s why we call these applications Single Page Applications: there is essentially one single page downloaded entirely from the server, and any subsequent pages were simply replacement of the content area.
</p>

* * *

<p style="text-align: justify;">
  <b>Note:<span class="Apple-converted-space"> </span></b>Again, I need to emphasize one more time that Jade is very sensitive about the white space character. You cannot mix space and tab for indentation in the same view. The default Jade views generated by Express Generator use two white space characters for indentation. So, make sure to add two white space characters before<span class="Apple-converted-space"> </span><b>div(ng-view)</b><span class="Apple-converted-space"> </span>or you may get a runtime parsing error.
</p>

* * *

<p style="text-align: justify;">
  Let’s do a quick test before implementing the final step. Back in your browser, navigate to<span class="Apple-converted-space"> </span><a href="http://localhost:3000/"><b>http://localhost:3000</b></a><b>.<span class="Apple-converted-space"> </span></b>You should see our new home page built with Angular:
</p>

[<img class="aligncenter size-full wp-image-147337" src="https://blog.udemy.com/wp-content/uploads/2015/09/image111.png" alt="image11" width="1394" height="883" />](https://blog.udemy.com/wp-content/uploads/2015/09/image111.png)

### <span id="Step_5_Implementing_the_Controller">Step 5: Implementing the Controller</span>

<p style="text-align: justify;">
  The home page is properly hooked in. Now we need to get the videos from the server and render them on the home page. In Angular, or any other MVC controllers, this is the responsibility of the controller. A view is purely responsible for presentation, while a controller is responsible for getting the data for the view, or handling the events raised from the view (e.g., clicks of buttons, etc.).
</p>

<p style="text-align: justify;">
  Let’s create a controller for our home view. Open<span class="Apple-converted-space"> </span><b>vidzy.js<span class="Apple-converted-space"> </span></b>and<span class="Apple-converted-space"> </span>change the declaration of the<span class="Apple-converted-space"> </span><b>app</b><span class="Apple-converted-space"> </span>module as follows:
</p>

<pre>var app = angular.module('Vidzy', ['ngResource', 'ngRoute']);</pre>

Now we depend on two modules:<span class="Apple-converted-space"> </span>**ngResource,<span class="Apple-converted-space"> </span>**for consuming RESTful APIs and **ngRoute<span class="Apple-converted-space"> </span>**for routing.

Next, type the following code at the end of the file to create a controller:

<pre><code class="hljs php">app.controller(&lt;span class="hljs-string">'HomeCtrl'&lt;/span>, [&lt;span class="hljs-string">'$scope'&lt;/span>, &lt;span class="hljs-string">'$resource'&lt;/span>, 
    &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(&lt;span class="hljs-variable">$scope&lt;/span>, &lt;span class="hljs-variable">$resource&lt;/span>)&lt;/span>&lt;/span>{
    }]);
</code></pre>

Let me break this down for you.

<p style="text-align: justify;">
  Here we’re using the<span class="Apple-converted-space"> </span><b>controller<span class="Apple-converted-space"> </span></b>method of the<span class="Apple-converted-space"> </span><b>app<span class="Apple-converted-space"> </span></b>module to define a new controller.
</p>

<p style="text-align: justify;">
  The first parameter is a string that specifies the name of this controller. By convention, we append<span class="Apple-converted-space"> </span><b>Ctrl<span class="Apple-converted-space"> </span></b>to our Angular controller names.
</p>

<p style="text-align: justify;">
  The second argument is an array. This array can include zero or more strings, each representing a dependency for this controller. Here, we’re specifying a dependency to<span class="Apple-converted-space"> </span><b>$scope<span class="Apple-converted-space"> </span></b>and<span class="Apple-converted-space"> </span><b>$resource</b>. Both of these are built-in Angular services, and that’s why they are prefixed with a $ sign. We’ll use<span class="Apple-converted-space"> </span><b>$scope<span class="Apple-converted-space"> </span></b>to pass data to the view and<span class="Apple-converted-space"> </span><b>$resource<span class="Apple-converted-space"> </span></b>to consume a RESTful API. The last object in this array of dependencies is a function that represents the body or implementation of the controller. In this example, our function gets two parameters called<span class="Apple-converted-space"> </span><b>$scope<span class="Apple-converted-space"> </span></b>and <b>$resource.<span class="Apple-converted-space"> </span></b>This is because we referenced<span class="Apple-converted-space"> </span><b>$scope<span class="Apple-converted-space"> </span></b>and<span class="Apple-converted-space"> </span><b>$resource<span class="Apple-converted-space"> </span></b>before declaring this function.
</p>

<p style="text-align: justify;">
  Let’s implement the body of this controller. Inside the controller function, type the following code:
</p>

<pre><code class="hljs php">app.controller(&lt;span class="hljs-string">'HomeCtrl'&lt;/span>, [&lt;span class="hljs-string">'$scope'&lt;/span>, &lt;span class="hljs-string">'$resource'&lt;/span>, 
    &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(&lt;span class="hljs-variable">$scope&lt;/span>, &lt;span class="hljs-variable">$resource&lt;/span>)&lt;/span>&lt;/span>{
        &lt;span class="hljs-keyword">var&lt;/span> Videos = &lt;span class="hljs-variable">$resource&lt;/span>(&lt;span class="hljs-string">'/api/videos'&lt;/span>);
        Videos.query(&lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(videos)&lt;/span>&lt;/span>{
            &lt;span class="hljs-variable">$scope&lt;/span>.videos = videos;
        });
    }]);
</code></pre>

<p style="text-align: justify;">
  Here, we call the<span class="Apple-converted-space"> </span><b>$resource<span class="Apple-converted-space"> </span></b>method to get a resource object for the given API endpoint (<b>/api/videos</b>). This object will provide methods to work with our API. We use the<span class="Apple-converted-space"> </span><b>query<span class="Apple-converted-space"> </span></b>method to get all videos. The<span class="Apple-converted-space"> </span><b>query<span class="Apple-converted-space"> </span></b>method gets a callback method that will be executed when the query result is ready. This function will receive the videos we retrieved from the server. Finally, we store these videos in <b>$scope<span class="Apple-converted-space"> </span></b>so that we can access them in the view for rendering. Remember,<span class="Apple-converted-space"> </span><b>$scope<span class="Apple-converted-space"> </span></b>is the glue between views and controllers.
</p>

<p style="text-align: justify;">
  Now, we need to change the view to render the list of views. Open<span class="Apple-converted-space"> </span><b>partials > home.html<span class="Apple-converted-space"> </span></b>and write the following code:
</p>

<pre><code class="hljs xml">&lt;span class="hljs-tag">&lt;&lt;span class="hljs-title">ul&lt;/span>&gt;&lt;/span>
    &lt;span class="hljs-tag">&lt;&lt;span class="hljs-title">li&lt;/span> &lt;span class="hljs-attribute">ng-repeat&lt;/span>=&lt;span class="hljs-value">'video in videos'&lt;/span>&gt;&lt;/span>{{video.title}}&lt;span class="hljs-tag">&lt;/&lt;span class="hljs-title">li&lt;/span>&gt;&lt;/span>
&lt;span class="hljs-tag">&lt;/&lt;span class="hljs-title">ul&lt;/span>&gt;&lt;/span>
</code></pre>

<p style="text-align: justify;">
  We’re using a UL and LI to render the list of videos. Our LI has an Angular-specific attribute called<span class="Apple-converted-space"> </span><b>ng-repeat.<span class="Apple-converted-space"> </span></b>These attributes in Angular are called<span class="Apple-converted-space"> </span><i>directives<span class="Apple-converted-space"> </span></i>and are used to add behavior to HTML elements. The value of<span class="Apple-converted-space"> </span><b>ng-repeat<span class="Apple-converted-space"> </span></b>is an expression similar to a foreach expression in Javascript. So,<span class="Apple-converted-space"> </span><b>videos<span class="Apple-converted-space"> </span></b>is essentially the property we set in the<span class="Apple-converted-space"> </span><b>$scope<span class="Apple-converted-space"> </span></b>earlier in the controller.<span class="Apple-converted-space"> </span><b>video in videos<span class="Apple-converted-space"> </span></b>represents one video at a time in this array. So, this<span class="Apple-converted-space"> </span><b>LI<span class="Apple-converted-space"> </span></b>element will be repeated for each video in the videos array. We use<span class="Apple-converted-space"> </span><b>{{ }}<span class="Apple-converted-space"> </span></b>to write expressions. Here, we simply render the title of each video in our LI.
</p>

<p style="text-align: justify;">
  Finally, we need to register this controller as part of the route. Back in<span class="Apple-converted-space"> </span><b>vidzy.js</b>, change the route configuration as follows:
</p>

<pre><code class="hljs perl">.&lt;span class="hljs-keyword">when&lt;/span>(&lt;span class="hljs-string">'/'&lt;/span>, {
    templateUrl: &lt;span class="hljs-string">'partials/home.html'&lt;/span>,
    controller: &lt;span class="hljs-string">'HomeCtrl'&lt;/span>
})
</code></pre>

With this, we’re telling Angular that when the user navigates to the root of the site, display<span class="Apple-converted-space"> </span>**partials/home.html<span class="Apple-converted-space"> </span>**and attach<span class="Apple-converted-space"> </span>**HomeCtrl<span class="Apple-converted-space"> </span>**controller to it.

We’re done. Go back to the browser and refresh the home page. You should see the list of videos.

[<img class="aligncenter size-full wp-image-147339" src="https://blog.udemy.com/wp-content/uploads/2015/09/image043.png" alt="image04" width="1394" height="883" />](https://blog.udemy.com/wp-content/uploads/2015/09/image043.png)

<p style="text-align: justify;">
  If you’re new to Angular and are a little confused, that’s perfectly fine. We’ll work with Angular controllers, views and routes more in the next few sections.
</p>

<p style="text-align: justify;">
  Let’s quickly recap. In this section we added the first feature to our application. We started by using RoboMongo to connect to MongoDB. We created a database and populated it with a few video documents. Then, we created an API using Express to expose the list of videos. Finally, we added Angular to our application and consumed our API to render the list of videos.
</p>

<p style="text-align: justify;">
  In the next section, we’ll add another feature to our application.
</p>

## <span id="Adding_a_New_Video">Adding a New Video</span>

<p style="text-align: justify;">
  In this section, you’re going to learn more about creating API endpoints with Express, building forms using Angular, and storing documents in Mongo using Monk.
</p>

<p style="text-align: justify;">
  Similar to the last section, we’re going to implement this feature in a few steps from the back end to the front end. First, we’ll create an API endpoint for adding videos. We’ll use Express routes to create this endpoint and Monk to store a video document in Mongo. Then, we’ll create a new page with a form to add a video. We’re going to build this page using Angular.
</p>

Let’s get started.

### <span id="Step_1_Creating_an_API_Endpoint">Step 1: Creating an API Endpoint</span>

Open<span class="Apple-converted-space"> </span>**routes > videos.js**<span class="Apple-converted-space"> </span>and add this new route after the existing one and before **module.exports**<span class="Apple-converted-space"> </span>(remember, module.exports should be the last line of your module):

<pre><code class="hljs php">router.post(&lt;span class="hljs-string">'/'&lt;/span>, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(req, res)&lt;/span>&lt;/span>{
    &lt;span class="hljs-keyword">var&lt;/span> collection = db.get(&lt;span class="hljs-string">'videos'&lt;/span>);
    collection.insert({
        title: req.body.title,
        description: req.body.description
    }, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(err, video)&lt;/span>&lt;/span>{
        &lt;span class="hljs-keyword">if&lt;/span> (err) &lt;span class="hljs-keyword">throw&lt;/span> err;

        res.json(video);
    });
});
</code></pre>

<p style="text-align: justify;">
  The code should be reasonably familiar. Let me go over the important parts. First, note the use of<span class="Apple-converted-space"> </span><b>router.post</b><span class="Apple-converted-space"> </span>method. In the last section, we used<span class="Apple-converted-space"> </span><b>router.get </b>method for handling an HTTP GET request. Here, we use HTTP POST, which is the REST convention for creating new objects.
</p>

<p style="text-align: justify;">
  In the route handler, first we get a reference to the<span class="Apple-converted-space"> </span><b>videos</b><span class="Apple-converted-space"> </span>collection and then use the<span class="Apple-converted-space"> </span><b>insert</b><span class="Apple-converted-space"> </span>method to add a new document to Mongo.
</p>

<p style="text-align: justify;">
  The first argument to this method is a JSON object with two properties:<span class="Apple-converted-space"> </span><b>title</b><span class="Apple-converted-space"> </span>and <b>description</b>. We read the values for these properties using<span class="Apple-converted-space"> </span><b>req.body.</b><span class="Apple-converted-space"> </span>This object represents the data that will be posted in the body of the request.
</p>

<p style="text-align: justify;">
  Finally, in the callback method for inserting a document, if we don’t get any errors, we use the<span class="Apple-converted-space"> </span><b>json<span class="Apple-converted-space"> </span></b>method of the response (<b>res</b>)<span class="Apple-converted-space"> </span>to return a JSON representation of the new video document.
</p>

### <span id="Step_2_Creating_a_Form">Step 2: Creating a Form</span>

Okay, so the API is ready. Now we need a form for adding a video.

Create a new view file called<span class="Apple-converted-space"> </span>**video-form.html<span class="Apple-converted-space"> </span>**under<span class="Apple-converted-space"> </span>**public > partials.<span class="Apple-converted-space"> </span>**Type the following code inside this view:

<pre class="lang:xhtml decode:true ">&lt;h1&gt;Add a Video&lt;/h1&gt;

&lt;form&gt;
    &lt;div&gt;
        &lt;label&gt;Title&lt;/label&gt;		
        &lt;input&gt;&lt;/input&gt;
    &lt;/div&gt;
    &lt;div&gt;
        &lt;label&gt;Description&lt;/label&gt;
        &lt;textarea&gt;&lt;/textarea&gt;
    &lt;/div&gt;
    &lt;input type="button" value="Save"&gt;&lt;/input&gt;	
&lt;/form&gt;
</pre>

Now, we need to tell Angular to present this view when the user navigates to **/add-video**. We need a new route for that.

Open<span class="Apple-converted-space"> </span>**vidzy.js**<span class="Apple-converted-space"> </span>and update the routing configuration code as follows:

<pre><code class="hljs php">
app.config([&lt;span class="hljs-string">'$routeProvider'&lt;/span>, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(&lt;span class="hljs-variable">$routeProvider&lt;/span>)&lt;/span>&lt;/span>{
    &lt;span class="hljs-variable">$routeProvider&lt;/span>
        .when(&lt;span class="hljs-string">'/'&lt;/span>, {
            templateUrl: &lt;span class="hljs-string">'partials/home.html'&lt;/span>,
            controller: &lt;span class="hljs-string">'HomeCtrl'&lt;/span>
        })
        .when(&lt;span class="hljs-string">'/add-video'&lt;/span>, {
            templateUrl: &lt;span class="hljs-string">'partials/video-form.html'&lt;/span>
        })
        .otherwise({
            redirectTo: &lt;span class="hljs-string">'/'&lt;/span>
        });
}]);
</code></pre>

Note that I haven’t set the controller here, because we don’t have a controller yet. We’ll do that in the next step.

The view and route are ready. Finally, we need to add a link to<span class="Apple-converted-space"> </span>**/add-video<span class="Apple-converted-space"> </span>**in our home page. Open<span class="Apple-converted-space"> </span>**partials > home.html<span class="Apple-converted-space"> </span>**and add a new link above the<span class="Apple-converted-space"> </span>**UL:**

<pre class="lang:xhtml decode:true">&lt;p&gt;
    &lt;a href="/#/add-video"&gt;Add a Video&lt;/a&gt;
&lt;/p&gt;
</pre>

<p style="text-align: justify;">
  Note that you should prefix the links in your Angular application with<span class="Apple-converted-space"> </span><code>&lt;b>/#&lt;/b></code><code>.</code><span class="Apple-converted-space"> </span>This is used for compatibility with older browsers that don’t support single-page applications.
</p>

<p style="text-align: justify;">
  Let’s preview what we’ve done so far. Go back to your browser, and refresh the home page. You should see the link to add a video. Click the link and see the Add Video page.
</p>

<p style="text-align: justify;">
  I have to admit that this form looks really ugly and it is far from a real-world application. Let’s give it a nice, modern look.
</p>

### <span id="Step_3_Adding_Bootstrap">Step 3: Adding Bootstrap</span>

<p style="text-align: justify;">
  We’re going to use Bootstrap for adding a bit of style to our form. In case you’re not familiar with Bootstrap, it’s a front-end CSS framework for building modern and responsive web applications. In this step, we’re going to reference Bootstrap CSS file and decorate our form elements with a few Bootstrap classes.
</p>

Open<span class="Apple-converted-space"> </span>**views > layout.jade**.

Add this line at the end of the<span class="Apple-converted-space"> </span>**head**<span class="Apple-converted-space"> </span>section:

<pre>link(rel='stylesheet', href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css')</pre>

Make sure it has the same indent level as the lines above.

Now, back to<span class="Apple-converted-space"> </span>**partials > video-form.html**. Add the following classes to the HTML elements:

<pre class="lang:xhtml decode:true ">&lt;h1&gt;Add a Video&lt;/h1&gt;

&lt;form&gt;
    &lt;div class="form-group"&gt;
        &lt;label&gt;Title&lt;/label&gt;		
        &lt;input class="form-control"&gt;&lt;/input&gt;
    &lt;/div&gt;
    &lt;div class="form-group"&gt;
        &lt;label&gt;Description&lt;/label&gt;
        &lt;textarea class="form-control"&gt;&lt;/textarea&gt;
    &lt;/div&gt;
    &lt;input type="button" class="btn btn-primary" value="Save"&gt;&lt;/input&gt;	
&lt;/form&gt;
</pre>

These classes are standard Bootstrap classes for creating forms. For more information about how to create modern forms with Bootstrap, check out [Bootstrap documentation](http://getbootstrap.com/).

Let’s get back to the browser and refresh the page.

[<img class="aligncenter size-full wp-image-147341" src="https://blog.udemy.com/wp-content/uploads/2015/09/image003.png" alt="image00" width="1394" height="883" />](https://blog.udemy.com/wp-content/uploads/2015/09/image003.png)

It looks much better.

Now, this form has no behavior. Nothing happens if you click the Save button. That’s what we’re going to implement next.

### <span id="Step_4_Implementing_the_Controller">Step 4: Implementing the Controller</span>

<p style="text-align: justify;">
  As I told you before, in MVC frameworks, a controller is responsible for handling events raised by the view. We’re going to create an Angular controller to handle the click event of the Save button.
</p>

Open<span class="Apple-converted-space"> </span>**vidzy.js<span class="Apple-converted-space"> </span>**and type the following code at the end of the file:

<pre class="lang:js decode:true ">app.controller('AddVideoCtrl', ['$scope', '$resource', '$location',
    function($scope, $resource, $location){
        $scope.save = function(){
            var Videos = $resource('/api/videos');
            Videos.save($scope.video, function(){
                $location.path('/');
            });
        };
    }]);</pre>

This controller has three dependencies:<span class="Apple-converted-space"> </span>**$scope**<span class="Apple-converted-space"> </span>as the glue between the controller and the view,<span class="Apple-converted-space"> </span>**$resource<span class="Apple-converted-space"> </span>**for working with our RESTful API, and<span class="Apple-converted-space"> </span>**$location<span class="Apple-converted-space"> </span>**for changing the URL in the browser address bar. All of these are built-in Angular services.

<p style="text-align: justify;">
  Inside the body of this controller, we define the<span class="Apple-converted-space"> </span><b>save<span class="Apple-converted-space"> </span></b>method on the<span class="Apple-converted-space"> </span><b>$scope.</b><span class="Apple-converted-space"> </span>This method will be called when the user clicks the Save button. We’ll hook this to our view shortly. For now, let’s see what’s happening inside this method.
</p>

<p style="text-align: justify;">
  First, we call the<span class="Apple-converted-space"> </span><b>$resource<span class="Apple-converted-space"> </span></b>method<span class="Apple-converted-space"> </span>passing the address of our API (<b>/api/videos</b>). This returns an object with methods to work with the API. In the last section, we used the<span class="Apple-converted-space"> </span><b>query<span class="Apple-converted-space"> </span></b>method to get all videos. Here, we use the<span class="Apple-converted-space"> </span><b>save</b><span class="Apple-converted-space"> </span>method to post a video to our API.
</p>

<p style="text-align: justify;">
  The<span class="Apple-converted-space"> </span><b>videos.save<span class="Apple-converted-space"> </span></b>method takes two parameters: the object to post, and the callback function, which will indicate that the asynchronous call is complete. In the callback, we use the<span class="Apple-converted-space"> </span><b>$location<span class="Apple-converted-space"> </span></b>service to change the browser’s address to the root of the site. Angular knows (based on our routes) that this root URL is bound to the <b>home<span class="Apple-converted-space"> </span></b>view. It will display the home page to the user.
</p>

Now, open<span class="Apple-converted-space"> </span>**partials > video-form.html<span class="Apple-converted-space"> </span>**and change the input fields as follows:

<pre class="lang:xhtml decode:true ">&lt;div class="form-group"&gt;
	&lt;label&gt;Title&lt;/label&gt;		
	&lt;input class="form-control" ng-model="video.title"&gt;&lt;/input&gt;
&lt;/div&gt;
&lt;div class="form-group"&gt;
	&lt;label&gt;Description&lt;/label&gt;
	&lt;textarea class="form-control" ng-model="video.description"&gt;&lt;/textarea&gt;
&lt;/div&gt;
</pre>

<p style="text-align: justify;">
  The<span class="Apple-converted-space"> </span><b>ng-model<span class="Apple-converted-space"> </span></b>attribute is another directive that we use for data binding. With this, we tell Angular that anytime the value in our input fields is changed, it should automatically update the referenced property of $scope. In the first example, when the value of title text box is changed, Angular will automatically set it in <b>$scope.video.title</b>.
</p>

Next, change the button declaration as follows:

<pre class="lang:xhtml decode:true ">&lt;input type="button" class="btn btn-primary" value="Save" ng-click="save()"&gt;&lt;/input&gt;</pre>

<p style="text-align: justify;">
  The<span class="Apple-converted-space"> </span><b>ng-click</b><span class="Apple-converted-space"> </span>attribute is yet another Angular directive that we use to handle the click event of HTML elements. With this directive, we’re telling Angular that if the user clicks this button, it should execute the<span class="Apple-converted-space"> </span><b>save<span class="Apple-converted-space"> </span></b>method on the<span class="Apple-converted-space"> </span><b>$scope</b>, which we defined a minute ago.
</p>

Finally, register this new controller in the route:

<pre class="lang:default decode:true ">.when('/add-video', {
	templateUrl: 'partials/video-form.html',
	controller: 'AddVideoCtrl'
})</pre>

<p style="text-align: justify;">
  We’re done. Let’s test the application. Go back to the browser, and fill out the form and submit it. You should see a new video in the list.
</p>

<p style="text-align: justify;">
  Let’s quickly recap what you’ve learned in this section. We created a new API endpoint using Express and used Monk to store a video document in Mongo. Then, we created an Angular view with a form to add a video. We spiced up our form by using Bootstrap. Finally, we created the controller for this view to handle the click event. In the handler for the click event, we used the<span class="Apple-converted-space"> </span><b>$resource<span class="Apple-converted-space"> </span></b>service to post the data to the server.
</p>

In the next section, we’ll add the ability to edit an existing video.

## <span id="Editing_a_Video">Editing a Video</span>

<p style="text-align: justify;">
  In this section you’re going to see another example of creating API endpoints, Angular views, controllers and routes.
</p>

<p style="text-align: justify;">
  We’re going to approach this section in a similar manner to the last one. First, we’ll build two API endpoints: one for getting a video with an ID, the other for updating a video. Then, we’ll add a link to each video in our home page. When the user clicks this link, they’ll be redirected to a form populated with the video details. When they click Save, changes will be saved and they will be returned to the home page.
</p>

### <span id="Step_1_Creating_the_API_Endpoints"><a id="9_1"></a>Step 1: Creating the API Endpoints</span>

This step should already be very familiar. We’re going to create two new routes:

<pre>GET /api/videos/{id}
PUT /api/videos/{id}</pre>

So, open **routes > videos.js**. and add the following route:

<pre class="lang:js decode:true ">router.get('/:id', function(req, res) {
    var collection = db.get('videos');
    collection.findOne({ _id: req.params.id }, function(err, video){
        if (err) throw err;

      	res.json(video);
    });
});</pre>

Note that here we have a route parameter, indicated by a colon (**:id**). You can access the value of this parameter using **req.params.id.**

<p style="text-align: justify;">
  Other than that, the rest of this route configuration is similar to the ones you’ve seen before. The only difference is that we’re using the <b>findOne </b>method of the collection to return only one object. The first argument to this method is the criteria object. So, we’re looking for a document with <b>_id </b>equal to <b>req.params.id</b>.
</p>

Now, create another route:

<pre><code class="hljs php">router.put(&lt;span class="hljs-string">'/:id'&lt;/span>, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(req, res)&lt;/span>&lt;/span>{
    &lt;span class="hljs-keyword">var&lt;/span> collection = db.get(&lt;span class="hljs-string">'videos'&lt;/span>);
    collection.update({
        _id: req.params.id
    },
    {
        title: req.body.title,
        description: req.body.description
    }, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(err, video)&lt;/span>&lt;/span>{
        &lt;span class="hljs-keyword">if&lt;/span> (err) &lt;span class="hljs-keyword">throw&lt;/span> err;

        res.json(video);
    });
});
</code></pre>

<p style="text-align: justify;">
  Note that we’ve defined this route using <b>router.put. </b>This handler will be called only when there is an HTTP PUT request to this endpoint.
</p>

<p style="text-align: justify;">
  We use the <b>update</b> method of the collection object to update a document. The first argument is the criteria object. We’re trying to update only the document with <b>_id</b> equal to <b>req.params.id</b>. The second argument represents the values to update.
</p>

The rest is pretty straightforward.

### <span id="Step_2_Creating_the_Edit_Page"><a id="9_2"></a>Step 2: Creating the Edit Page</span>

First, we need to add a link to each video on the home page. Open **partials > home.html** and change the LI as follows:

<pre class="lang:default decode:true ">&lt;li ng-repeat='video in videos'&gt;
        &lt;a href="/#/video/{{video._id}}"&gt;
            {{video.title}}
        &lt;/a&gt;
    &lt;/li&gt;</pre>

We’re using a Angular binding expression to render a dynamic URL based on the ID of the video. Again, note that the URL is prefixed with **/#** for compatibility with older browsers.

<p style="text-align: justify;">
  Now we need a view with a form to edit a video. However, we already have a view with a form. So, it’s best to reuse it.
</p>

In **vidzy.js**, add this new route:

<pre class="lang:js decode:true ">.when('/video/:id', {
        templateUrl: 'partials/video-form.html'
    })</pre>

Since we haven’t created the controller yet, I haven’t set the controller for the route. Let’s just review our work up to this point.

<p style="text-align: justify;">
  Go back to the browser, go to the home page and make sure to refresh the page. Now, each video should be represented using a hyperlink. Click a video. You should see an empty form.
</p>

<p style="text-align: justify;">
  In the next step, we’ll add behavior to this form. We’ll populate the form upon load and handle clicking the Save button.
</p>

### <span id="Step_3_Implementing_the_Controller"><a id="9_3"></a>Step 3: Implementing the Controller</span>

<p style="text-align: justify;">
  Now we have two choices. We can create a new controller, like <b>EditVideoCtrl</b>, or reuse the existing controller (<b>AddVideoCtrl</b>). What do you think is the best solution? The answer is: there is no best solution. It all depends. If two cases (Add and Edit) have a lot of similarities with few differences, it makes sense to create one controller that handles both scenarios. You’d need a few conditional statements to distinguish between Add and Edit scenarios. On the other hand, if the behaviours in these two scenarios are fairly different, you’ll end up with a lot of ugly conditional statements in one controller. In that case, it’s better to separate them into two different controllers.
</p>

<p style="text-align: justify;">
  Before writing this step of the tutorial, I started by reusing the same controller and I wasn’t very happy with the end result. So, I decided to split them into two different controllers.
</p>

In **vidzy.js**, create a new controller as follows:

<pre><code class="hljs php">app.controller(&lt;span class="hljs-string">'EditVideoCtrl'&lt;/span>, [&lt;span class="hljs-string">'$scope'&lt;/span>, &lt;span class="hljs-string">'$resource'&lt;/span>, &lt;span class="hljs-string">'$location'&lt;/span>, &lt;span class="hljs-string">'$routeParams'&lt;/span>,
    &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(&lt;span class="hljs-variable">$scope&lt;/span>, &lt;span class="hljs-variable">$resource&lt;/span>, &lt;span class="hljs-variable">$location&lt;/span>, &lt;span class="hljs-variable">$routeParams&lt;/span>)&lt;/span>&lt;/span>{	
        &lt;span class="hljs-keyword">var&lt;/span> Videos = &lt;span class="hljs-variable">$resource&lt;/span>(&lt;span class="hljs-string">'/api/videos/:id'&lt;/span>, { id: &lt;span class="hljs-string">'@_id'&lt;/span> }, {
            update: { method: &lt;span class="hljs-string">'PUT'&lt;/span> }
        });

        Videos.get({ id: &lt;span class="hljs-variable">$routeParams&lt;/span>.id }, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(video)&lt;/span>&lt;/span>{
            &lt;span class="hljs-variable">$scope&lt;/span>.video = video;
        });

        &lt;span class="hljs-variable">$scope&lt;/span>.save = &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">()&lt;/span>&lt;/span>{
            Videos.update(&lt;span class="hljs-variable">$scope&lt;/span>.video, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">()&lt;/span>&lt;/span>{
                &lt;span class="hljs-variable">$location&lt;/span>.path(&lt;span class="hljs-string">'/'&lt;/span>);
            });
        }
    }]);
</code></pre>

<p style="text-align: justify;">
  Let me explain what is happening here.
</p>

<p style="text-align: justify;">
  First, this controller, unlike our <b>AddVideoCtrl</b>, has four dependencies. We have an extra dependency here: <b>$routeParams</b>, which we use for accessing parameters in the route (URL). In this case, the ID of the video to edit will be our route parameter.
</p>

<p style="text-align: justify;">
  In the body of this controller, first we use the <b>$resource </b>service to get an object to work with our API endpoint. But this time, we’re using the <b>$resource </b>service in a different way.
</p>

<pre class="">var Videos = $resource('/api/videos/:id', { id: '@_id' }, {
     update: { method: 'PUT' }
});</pre>

<p style="text-align: justify;">
  The first argument is the URL to our endpoint. However, here we have a route parameter indicated by a colon (<b>:id</b>). We’re using a parameterized route because the two API endpoints we created earlier in this section included route parameters.
</p>

<pre>GET /api/videos/:id
PUT /api/videos/:id</pre>

The second argument to this method is an object that provides default values for the **:id** parameter in our route.

<pre>{ id: '@_id' }</pre>

<p style="text-align: justify;">
  Here, ‘<b>@_id’</b> tells Angular to look for a property called <b>_id </b>in the object included in the body of the request. So, when we send a request to <b>PUT /api/videos/:id</b>, Angular will use the <b>_id </b>property of the video object in the body of the request, to set the <b>:id </b>parameter in the route.
</p>

<p style="text-align: justify;">
  The third argument to the <b>$resource </b>method is used for extending the <b>$resource</b> service.
</p>

<pre class="">{
 update: { method: 'PUT' }
}</pre>

<p style="text-align: justify;">
  For some reason only known to developers of Angular, by default you cannot send HTTP PUT requests using <b>$resource </b>service. You need to extend it by adding an <b>update </b>method that uses HTTP PUT.
</p>

Next, we use **Videos.get** to get the video with the given ID.

<pre class="lang:js decode:true ">Videos.get({ id: $routeParams.id }, function(video){
    $scope.video = video;
});</pre>

This is to populate the form upon page load. The first argument to **Videos.get** provides the value for the **:id** parameter in the route. We use **$routeParams.id**, which gives us access to the parameters in the browser’s address bar. Remember the declaration of the route for the edit page?

<pre class="lang:js decode:true ">.when('/video/:id', {
    templateUrl: 'partials/video-form.html',
})</pre>

There, we used a route parameter (**:id**). So, we can access it with **$routeParams.**

<p style="text-align: justify;">
  In the callback method for <b>Videos. get</b>, we get the video returned from the server and store it in the <b>$scope. </b>This way, with Angular’s two-way binding running in the background, the form will be automatically populated with our video object. Remember <b>ng-model</b>? We bound our input fields to properties of the <b>video </b>object on <b>$scope. </b>Any changes in the input field will be reflected in the <b>$scope</b>, and vice versa.
</p>

<p style="text-align: justify;">
  Finally, in the body of the controller, we define a <b>save </b>method, which will be called when the user clicks the Save button.
</p>

<pre class="lang:js decode:true ">$scope.save = function(){
    Videos.update($scope.video, function(){
        $location.path('/');
    });
}</pre>

<p style="text-align: justify;">
  Note that here, instead of using <b>Videos.save</b>, we’re using <b>Videos.update</b>. This is the new method we defined earlier when extending the <b>$resource </b>service. So, this will issue an HTTP PUT request to our API endpoint.
</p>

<p style="text-align: justify;">
  We’re almost done. Our new controller is ready. We just need to reference it in the route configuration. Change the route configuration as follows:
</p>

<pre class="lang:js decode:true ">.when('/video/:id', {
        templateUrl: 'partials/video-form.html',
        controller: 'EditVideoCtrl'
    })</pre>

Let’s test this new feature. Go back to the browser, and refresh the home page. Select one of the videos. Make a few changes and click the Save button. Everything should work.

<p style="text-align: justify;">
  In the next section, we’ll add one more feature to our video rental store app to build the full CRUD functionality.
</p>

## <span id="Deleting_a_Video"><a id="10"></a>Deleting a Video</span>

<p style="text-align: justify;">
  In this section, you’re going to learn how to delete documents using Monk. You’ll also review what you learned about Express and Angular one more time. By the end of this section, all of the pieces of the puzzle should fit together.
</p>

<p style="text-align: justify;">
  We’re going to implement this feature in a few steps. Starting from the server, we’ll build an API endpoint for deleting video documents. Then, we’ll add a Delete link in front of each video in the list. When the user clicks this link, they’ll be redirected to a page where they can see details of the video. We’ll have a Confirm Delete button on that page. Once they click this button, a call will be made to our API endpoint, and then they’ll be taken back to the video list.
</p>

<p style="text-align: justify;">
  But before we jump into implementation, I have a suggestion. I think that with what you’ve learned so far, you should be able to implement this feature on your own. I want you to spend 10 – 15 minutes working on this feature as an exercise to apply what you’ve learned so far. Then, you can come back and review your solution with mine to see if there is any room for improvement. Before you get started, note that:
</p>

<li style="text-align: justify;">
  It’s best to do this exercise in a step-by-step fashion in the same order I mentioned earlier. Once you complete each step, come back here and review your solution.
</li>
<li style="text-align: justify;">
  If you forgot the syntax for Express or Angular APIs, don’t worry. It’s completely natural when learning a new framework. Just browse the source code for similar examples we did earlier.
</li>
<li style="text-align: justify;">
  If you get stuck, feel free to do a Google search.
</li>

### <span id="Step_1_Building_the_API_Endpoint"><a id="10_1"></a>Step 1: Building the API Endpoint</span>

Open **routes > videos.js** and add this new route:

<pre><code class="hljs php">router.delete(&lt;span class="hljs-string">'/:id'&lt;/span>, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(req, res)&lt;/span>&lt;/span>{
    &lt;span class="hljs-keyword">var&lt;/span> collection = db.get(&lt;span class="hljs-string">'videos'&lt;/span>);
    collection.remove({ _id: req.params.id }, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(err, video)&lt;/span>&lt;/span>{
        &lt;span class="hljs-keyword">if&lt;/span> (err) &lt;span class="hljs-keyword">throw&lt;/span> err;

        res.json(video);
    });
});
</code></pre>

<p style="text-align: justify;">
  Everything should be familiar here. The only difference is that we’re using <b>router.delete </b>to register a route handler for an HTTP DELETE request.
</p>

<p style="text-align: justify;">
  Also, note that we’re using the <b>remove</b> method of the video collection object. The first argument, as you might guess, is the criteria object.
</p>

### <span id="Step_2_Building_the_Delete_Page"><a id="10_2"></a>Step 2: Building the Delete Page</span>

Go to **partials > home.html** and add a Delete link in front of each video as follows:

<pre class="lang:xhtml decode:true ">&lt;li ng-repeat='video in videos'&gt;
        &lt;a href="/#/video/{{video._id}}"&gt;
            {{video.title}}
        &lt;/a&gt;
        &lt;a href="/#/video/delete/{{video._id}}"&gt;
            &lt;i class="glyphicon glyphicon-remove"&gt;&lt;/i&gt;
        &lt;/a&gt;
    &lt;/li&gt;</pre>

Here, **<i>** is used to render an icon. **glyphicon** and **glyphicon-remove** are both Bootstrap classes for rendering icons. Bootstrap includes tens of modern icons for building applications. To see the list of all icons, go [here](http://getbootstrap.com/components/). To render an icon, you need to use two CSS classes. One is **glyphicon** (the base for all icons), and the other is the class specific to the icon you’re using (**glyphicon-remove**). In the Bootstrap documentation, you can see the CSS class for each icon below the icon itself.

Clicking this icon is going to take us to a page at **/#/video/delete/{{video._id}}**. Let’s create this view and register a route for it.

Add a new file named **video-delete.html** to the **partials** folder. Write the following code in this file:

<pre class="lang:xhtml decode:true ">&lt;h1&gt;Delete Video&lt;/h1&gt;
&lt;p&gt;
    Are you sure you want to delete this video?
&lt;/p&gt;
&lt;ul&gt;
    &lt;li&gt;Title: {{video.title}}&lt;/li&gt;
    &lt;li&gt;Description: {{video.description}}&lt;/li&gt;
&lt;/ul&gt;
&lt;input type="button" value="Yes, Delete" class="btn btn-danger" ng-click="delete()" /&gt;
&lt;a class="btn btn-default" href="/#/"&gt;No, Go Back&lt;/a&gt;
</pre>

Nothing fancy here. I’m using a simple UL and LI to display different attributes of a video. In a real-world application, you may use a more complex markup.

Just note that I’ve added the **btn-danger** class to the Delete button to make it look red, and **btn-default** to the Back button to make it look white.

Once we finish the following steps, the page will look like this:

[<img class="aligncenter size-full wp-image-147342" src="https://blog.udemy.com/wp-content/uploads/2015/09/image101.png" alt="image10" width="1394" height="883" />](https://blog.udemy.com/wp-content/uploads/2015/09/image101.png)

Next, we need a controller for this view. In this controller, we’re going to call our API to get the details of the video upon page load. Also, when the user clicks the Delete button, we’ll call the API to delete the video.

Open **vidzy.js** and create this controller:

<pre><code class="hljs php">app.controller(&lt;span class="hljs-string">'DeleteVideoCtrl'&lt;/span>, [&lt;span class="hljs-string">'$scope'&lt;/span>, &lt;span class="hljs-string">'$resource'&lt;/span>, &lt;span class="hljs-string">'$location'&lt;/span>, &lt;span class="hljs-string">'$routeParams'&lt;/span>,
    &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(&lt;span class="hljs-variable">$scope&lt;/span>, &lt;span class="hljs-variable">$resource&lt;/span>, &lt;span class="hljs-variable">$location&lt;/span>, &lt;span class="hljs-variable">$routeParams&lt;/span>)&lt;/span>&lt;/span>{
        &lt;span class="hljs-keyword">var&lt;/span> Videos = &lt;span class="hljs-variable">$resource&lt;/span>(&lt;span class="hljs-string">'/api/videos/:id'&lt;/span>);

        Videos.get({ id: &lt;span class="hljs-variable">$routeParams&lt;/span>.id }, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(video)&lt;/span>&lt;/span>{
            &lt;span class="hljs-variable">$scope&lt;/span>.video = video;
        })

        &lt;span class="hljs-variable">$scope&lt;/span>.delete = &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">()&lt;/span>&lt;/span>{
            Videos.delete({ id: &lt;span class="hljs-variable">$routeParams&lt;/span>.id }, &lt;span class="hljs-function">&lt;span class="hljs-keyword">function&lt;/span>&lt;span class="hljs-params">(video)&lt;/span>&lt;/span>{
                &lt;span class="hljs-variable">$location&lt;/span>.path(&lt;span class="hljs-string">'/'&lt;/span>);
            });
        }
    }]);
</code></pre>

Again, everything should be familiar at this point.

Finally, we need to create a route to register this view and controller. Add this route to your application configuration code:

<pre class="lang:js decode:true ">.when('/video/delete/:id', {
	templateUrl: 'partials/video-delete.html',
	controller: 'DeleteVideoCtrl'
})</pre>

You’re done. Let’s test the delete functionality. Go back to the browser, and refresh the home page. Click the delete icon. Delete one of the videos. The video will be deleted, and you’ll be redirected to the home page.

## <span id="Wrapping_Up"><a id="11"></a>Wrapping Up</span>

<p style="text-align: justify;">
  If you made it this far, you’ve demonstrated that you’re enthusiastic about learning new things. So, great job. I hope you’ve enjoyed this tutorial and learned the basics of Node, Express, Angular and MongoDB.
</p>

<p style="text-align: justify;">
  Let’s quickly review what you learned in this tutorial:
</p>

<li style="text-align: justify;">
  You successfully built an application with full CRUD functionality using Node, Express, Angular and MongoDB.
</li>
<li style="text-align: justify;">
  You used RoboMongo to create and populate a database with video documents.
</li>
<li style="text-align: justify;">
  You used Monk to add, update, delete and get video documents from Mongo.
</li>
<li style="text-align: justify;">
  You created a RESTful API using Express with support for GET, POST, PUT and DELETE.
</li>
<li style="text-align: justify;">
  You learned about the module system in Node and using the <b>require </b>method.
</li>
<li style="text-align: justify;">
  You learned about Node Package Manager (NPM) and used it to install Node modules.
</li>
<li style="text-align: justify;">
  You used Angular to build a single-page application.
</li>
<li style="text-align: justify;">
  You learned about Angular’s dependency management.
</li>
<li style="text-align: justify;">
  You used Angular’s built-in services, such as <b>$scope, $resource, $location </b>and <b>$routeParams.</b>
</li>
<li style="text-align: justify;">
  You used Angular directives (<b>ng-model</b>, <b>ng-click</b>) to add behavior to DOM elements.
</li>
<li style="text-align: justify;">
  You became familiar with Bootstrap classes to give your forms and buttons a nice and modern look.
</li>

<p style="text-align: justify;">
  What’s next? Node is more than what you’ve read, and that’s why I’m planning to create a comprehensive course to teach you many amazing things you can do with Node. In particular, I’ll be teaching you:
</p>

  * Authentication and authorization
  * Prevention of common security attacks
  * Data validation
  * Caching
  * Real-time applications
  * And more…

If you enjoyed my teaching style and would like to learn more from me, [subscribe to my newsletter](http://programmingwithmosh.com/node-js-course-coming-soon/). I’ll send out an announcement once my course is ready.

<p class="note_normal">
  Published on Codingpedia.org with the permission of<span class="Apple-converted-space"> </span>Udemy<span class="Apple-converted-space"> </span>– source <a title="http://aredko.blogspot.ch/2015/02/a-fresh-look-on-accessing-database-on.html" href="https://blog.udemy.com/node-js-tutorial/" target="_blank">Node.js Tutorial</a> from<span class="Apple-converted-space"> </span><a title="http://aredko.blogspot.com" href="https://blog.udemy.com/" target="_blank">https://blog.udemy.com/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/udemy-logo.png" alt="Udemy blog" /> 
  
  <p id="about_author_header">
    <strong>Udemy</strong>
  </p>
  
  <div id="social_logos_up">
    <a class="icon-earth" href="https://blog.udemy.com/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/udemy" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/udemy" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/+UdemySF" target="_blank"> </a>
  </div>
  
  <div id="author_details" style="text-align: justify;">
    Udemy.com is a platform or marketplace for online learning. Unlike academic MOOC programs driven by traditional collegiate coursework, Udemy provides a platform for experts of any kind to create courses which can be offered to the public, either at no charge or for a tuition fee. Udemy provides tools which enable users to create a course, promote it and earn money from student tuition charges.
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div class="clear">
    </div>
  </div>
</div>
