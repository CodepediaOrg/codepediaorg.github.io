---
id: 2235
title: 'How JSF Works and how to Debug it &#8211; is polyglot an alternative?'
date: 2015-01-21T17:47:27+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=2235
permalink: /jhadesdev/how-jsf-works-and-how-to-debug-it-is-polyglot-an-alternative/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 0
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3441936197
categories:
  - Java EE
tags:
  - ajax
  - chrome dev tools
  - debug
  - debugging
  - enterprise
  - front-end
  - javascript
  - jsf
---
<p style="color: #3a4145; text-align: justify;">
  JSF is not what we often think it is. It&#8217;s also a framework that can be somewhat tricky to debug, specially when first encountered. In this post let&#8217;s go over on why that is and provide some JSF debugging techniques. We will go through the following topics:
</p>

<ul style="color: #3a4145;">
  <li>
    JSF is not what we often think
  </li>
  <li>
    The difficulties of JSF debugging
  </li>
  <li>
    How to debug JSF systematically
  </li>
  <li>
    How JSF Works &#8211; The JSF lifecycle
  </li>
  <li>
    Debugging an Ajax request from browser to server and back
  </li>
  <li>
    Debugging the JSF frontend Javascript code
  </li>
  <li>
    Final thoughts &#8211; alternatives? (questions to the reader)
  </li>
</ul>

<!--more-->

<h3 id="jsfisnotwhatweoftenthink" style="color: #3a4145;">
  JSF is not what we often think
</h3>

<p style="color: #3a4145; text-align: justify;">
  JSF looks on first look like an enterprise Java/XML frontend framework, but under the hood it really <a href="http://blog.primefaces.org/?p=3035">isn&#8217;t</a>. It&#8217;s really a polyglot Java/Javascript framework, where the client Javascript part is non-neglectable and also important to understand it. It also has good support for direct HTML/CSS use.
</p>

<p style="color: #3a4145; text-align: justify;">
  JSF developers are on ocasion already polyglot developers, whose primary language is Java but still need to use ocasionally Javascript.
</p>

<h3 id="thedifficultiesofjsfdebugging" style="color: #3a4145;">
  The difficulties of JSF debugging
</h3>

<p style="color: #3a4145; text-align: justify;">
  When <a href="http://blog.jhades.org/the-java-origins-of-angular-js-angular-vs-jsf-vs-gwt/">comparing JSF to GWT and AngularJS</a> in a previous post, I found that the (most often used) approach that the framework takes of abstracting HTML and CSS from the developer behind XML adds to the difficulty of debugging, because it creates an extra level of indirection.
</p>

<p style="color: #3a4145; text-align: justify;">
  A <a href="http://blog.primefaces.org/?p=3035">more direct approach</a> of using HTML/CSS directly is also possible, but it seems enterprise Java developers tend to stick to XML in most cases, because it&#8217;s a more familiar technology. Also another problem is that the client side Javascript part of the framework/libraries is not very well documented, and it&#8217;s often important to understand what is going on.
</p>

<h3 id="theonlywaytodebugjsfsystematically" style="color: #3a4145;">
  The only way to debug JSF systematically
</h3>

<p style="color: #3a4145; text-align: justify;">
  When first encountering JSF, I first tried to approach it from a Java, XML and documentation only. While I could do a part of the work that way, there where frequent situations where that approach was really not sufficient.
</p>

<p style="color: #3a4145; text-align: justify;">
  The conclusion that I got to is that in order to be able to debug JSF applications effectively, an understanding of the following is needed:
</p>

<ul style="color: #3a4145;">
  <li>
    HTML
  </li>
  <li>
    CSS
  </li>
  <li>
    Javascript
  </li>
  <li>
    HTTP
  </li>
  <li>
    Chrome Dev Tools, Firebug or equivalent
  </li>
  <li>
    The JSF Lifecycle
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  This might sound surprising to developers that work mostly in Java/XML, but this web-centric approach to debugging JSF is the only way that I managed to tackle many requirements that needed some significant component customization, or to be able to fix certain bugs.
</p>

<p style="color: #3a4145;">
  Let’s start by understanding the inner workings of JSF, so that we can debug it better.
</p>

