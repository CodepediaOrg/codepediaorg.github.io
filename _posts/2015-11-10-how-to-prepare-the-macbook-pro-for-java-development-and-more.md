---
id: 2521
title: How to prepare the MacBook Pro for Java development and more
date: 2015-11-10T21:23:41+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2521
permalink: /ama/how-to-prepare-the-macbook-pro-for-java-development-and-more/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4307305740
fsb_social_facebook:
  - 9
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 15
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - development
tags:
  - Git
  - GitHub
  - intellij
  - java
  - java development
  - jhipster
  - macbook
  - macbook pro
  - maven
  - nodejs
---
<p style="text-align: justify;">
  Well, I&#8217;ve recently gone to the &#8220;silver&#8221; side and acquired a MacBook Pro to use it for development when I am not at my PC. By development I mean here mainly Java + Javascript development. So I&#8217;ve written this post to remember what I had to install/configure to achieve this goal.
</p>

<p class="note_normal" style="text-align: justify;">
  I need to mention that until now I’ve been a user of Windows (XP/7) and Linux (Ubuntu/Mint/Cent OS) operation systems.
</p>

<p style="text-align: justify;">
  At the time of this writing MacBook Pro runs on OS X Yosemite Version 10.10.5. The new version El Capitan was available, but I didn’t do the upgrade first because it had to many bad reviews&#8230;
</p>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#JDK">JDK</a><ul>
        <li>
          <a href="#Set_JAVA_HOME">Set JAVA_HOME</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Maven">Maven</a>
    </li>
    <li>
      <a href="#GIT">GIT</a>
    </li>
    <li>
      <a href="#IntelliJ">IntelliJ</a>
    </li>
    <li>
      <a href="#Extras">Extras</a><ul>
        <li>
          <a href="#Keyboard_shortcuts">Keyboard shortcuts</a><ul>
            <li>
              <a href="#General">General</a>
            </li>
            <li>
              <a href="#Finder">Finder</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#NodeJS">NodeJS</a>
        </li>
        <li>
          <a href="#MySQL">MySQL</a><ul>
            <li>
              <a href="#Start_stop_server">Start, stop server</a>
            </li>
            <li>
              <a href="#Access_MySQL_from_command_line">Access MySQL from command line</a>
            </li>
            <li>
              <a href="#Install_MySQL_Workbench">Install MySQL Workbench</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#Terminal_window">Terminal window</a><ul>
            <li>
              <a href="#Set_background_black">Set background black</a>
            </li>
            <li>
              <a href="#Jump_to_beginningend_of_a_line">Jump to beginning/end of a line</a>
            </li>
            <li>
              <a href="#Open_terminal_in_here">Open terminal in here</a>
            </li>
            <li>
              <a href="#Use_aliases">Use aliases</a>
            </li>
            <li>
              <a href="#Commands">Commands</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#iTerm2">iTerm2</a>
        </li>
        <li>
          <a href="#Generate_ssh_keys">Generate ssh keys</a>
        </li>
        <li>
          <a href="#Install_Programs_from_Unidentified_Developers">Install Programs from Unidentified Developers</a>
        </li>
        <li>
          <a href="#Often_used_UNIX_keys_on_the_GermanSwiss_keyboard">Often used UNIX keys on the German/Swiss keyboard</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#How_to_test_everything_is_working">How to test everything is working</a>
    </li>
    <li>
      <a href="#References">References</a>
    </li>
  </ul>
</div>

<h2 style="text-align: justify;">
  <span id="JDK">JDK</span>
</h2>

<p style="text-align: justify;">
  So first things first- installe a <b>Java Development Kit</b> (<b>JDK</b>), which is a software development environment used for developing Java applications and applets. It includes the Java Runtime Environment (JRE), an interpreter/loader (java), a compiler (javac), an archiver (jar), a documentation generator (javadoc) and other tools needed in Java development.
</p>

<p style="text-align: justify;">
  Download the Mac OS X x64 <a href="http://www.howtogeek.com/177619/how-to-install-applications-on-a-mac-everything-you-need-to-know/">.dmg</a> files version
</p>

<ul style="text-align: justify;">
  <li>
    <a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html">Java 7</a>
  </li>
  <li>
    <a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html">Java 8</a>
  </li>
</ul>