<h3 id="thejsftakeonmvc" style="color: #3a4145;">
  The JSF take on MVC
</h3>

<p style="color: #3a4145;">
  The way JSF approaches <a href="http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller">MVC</a> is that the whole 3 components reside on the server side:
</p>

<ul style="color: #3a4145;">
  <li>
    The Model is a tree of plain Java objects
  </li>
  <li>
    The View is a server side template defined in XML that is read to build an in-memory view definition
  </li>
  <li>
    The Controller is a Java servlet, that receives each request and processes them through a series of steps
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  The browser is assumed to be simply a rendering engine for the HTML generated at server side. Ajax is achieved by submitting parts of the page for server processing, and requesting a server to ‘repaint’ only portions of the screen, without navigating away from the page.
</p>

<h3 id="thejsflifecycle" style="color: #3a4145;">
  The JSF Lifecycle
</h3>

<p style="color: #3a4145; text-align: justify;">
  Once an HTTP request reaches the backend, it gets caught by the JSF Controller that will then process it. The request goes through a series of phases known as the JSF lifecycle, which is essential to understand how JSF works:
</p>

<p style="color: #3a4145;">
  <img src="https://s3-us-west-1.amazonaws.com/blog-jhades/post-debug-jsf-apps/jsfIntro-lifecycle.gif" alt="JSF Hello World Application" />
</p>

<h4 id="designgoalsofthejsflifecycle" style="color: #3a4145;">
  Design Goals of the JSF Lifecycle
</h4>

<p style="color: #3a4145;">
  The whole point of the lifecycle is to manage MVC 100% on the server side, using the browser as a rendering platform only.
</p>

<p style="color: #3a4145; text-align: justify;">
  The initial idea was to decouple the rendering platform from the server-side UI component model, in order to allow to replace HTML with alternative markup languages by swapping the Render Response phase.
</p>

<p style="color: #3a4145; text-align: justify;">
  This was in the early 2000&#8217;s when HTML could be soon replaced by XML-based alternatives (that never came to be), and then HTML5 came along. Also browsers where much more qwirkier than what they are today, and the idea of cross-browser Javascript libraries was not widespread.
</p>

<p style="color: #3a4145; text-align: justify;">
  So let’s go through each phase and see how to debug it if needed, starting in the browser. Let&#8217;s base ourselves in a simple example that uses an Ajax request.
</p>

<h3 id="ajsf2helloworldexample" style="color: #3a4145;">
  A JSF 2 Hello World Example
</h3>

<p style="color: #3a4145; text-align: justify;">
  The following is a minimal JSF 2 page, that receives an input text from the user, sends the text via an Ajax request to the backend and refreshes only an output label:
</p>

<pre class="lang:xhtml decode:true ">&lt;h:body&gt;  
    &lt;h3&gt;JSF 2.2 Hello World Example&lt;/h3&gt;
    &lt;h:form&gt;
        &lt;h:outputtext id="output" value="#{simpleFormBean.inputText}"&gt;&lt;/h:outputtext&gt;  
        &lt;h:inputtext id="input" value="#{simpleFormBean.inputText}"&gt;&lt;/h:inputtext&gt;
        &lt;h:commandbutton value="Submit" action="index"&gt;
            &lt;f:ajax execute="input" render="output"&gt;
        &lt;/f:ajax&gt;&lt;/h:commandbutton&gt;
    &lt;/h:form&gt;
&lt;/h:body&gt;</pre>

<p style="color: #3a4145;">
  The page looks like this:
</p>

<p style="color: #3a4145;">
  <img src="https://s3-us-west-1.amazonaws.com/blog-jhades/post-debug-jsf-apps/hello_world.png" alt="JSF Hello World Application" />
</p>

<h3 id="followingoneajaxrequesttotheserverandback" style="color: #3a4145;">
  Following one Ajax request &#8211; to the server and back
</h3>

<p style="color: #3a4145; text-align: justify;">
  Let’s click submit in order to trigger the Ajax request, and use the Chrome Dev Tools Network tab (right click and inspect any element on the page).What goes over the wire? This is what we see in the Form Data section of the request:
</p>

<pre class="lang:default decode:true ">j_idt8:input: Hello World   
javax.faces.ViewState: -2798727343674530263:954565149304692491   
javax.faces.source: j_idt8:j_idt9
javax.faces.partial.event: click
javax.faces.partial.execute: j_idt8:j_idt9 j_idt8:input
javax.faces.partial.render: j_idt8:output
javax.faces.behavior.event: action
javax.faces.partial.ajax:true</pre>

<p style="color: #3a4145;">
  This request says:
</p>

<blockquote style="color: #3a4145;">
  <p style="font-style: italic;">
    The new value of the input field is &#8220;Hello World&#8221;, send me a new value for the output field only, and don&#8217;t navigate away from this page.
  </p>
</blockquote>

<p style="color: #3a4145; text-align: justify;">
  Let&#8217;s see how this can be read from the request. As we can see, the new values of the form are submitted to the server, namely the “Hello World” value. This is the meaning of the several entries:
</p>

<ul style="color: #3a4145;">
  <li>
    <code>javax.faces.ViewState</code> identifies the view from which the request was made.
  </li>
  <li>
    The request is an Ajax request, as indicated by the flag <code>javax.faces.partial.ajax</code>,
  </li>
  <li>
    The request was triggered by a click as defined in <code>javax.faces.partial.event</code>.
  </li>
</ul>

<p style="color: #3a4145; text-align: justify;">
  But what are those <code>j_</code> strings ? Those are space separated generated identifiers of HTML elements. For example this is how we can see what is the page element corresponding to <code>j_idt8:input</code>, using the Chrome Dev Tools:
</p>

<p style="color: #3a4145;">
  <img src="https://s3-us-west-1.amazonaws.com/blog-jhades/post-debug-jsf-apps/img2_component_ids.png" alt="JSF Hello World Application" />
</p>

<p style="color: #3a4145;">
  There are also 3 extra form parameters that use these identifiers, that are linked to UI components:
</p>

<ul style="color: #3a4145;">
  <li>
    <code>javax.faces.source</code>: The identifier of the HTML element that originated this request, in this case the Id of the submit button.
  </li>
  <li>
    <code>javax.faces.execute</code>: The list of identifiers of the elements whose values are sent to the server for processing, in this case the input text field.
  </li>
  <li>
    <code>javax.faces.render</code>: The list of identifiers of the sections of the page that are to be ‘repainted&#8217;, in this case the output field only.
  </li>
</ul>

<p style="color: #3a4145;">
  But what happens when the request hits the server ?
</p>

<h3 id="jsflifecyclerestoreviewphase" style="color: #3a4145;">
  JSF lifecycle &#8211; Restore View Phase
</h3>

<p style="color: #3a4145; text-align: justify;">
  Once the request reaches the server, the JSF controller will inspect the<br /> <code>javax.faces.ViewState</code> and identify to which view it refers. It will then build or restore a Java representation of the view, that is somehow similar to the document definition in the browser side.
</p>

<p style="color: #3a4145;">
  The view will be attached to the request and used throughout. There is usually little need to debug this phase during application development.
</p>

<h3 id="jsflifecycleapplyrequestvalues" style="color: #3a4145;">
  JSF Lifecycle &#8211; Apply Request Values
</h3>

<p style="color: #3a4145; text-align: justify;">
  The JSF Controller will then apply to the view widgets the new values received via the request. The values might be invalid at this point. Each JSF component gets a call to it’s <code>decode</code> method in this phase.
</p>

<p style="color: #3a4145; text-align: justify;">
  This method will retrieve the submitted value for the widget in question from the HTTP request and store it on the widget itself.
</p>

<p style="color: #3a4145;">
  To debug this, let’s put a breakpoint in the <code>decode</code> method of the<br /> <code>HtmlInputText</code> class, to see the value “Hello World”:
</p>

<p style="color: #3a4145;">
  <img src="https://s3-us-west-1.amazonaws.com/blog-jhades/post-debug-jsf-apps/img4_debug_ARV.png" alt="JSF Hello World Application" />
</p>

<p style="color: #3a4145; text-align: justify;">
  Notice the conditional breakpoint using the HTML <code>clientId</code> of the field we want. This would allow to quickly debug only the decoding of the component we want, even in a large page with many other similar widgets. Next after decoding is the validation phase.
</p>