<p style="text-align: justify;">
  You can find out where the JDK is installed, by executing the <code>/usr/libexec/java_home -v 1.7</code> , on the terminal command:
</p>

<pre class="lang:sh decode:true ">Adrians-MacBook-Pro:ama ama$ /usr/libexec/java_home -v 1.8
/Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home
Adrians-MacBook-Pro:ama ama$ /usr/libexec/java_home -v 1.7
/Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home
Adrians-MacBook-Pro:ama ama$</pre>

<p style="text-align: justify;">
  You will need to know this when setting up a project in IntelliJ for example.
</p>

<h3 style="text-align: justify;">
  <span id="Set_JAVA_HOME">Set JAVA_HOME</span>
</h3>

<p style="text-align: justify;">
  <code>JAVA_HOME</code> is just a convention, usually used by Tomcat, other Java EE app servers and build tools such as <code>Maven</code> to find where Java lives.
</p>

<p style="text-align: justify;">
  In Mac OSX 10.5 or later, Apple recommends to set the <code>$JAVA_HOME</code> variable to <code>/usr/libexec/java_home</code>, just export <code>$JAVA_HOME</code> in file <code>~/.bash_profile</code> or <code>~/.profile</code>
</p>

<pre class="lang:sh decode:true">$ vim .bash_profile

export JAVA_HOME=$(/usr/libexec/java_home)

$ source .bash_profile

$ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home</pre>

<h2 style="text-align: justify;">
  <span id="Maven">Maven</span>
</h2>

<p style="text-align: justify;">
  With the JAVA_HOME environment variable configure, go to the <a href="https://maven.apache.org/download.cgi">Apache Maven Downloads</a> website, download the <em>.tar.gz</em> or <em>.zip</em> archive and unpack it in a folder of your choice &#8211; I put it under the <em>/opt</em> directory:
</p>

<pre class="lang:sh decode:true ">tar xzvf apache-maven-3.3.3-bin.tar.gz</pre>

<p style="text-align: justify;">
  It is also recommended to create a <a href="http://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/">symbolic link</a> to the Maven home, so that when let’s say you update your Maven version,  you’ll only have to change the symbolic link target:
</p>

<pre class="lang:default decode:true">ln -s /opt/apache-maven-3.3.3 /opt/maven</pre>

<p style="text-align: justify;">
  Then set Maven in the environment variables
</p>

<pre class="lang:sh decode:true ">vim ~/.bash_profile</pre>

<pre class="lang:vim decode:true" title="Add these lines to .bash_profile">export M2_HOME=/path/to/maven
export M2=$M2_HOME/bin
export PATH=$M2:$PATH</pre>

<p style="text-align: justify;">
  Close the terminal and open a new one. When you try now to get the maven versioning you should get something like the following:
</p>

<pre class="lang:sh decode:true " title="mvn -version">ama$ mvn -version
Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T13:57:37+02:00)
Maven home: /opt/maven
Java version: 1.8.0_65, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.10.5", arch: "x86_64", family: "mac"</pre>

<p style="text-align: justify;">
  An alternative is to use <a href="http://brew.sh/">Homebrew</a> and execute the following command:
</p>

<pre class="lang:sh decode:true" title="Installation via Homebrew">brew install maven</pre>

<h2 style="text-align: justify;">
  <span id="GIT">GIT</span>
</h2>

<p style="text-align: justify;">
  Open a terminal window and type the following command for example:
</p>

<pre class="lang:default decode:true">$ git --version
</pre>

<p style="text-align: justify;">
  At the next moment you will be asekd to install Xcode. This is the a complete developer toolset for building apps that run on Apple TV, Apple Watch, iPhone, iPad, and Mac. It includes the Xcode IDE, simulators, and all the required tools and frameworks to build apps for iOS, watchOS, tvOS, and OS X (it also contains <a href="https://en.wikipedia.org/wiki/GNU_Compiler_Collection">GNU Compiler Collection-gcc</a>).
</p>