<h3 id="jsflifecycleprocessvalidations" style="color: #3a4145;">
  JSF Lifecycle &#8211; Process Validations
</h3>

<p style="color: #3a4145; text-align: justify;">
  In this phase, validations are applied and if the value is found to be in error (for example a date is invalid), then the request bypasses Invoke Application and goes directly to Render Response phase.
</p>

<p style="color: #3a4145; text-align: justify;">
  To debug this phase, a similar breakpoint can be put on method<br /> <code>processValidators</code>, or in the validators themselves if you happen to know which ones or if they are custom.
</p>

<h3 id="jsflifecycleupdatemodel" style="color: #3a4145;">
  JSF Lifecycle &#8211; Update Model
</h3>

<p style="color: #3a4145; text-align: justify;">
  In this phase, we know all the submitted values where correct. JSF can now update the view model by applying the new values received in the requests to the plain Java objects in the view model.
</p>

<p style="color: #3a4145; text-align: justify;">
  This phase can be debugged by putting a breakpoint in the<br /> <code>processUpdates</code> method of the component in question, eventually using a similar conditional breakpoint to break only on the component needed.
</p>

<h3 id="jsflifecycleinvokeapplication" style="color: #3a4145;">
  JSF Lifecycle &#8211; Invoke Application
</h3>

<p style="color: #3a4145;">
  This is the simplest phase to debug. The application now has an updated view model, and some logic can be applied on it.
</p>

<p style="color: #3a4145;">
  This is where the action listeners defined in the XML view definition (the &#8216;action&#8217; properties and the listener tags) are executed.
</p>

<h3 id="jsflifecyclerenderresponse" style="color: #3a4145;">
  JSF Lifecycle &#8211; Render Response
</h3>

<p style="color: #3a4145; text-align: justify;">
  This is the phase that I end up debugging the most: why is the value not being displayed as we expect it, etc, it all can be found here. In this phase the view and the new model values will be transformed from Java objects into HTML, CSS and eventually Javascript and sent back over the wire to the browser.
</p>

<p style="color: #3a4145; text-align: justify;">
  This phase can be debugged using breakpoints in the <code>encodeBegin</code>,<br /> <code>encodeChildren</code> and <code>encodeEnd</code> methods of the component in question.
</p>

<p style="color: #3a4145; text-align: justify;">
  The components will either render themselves or delegate rendering to a<code>Renderer</code> class.
</p>

<h3 id="backinthebrowser" style="color: #3a4145;">
  Back in the browser
</h3>

<p style="color: #3a4145;">
  It was a long trip, but we are back where we started! This is how the response generated by JSF looks once received in the browser:
</p>

<pre class="lang:default decode:true ">&lt;!--?xml version='1.0' encoding='UTF-8'?--&gt; 
&lt;partial-response&gt;  
    &lt;changes&gt;
        &lt;update id="j_idt8:output"&gt;&lt;span id="j_idt8:output"&gt;&lt;/span&gt;&lt;/update&gt;
        &lt;update id="javax.faces.ViewState"&gt;-8188482707773604502:6956126859616189525&gt;&lt;/update&gt;
    &lt;/changes&gt;
&lt;/partial-response&gt;</pre>

<p style="color: #3a4145;">
  What the Javascript part of the framework will do is to take the contents of the partial response, update by update.
</p>

<p style="color: #3a4145;">
  Using the Id of the update, the client side JSF callback will search for a component with that Id, delete it from the document and replace it with the new updated version.
</p>

<p style="color: #3a4145;">
  In this case, &#8220;Hello World&#8221; will show up on the label next to the Input text field!
</p>

<p style="color: #3a4145;">
  And so thats how JSF works under the hood. But what about if we need to debug the Javascript part of the framework?
</p>

<h3 id="debuggingthejsfjavascriptcode" style="color: #3a4145;">
  Debugging the JSF Javascript Code
</h3>

<p style="color: #3a4145;">
  The Chrome Dev Tools can help debug the client part. For example let’s say that we want to halt the client when an Ajax request is triggered. We need to go to the sources tab, add an XHR (Ajax) breakpoint and trigger the browser action. The debugger will stop and the call stack can be examined:
</p>

<p style="color: #3a4145;">
  <img src="https://s3-us-west-1.amazonaws.com/blog-jhades/post-debug-jsf-apps/debug_frontend.jpg" alt="JSF Hello World Application" />
</p>

<p style="color: #3a4145; text-align: justify;">
  For some frameworks like Primefaces, the Javascript sources might be minified (non human-readable) because they are optimized for size.
</p>

<p style="color: #3a4145; text-align: justify;">
  To solve this, download the source code of the library and do a non minified build of the jar. There are usually instructions for this, otherwise check the project poms. This will install in your Maven repository a jar with non minified sources for debugging.
</p>

<h3 id="theuidebugtag" style="color: #3a4145;">
  The UI Debug tag:
</h3>

<p style="color: #3a4145;">
  The <code>ui:debug</code> tag allows to view a lot of debugging information using a keyboard shortcut, see <a href="http://www.jsftoolbox.com/documentation/facelets/10-TagReference/facelets-ui-debug.html">here</a> for further details.
</p>

<h3 id="finalthoughts" style="color: #3a4145;">
  Final Thoughts
</h3>

<p style="color: #3a4145; text-align: justify;">
  JSF is very popular in the enterprise Java world, and it handles a lot of problems well, specially if the UI designers take into account the possibilities of the widget library being used.
</p>

<p style="color: #3a4145; text-align: justify;">
  The problem is that there are usually feature requests that force us to dig deeper into the widgets internal implementation in order to customize them, and this requires HTML, CSS, Javascript and HTTP plus JSF lifecycle knowledge.
</p>

<h4 id="ispolyglotanalternative" style="color: #3a4145;">
  Is polyglot an alternative?
</h4>

<p style="color: #3a4145; text-align: justify;">
  We can wonder that if developers have to know a fair amount about web technologies in order to be able to debug JSF effectively, then it would be simpler to build enterprise front ends (just the client part) using those technologies directly instead.
</p>

<p style="color: #3a4145; text-align: justify;">
  It&#8217;s possible that a polyglot approach of a Java backend plus a Javascript-only frontend could be proved effective in a nearby future, specially using some sort of a client side MVC framework like <a href="https://angularjs.org/">Angular</a>.
</p>

<p style="color: #3a4145; text-align: justify;">
  This would require learning more Javascript, (have a look at <a href="http://blog.jhades.org/javascript-for-java-developers/">Javascript for Java developers</a> post if curious), but this is already often necessary to do custom widget development in JSF anyway.
</p>

<h4 id="conclusionsandsomequestionsifyouhavethetime" style="color: #3a4145;">
  Conclusions and some questions if you have the time
</h4>

<p style="color: #3a4145; text-align: justify;">
  Thanks for reading, please take a moment to share your thoughts on these matters on the comments bellow:
</p>

<ul style="color: #3a4145;">
  <li style="text-align: justify;">
    do you believe polyglot development (Java/Javascript) is a viable alternative in general, and in your workplace in particular?
  </li>
  <li style="text-align: justify;">
    Did you find one of the GWT-based frameworks (plain GWT, Vaadin, Errai), or the Play Framework to be easier to use and of better productivity?
  </li>
</ul>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with permission of Aleksey Novik – source <a title="http://blog.jhades.org/how-jsf-works-and-how-to-debug-jsf-applications-effectivelly-is-polyglot-an-alternative/" href="http://blog.jhades.org/how-jsf-works-and-how-to-debug-jsf-applications-effectivelly-is-polyglot-an-alternative/" target="_blank">How JSF Works and how to Debug it</a> – is polyglot an alternative? from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://lh6.googleusercontent.com/-nJLCOBcwQyQ/U3PTSOfhw_I/AAAAAAAAABI/w21JxlhW4lo/s498-no/my-blog-53.jpg" alt="Podcastpedia image" /> 
  
  <p id="about_author_header">
    <strongAleksey Novik</strong>
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Software developer, likes to learn new technologies, hang out on stackoverflow and blog on tips and tricks on Java/Javascript polyglot enterprise development.
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://blog.jhades.org/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/113901291479894108481/posts" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/JhadesDev" target="_blank"> </a> <a class="icon-github" href="https://github.com/jhades" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>

&nbsp;

&nbsp;