<p style="text-align: justify;">
  You can do the above, but if you do not want everything from that package you can install <a href="http://brew.sh/">Homebrew</a> (<em>&#8220;Homebrew installs <a title="List of Homebrew packages" href="https://github.com/Homebrew/homebrew/tree/master/Library/Formula">the stuff you need</a> that Apple didn’t.&#8221;</em>) and run the following commands:
</p>

<pre class="lang:sh decode:true">brew install gcc
brew install git</pre>

<p style="text-align: justify;">
  Either way once Git is installed the initial command <em>git &#8211;version </em>will bring the installed version:
</p>

<pre class="lang:sh decode:true">$ git --version
git version 2.4.9 (Apple Git-60)</pre>

<p style="text-align: justify;">
  If you are working with Github, I recommend you also install the <a href="https://desktop.github.com/">Github Desktop</a>
</p>

<h2 style="text-align: justify;">
  <span id="IntelliJ">IntelliJ</span>
</h2>

<p style="text-align: justify;">
  In the mean time IntelliJ has become my favorite IDE, mainly because you have almost the same feature support when doing front-end development. To install it, go to the <a href="https://www.jetbrains.com/idea/download/">download page </a>and follow the installation instructions:
</p>

<p style="text-align: justify;">
  <strong>INSTALLATION INSTRUCTIONS</strong>
</p>

<ul class="starlist" style="text-align: justify;">
  <li>
    Download the idea-15.dmg OS X Disk Image file.
  </li>
  <li>
    Mount it as another disk in your system.
  </li>
  <li>
    Copy IntelliJ IDEA to your Applications folder
  </li>
</ul>

Once done you need to get acquainted with key shortcuts for OS X &#8211; [IntelliJ IDEA Mac OS X Keymap](https://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard_Mac.pdf)

<h2 style="text-align: justify;">
  <span id="Extras">Extras</span>
</h2>

<h3 style="text-align: justify;">
  <span id="Keyboard_shortcuts">Keyboard shortcuts</span>
</h3>

#### <span id="General">General</span>

Please visit](https://support.apple.com/en-us/HT201236) for usual keyboard shortcuts (Cut, copy, paste, and other common shortcuts, document shortcuts etc.)

#### <span id="Finder"><a href="https://support.apple.com/en-us/HT201732">Finder</a></span>

  * **Shift + cmd + C** > go to Computer
  * **Shift + cmd + H** > go to Home folder
  * **Shift + cmd + D** > go to Desktop

A quick access with the mouse to the same folders is by dragging and dropping them on the sidebar under **Favorites**

As long as we are by sidebar subject, a good productivity gain can be achieved by using **Smart Folders** &#8211; t<span>hese folders let you save a search to reuse in the future. Smart Folders are updated continuously, so they always find all the files on your computer that match the search criteria. Watch the following video to see how you can easily add them to the sidebar </span>



<span></span>

<h3 style="text-align: justify;">
  <span id="NodeJS">NodeJS</span>
</h3>

<p style="text-align: justify;">
  Node.js® is a JavaScript runtime built on <a href="https://developers.google.com/v8/">Chrome&#8217;s V8 JavaScript engine</a>. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js&#8217; package ecosystem, <a href="https://www.npmjs.com/">npm</a>, is the largest ecosystem of open source libraries in the world. Recently is a must have tool if you need to do fancier stuff on your front-end part of your application.
</p>

<p style="text-align: justify;">
  Go to <a href="https://nodejs.org/en/">https://nodejs.org/</a> and download the latest version for OS X (x64). Double click on the node-v4.2.2.pkg file (latest stable version at the writing of the post) and follow the installation instructions steps.
</p>

<p style="text-align: justify;">
  When ready open a terminal window and check the version installed to see if it is working:
</p>

<pre class="lang:sh decode:true">$ node --version
v4.2.2</pre>

<h3 style="text-align: justify;">
  <span id="MySQL">MySQL</span>
</h3>

<p style="text-align: justify;">
  Go to Downloads page &#8211; <a href="http://dev.mysql.com/downloads/mysql/">http://dev.mysql.com/downloads/mysql/</a>, download the <em>Mac OS X 10.10 (x86, 64-bit), DMG Archive</em> and follow the steps described in the <a href="https://dev.mysql.com/doc/refman/5.7/en/osx-installation-pkg.html">installation guide</a>.
</p>

<h4 style="text-align: justify;">
  <span id="Start_stop_server">Start, stop server</span>
</h4>

<p style="text-align: justify;">
  The MySQL Installation Package includes a MySQL preference pane that enables you to start, stop, and control automated startup during boot of your MySQL installation.
</p>

<p style="text-align: justify;">
  This preference pane is installed by default, and is listed under your system&#8217;s <span class="emphasis"><em>System Preferences</em></span> window, which can be found under <em>Applications</em>.
</p>

<p style="text-align: justify;">
  <img class="alignnone" src="https://dev.mysql.com/doc/refman/5.7/en/images/mac-installer-preference-pane-location.png" alt="" height="746" width="800" />
</p>

<h4 style="text-align: justify;">
  <span id="Access_MySQL_from_command_line">Access MySQL from command line</span>
</h4>

Basically need to add MySQL to the PATH variable. Edit the _/~.bash_profile_ with the following:

<pre class="lang:default decode:true">export MYSQL_HOME=/usr/local/bin/mysql
export PATH=$PATH:$MYSQL_HOME/bin</pre>

To test that it&#8217;s working start a new terminal and verify mysql version from command line:

<pre class="lang:default decode:true">adrians-macbook-pro:~ ama$ mysql --version
mysql  Ver 14.14 Distrib 5.7.9, for osx10.9 (x86_64) using  EditLine wrapper</pre>

<h4 style="text-align: justify;">
  <span id="Install_MySQL_Workbench">Install MySQL Workbench</span>
</h4>

<p style="text-align: justify;">
  If you want to have also a GUI on top of it I recommend you install the MySQL Workbench that can be also found in the <a href="http://dev.mysql.com/downloads/workbench/">downloads section</a>. Installation instruction is the same as the MySQL server installation.
</p>

<h3 style="text-align: justify;">
  <span id="Terminal_window">Terminal window</span>
</h3>

<h4 style="text-align: justify;">
  <span id="Set_background_black">Set background black</span>
</h4>

<p style="text-align: justify;">
  Open Terminal, then go to the <em>Terminal menu -> Preferences</em>, choose the <em>Settings</em> tab and set the <em>Pro</em> theme as the default.
</p>

<h4 style="text-align: justify;">
  <span id="Jump_to_beginningend_of_a_line">Jump to beginning/end of a line</span>
</h4>

To jump at

  * beginning of a line &#8211; _Ctrl+A_
  * end of a line &#8211; _Ctrl+E_
  * jump between words &#8211; _Alt+</>_

#### <span id="Open_terminal_in_here">Open terminal in here</span>

Go to:

<pre class="lang:default decode:true ">System Preferences &gt; Keyboard &gt; Shortcuts &gt; Services</pre>

Enable **New Terminal at Folder**. There&#8217;s also **New Terminal Tab at Folder**, which will create a tab in the frontmost Terminal window (if any, else it will create a new window). These Services work in all applications, not just Finder, and they operate on folders as well as absolute pathnames selected in text.

You can even assign command keys to them.

Services appear in the Services submenu of each application menu, and within the contextual menu (Control-Click or Right-Click on a folder or pathname).

The **New Terminal at Folder** service will become active **when you select a folder** in Finder. You cannot simply have the folder open and run the service &#8220;in place&#8221;. Go back to the parent folder, select the relevant folder, then activate the service via the Services menu or context menu.

In addition, Lion Terminal will open a new terminal window if you drag a folder (or pathname) onto the Terminal application icon, and you can also drag to the tab bar of an existing window to create a new tab.

Finally, if you drag a folder or pathname onto a tab (in the tab bar) and the foreground process is the shell, it will automatically execute a &#8220;cd&#8221; command. (Dragging into the terminal view within the tab merely inserts the pathname on its own, as in older versions of Terminal.)

You can also do this from the command line or a shell script:

    open -a Terminal /path/to/folder


This is the command-line equivalent of dragging a folder/pathname onto the Terminal application icon.

#### <span id="Use_aliases">Use aliases</span>

<p style="text-align: justify;">
  To ease your life for long and usual commands use aliases. For example to connect remote instead of typing <em>ssh ama@x.y.z.q</em>  and having to remember ip address or server name, you could just type <em>rmcon</em> (or whatever it&#8217;s easy for you to remember). To do that append to the <em>.bash_profile</em> in your home directory the alias command and then source the file:
</p>

<pre class="lang:sh decode:true ">vim ~/.bash_profile</pre>

<pre class="lang:vim decode:true">export MYSQL_HOME=/usr/local/bin/mysql
export PATH=$M2:$MYSQL_HOME/bin

alias rmcon='ssh ama@x.y.z.q'</pre>

<pre class="lang:default decode:true">source ~/.bash_profile</pre>

> I can't stress enough, how much comfortable your life can become, if you are using aliases the right way - [A developer's guide to using aliases](http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/)

<iframe width="560" height="315" src="https://www.youtube.com/embed/Emlc7mkZDQ4" frameborder="0" allowfullscreen></iframe>

#### <span id="Commands">Commands</span>

Find out who is listening on port (e.g. 8080)

<pre class="lang:default decode:true">lsof -i | grep 8080</pre>

### <span id="iTerm2">iTerm2</span>

A very nice alternative to the &#8220;classic&#8221; terminal is [iTerm](http://www.iterm2.com/), now in version 2:

<p style="padding-left: 30px; text-align: justify;">
  &#8220;<span>iTerm2 is a replacement for Terminal and the successor to iTerm. It works on Macs with OS 10.5 (Leopard) or newer. iTerm2 brings the terminal into the modern age with features you never knew you always wanted</span>&#8220;
</p>

Look under _Preferences > Keys_ for shortcuts to easily navigate/move the tabs&#8230;

### <span id="Generate_ssh_keys">Generate ssh keys</span>

Open a terminal window and execute the following command:

<pre class="lang:default decode:true ">ssh-keygen -t rsa</pre>

Man pages:

<p class="p1">
  <em><span class="s1"><b>ssh-keygen</b> generates, manages and converts authentication keys for ssh(1).<span class="Apple-converted-space">  </span><b>ssh-keygen</b> can create RSA keys for use by SSH protocol version 1 and DSA, ECDSA, ED25519 or RSA keys for use by SSH </span><span class="s1">protocol version 2.<span class="Apple-converted-space">  </span>The type of key to be generated is specified with the <b>-t</b> option.<span class="Apple-converted-space">  </span>If invoked without any arguments, <b>ssh-keygen</b> will generate an RSA key for use in SSH protocol 2 connec-</span><span class="s1">tions.</span></em>
</p>

You will be asked then where to store the key (default under _/Users/YOUR\_USERNAME/.ssh/id\_rsa_)

<span>When asked for a passphrase you can enter a passphrase to add it to the key. If you choose to add a passphrase<span> every time you want to use your key with ssh, you&#8217;ll have to enter this passphrase. It is a little bit more inconvenient, but more secure. </span></span>

Once that is done, you should get a message like the following:

<pre class="lang:default decode:true">Your identification has been saved in /Users/ama/.ssh/id_rsa.
Your public key has been saved in /Users/ama/.ssh/id_rsa.pub</pre>

You can now use the generated _id_rsa.pub_ key and upload it to the systems you want to connect to over ssh.

### <span id="Install_Programs_from_Unidentified_Developers">Install Programs from Unidentified Developers</span> {.title}

<p style="text-align: justify;">
  By default, Mac OS only allows users to install applications from &#8216;verified sources.&#8217; To change that open the <strong>System Preferences > Security & Privacy > General </strong>and select &#8220;<em>Allow applications downloaded from: Anywhere</em>&#8220;. Follow this <a href="https://kb.wisc.edu/helpdesk/page.php?id=25443">link</a>, to see a more detailed description with pictures.
</p>

<h3 style="text-align: justify;">
  <span id="Often_used_UNIX_keys_on_the_GermanSwiss_keyboard">Often used UNIX keys on the German/Swiss keyboard</span>
</h3>

<p style="text-align: justify;">
  I bought the Mac Book to use it as developer machine on the go and one of my initial surprises was the missing of some keys a developer/terminal user uses pretty often like <em><strong>[]|{}~</strong></em>
</p>

<p style="text-align: justify;">
  Find below a map for these keys:
</p>

<p style="text-align: justify;">
  So here it is, my personal keyboard map reminder for the Mac OS X:
</p>

<div style="padding-bottom:20px">
  <table border="1" cellpadding="1" cellspacing="1">
    <tr>
      <td align="center">
        <b>|</b>
      </td>

      <td>
         pipe symbol
      </td>

      <td>
        <b> alt7</b>
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>\\</b>
      </td>

      <td valign="top">
         backslash
      </td>

      <td valign="top">
        <b> alt shift 7</b> = <b>alt/</b>
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>[</b>
      </td>

      <td valign="top">
         left (opening) square bracket
      </td>

      <td valign="top">
        <b> alt 5</b>
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>]</b>
      </td>

      <td valign="top">
         right (closing) square bracket
      </td>

      <td valign="top">
        <b> alt 6</b>
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>{</b>
      </td>

      <td valign="top">
         left (opening) curly bracket
      </td>

      <td valign="top">
        <b> alt 8</b>
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>}</b>
      </td>

      <td valign="top">
         right (closing) curly bracket
      </td>

      <td valign="top">
        <b> alt 9</b>
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>~</b>
      </td>

      <td valign="top">
         Tilde
      </td>

      <td valign="top">
        <b> alt n</b> followed by the space key
      </td>
    </tr>

    <tr>
      <td align="center" valign="top">
        <b>@</b>
      </td>

      <td valign="top">
         &#8220;At&#8221; symbol
      </td>

      <td valign="top">
        <b> alt g</b> (lowercase G)

        <b>alt L</b> German Keyboard
      </td>
    </tr>
  </table>
</div>

<h3 style="text-align: justify;">
</h3>

<h2 style="text-align: justify;">
  <span id="How_to_test_everything_is_working">How to test everything is working</span>
</h2>

<p style="text-align: justify;">
  A smoke test to verify if everything installed is functioning properly &#8220;together&#8221; is to generate an application with <a href="http://jhipster.github.io/">JHipster</a> and push it to a git repository.
</p>

<p class="note_normal" style="text-align: justify;">
  JHipster is a <a href="http://yeoman.io/" target="_blank">Yeoman generator</a>, used to create a <a href="http://projects.spring.io/spring-boot/" target="_blank">Spring Boot</a> + <a href="https://angularjs.org/" target="_blank">AngularJS</a> project.
</p>

<p style="text-align: justify;">
  For any suggestions please leave a comment. Thank you.
</p>


<h2 style="text-align: justify;">
  <span id="References">References</span>
</h2>

<ol>
  <li style="text-align: justify;">
    <a href="http://superuser.com/questions/100281/linux-mac-os-x-terminal-make-the-background-black-not-white">Linux/Mac OS X Terminal: make the background black, not white</a>
  </li>
  <li style="text-align: justify;">
    <a href="http://www.mkyong.com/java/how-to-set-java_home-environment-variable-on-mac-os-x/">How to Set $JAVA_HOME environment variable on Mac OS X</a>
  </li>
  <li style="text-align: justify;">
    <a href="https://maven.apache.org/install.html">Maven installation guide</a>
  </li>
  <li style="text-align: justify;">
    <a href="https://blogs.oracle.com/blogfinger/entry/mac_os_x_often_used">Mac OS X: often used UNIX keys on the German keyboard</a>
  </li>
  <li style="text-align: justify;">
    <a href="http://stackoverflow.com/questions/81272/is-there-any-way-in-the-os-x-terminal-to-move-the-cursor-word-by-word">Is there any way in the OS X Terminal to move the cursor word by word?</a>
  </li>
  <li style="text-align: justify;">
    <a href="http://stackoverflow.com/questions/420456/open-terminal-here-in-mac-os-finder">Open terminal here in Mac OS finder</a>
  </li>
  <li style="text-align: justify;">
    <a href="http://www.puddingonline.com/~dave/publications/SSH-with-Keys-HOWTO/document/html/SSH-with-Keys-HOWTO-5.html">SSH Keys with a Passphrase</a>
  </li>
  <li style="text-align: justify;">
    <a href="http://coolestguidesontheplanet.com/make-an-alias-in-bash-shell-in-os-x-terminal/">Make an Alias in Bash Shell in OS X Terminal</a>
  </li>
</ol>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/amacoder.png" alt="Podcastpedia image" />

  <p id="about_author_header">
    <strong>Adrian Matei</strong>
  </p>

  <div id="author_details" style="text-align: justify;">
    Creator of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> and <a title="Codingpedia, sharing coding knowledge" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>, computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy - but these are just outside labels and not that important, deep inside we are all just consciousness, right?
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
       <a class="icon-twitter" href="https://twitter.com/codingpedia" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/codingpedia" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codingpediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
