---
id: 2642
title: 'Git Tutorial: A Comprehensive Guide'
date: 2016-02-13T07:07:45+00:00
author: Udemy tutorials
layout: post
guid: http://www.codingpedia.org/?p=2642
permalink: /udemy/git-tutorial-a-comprehensive-guide/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4575290540
fsb_social_facebook:
  - 3
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 6
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - development
tags:
  - beginner
  - cvs
  - distributed version control systems
  - dvcs
  - Git
  - GitHub
  - version control system
---
<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#INTRODUCTION">INTRODUCTION</a>
      </li>
      <li>
        <a href="#Prerequisites">Prerequisites</a>
      </li>
      <li>
        <a href="#Version_Control">Version Control</a><ul>
          <li>
            <a href="#Local_Version_Control">Local Version Control</a>
          </li>
          <li>
            <a href="#Centralized_Version_Control">Centralized Version Control</a>
          </li>
          <li>
            <a href="#Distributed_Version_Control">Distributed Version Control</a>
          </li>
          <li>
            <a href="#So_what_is_Git">So, what is Git?</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#History_of_Git">History of Git</a>
      </li>
      <li>
        <a href="#Getting_Started">Getting Started</a><ul>
          <li>
            <a href="#Download_Git">Download Git</a>
          </li>
          <li>
            <a href="#Mac_OS_X">Mac OS X</a>
          </li>
          <li>
            <a href="#Windows">Windows</a>
          </li>
          <li>
            <a href="#Linux">Linux</a>
          </li>
          <li>
            <a href="#Graphical_Clients">Graphical Clients</a>
          </li>
          <li>
            <a href="#Initial_Customization">Initial Customization</a>
          </li>
          <li>
            <a href="#Default_Text_Editor">Default Text Editor</a>
          </li>
          <li>
            <a href="#Check_Settings">Check Settings</a>
          </li>
          <li>
            <a href="#Getting_Help">Getting Help</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Setting_up_a_Git_Repository">Setting up a Git Repository</a><ul>
          <li>
            <a href="#Importing">Importing</a>
          </li>
          <li>
            <a href="#Cloning">Cloning</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Workflow">Workflow</a><ul>
          <li>
            <a href="#Local_Repository_Structure">Local Repository Structure</a>
          </li>
          <li>
            <a href="#Saving_Changes">Saving Changes</a>
          </li>
          <li>
            <a href="#The_Life_Cycle_of_File_Status">The Life Cycle of File Status</a>
          </li>
          <li>
            <a href="#Check_File_Status">Check File Status</a>
          </li>
          <li>
            <a href="#Tracking_and_Staging_a_New_File">Tracking and Staging a New File</a>
          </li>
          <li>
            <a href="#Committing_a_File">Committing a File</a>
          </li>
          <li>
            <a href="#Committing_All_Changes">Committing All Changes</a>
          </li>
          <li>
            <a href="#Pushing_Changes">Pushing Changes</a>
          </li>
          <li>
            <a href="#Stashing_Changes">Stashing Changes</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Remote_Repositories">Remote Repositories</a><ul>
          <li>
            <a href="#Cloned_Repositories">Cloned Repositories</a>
          </li>
          <li>
            <a href="#Seeing_Your_Remotes">Seeing Your Remotes</a>
          </li>
          <li>
            <a href="#Adding_Remotes">Adding Remotes</a>
          </li>
          <li>
            <a href="#Fetching">Fetching</a>
          </li>
          <li>
            <a href="#Pushing">Pushing</a>
          </li>
          <li>
            <a href="#RenamingRemoving_Remotes">Renaming/Removing Remotes</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Undoing_Actions">Undoing Actions</a><ul>
          <li>
            <a href="#Commit_Mistakes">Commit Mistakes</a>
          </li>
          <li>
            <a href="#Unstaging_a_File">Unstaging a File</a>
          </li>
          <li>
            <a href="#Undo_a_Committed_Snapshot">Undo a Committed Snapshot</a>
          </li>
          <li>
            <a href="#Unmodifying_a_File">Unmodifying a File</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Branching">Branching</a><ul>
          <li>
            <a href="#Creating_a_New_Branch">Creating a New Branch</a>
          </li>
          <li>
            <a href="#Switching_Branches">Switching Branches</a>
          </li>
          <li>
            <a href="#Create_a_Branch_and_Checkout">Create a Branch and Checkout</a>
          </li>
          <li>
            <a href="#List_Branches">List Branches</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Merging">Merging</a><ul>
          <li>
            <a href="#Fast-Forward_Merging">Fast-Forward Merging</a>
          </li>
          <li>
            <a href="#No_Fast-forward">No Fast-forward</a>
          </li>
          <li>
            <a href="#Three-way_Merge">Three-way Merge</a>
          </li>
          <li>
            <a href="#Fetch_and_Merge">Fetch and Merge</a>
          </li>
          <li>
            <a href="#Deleting_Branches">Deleting Branches</a>
          </li>
          <li>
            <a href="#Merge_Conflicts">Merge Conflicts</a>
          </li>
          <li>
            <a href="#Preview_Changes">Preview Changes</a>
          </li>
          <li>
            <a href="#Listing_Branches">Listing Branches</a>
          </li>
          <li>
            <a href="#See_Latest_Commits">See Latest Commits</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Rebasing">Rebasing</a><ul>
          <li>
            <a href="#The_Basics">The Basics</a>
          </li>
          <li>
            <a href="#Rebase_vs_Merge">Rebase vs. Merge</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Log">Log</a>
      </li>
      <li>
        <a href="#Tagging">Tagging</a><ul>
          <li>
            <a href="#Create_a_Tag">Create a Tag</a>
          </li>
          <li>
            <a href="#Tag_Sharing">Tag Sharing</a>
          </li>
          <li>
            <a href="#Tag_Checkout">Tag Checkout</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Aliases">Aliases</a>
      </li>
      <li>
        <a href="#Ignoring_Files">Ignoring Files</a>
      </li>
      <li>
        <a href="#Distributed_Workflows">Distributed Workflows</a><ul>
          <li>
            <a href="#Centralized_Workflow">Centralized Workflow</a>
          </li>
          <li>
            <a href="#Integration-Manager_Workflow">Integration-Manager Workflow</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#GitHub">GitHub</a><ul>
          <li>
            <a href="#Getting_Started-2">Getting Started</a>
          </li>
          <li>
            <a href="#Contributing_to_Projects">Contributing to Projects</a>
          </li>
          <li>
            <a href="#General_Workflow">General Workflow</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Additional_Resources">Additional Resources</a>
      </li>
    </ul>
  </div>
</p>

<!--more-->

<h2 style="text-align: justify;">
  <span id="INTRODUCTION"><span>INTRODUCTION</span></span>
</h2>

<h2 style="text-align: justify;">
  <span id="Prerequisites"><span><a id="1"></a>Prerequisites</span></span>
</h2>

<p style="text-align: justify;">
  Before beginning this tutorial, it is highly recommended that you have a solid understanding of the Terminal (for Mac) or Command Line (for Windows and Linux).
</p>

<p style="text-align: justify;">
  In order to explain Git, we have to first explain various aspects of Version Control.
</p>

<h2 style="text-align: justify;">
  <span id="Version_Control"><span><a id="2"></a>Version Control</span></span>
</h2>

<p style="text-align: justify;">
  Version Control is a system that allows you to revisit various versions of a file or set of files by recording changes. Through version control, one can revert a file or project to a previous version, track modifications and modifying individuals, and compare changes. By utilizing a Version Control System (VCS), mistakes with files can easily be rectified.
</p>

<h3 style="text-align: justify;">
  <span id="Local_Version_Control"><span><a id="2_1"></a>Local Version Control</span></span>
</h3>

<p style="text-align: justify;">
  Many years ago, programmers created Local Version Control systems. A Local VCS entails one database on your hard disk that stores changes to files.
</p>

<h3 style="text-align: justify;">
  <span id="Centralized_Version_Control"><span><a id="2_2"></a>Centralized Version Control</span></span>
</h3>

<p style="text-align: justify;">
  The need for collaboration within a developer team on a single file or set of files led to the advent of the Centralized Version Control System (CVCS). This system entails a single server storing all changes and file versions, which can be accessed by various clients. This streamlined the collaboration process (by eliminating the need to involve all local databases), allowed programmers to have more knowledge of team members’ activities with certain files, and gave administrators much more control over divvying up revision privileges.
</p>

<h3 style="text-align: justify;">
  <span id="Distributed_Version_Control"><span><a id="2_3"></a>Distributed Version Control</span></span>
</h3>

<p style="text-align: justify;">
  A Distributed Version Control systems (DVCS) addresses the major vulnerability of the CVS: the server as a single point of failure. If a CVS goes down, collaborators cannot work with each other on a file or save changes and new versions. Also, in the event of corruption of a central database’s hard disk — with the absence of backups — all work will be lost, except for any portions on local machines.
</p>

<p style="text-align: justify;">
  To prevent this type of catastrophic loss, a DVCS allows clients to create mirrored repositories. These data backups can be easily be placed on the server to replace any lost information.
</p>

<p style="text-align: justify;">
  Because the DVCS allows for multiple mirrored repositories, programmers working in teams can collaborate with each other in various ways to complete a joint project, which enables the use of various simultaneous workflows.
</p>

<h3 style="text-align: justify;">
  <span id="So_what_is_Git"><span><a id="2_4"></a>So, what is Git?</span></span>
</h3>

<p style="text-align: justify;">
  <em><strong>Snapshots</strong></em>
</p>

<p style="text-align: justify;">
  Git is a DVCS that stores data in a file system made up of snapshots. Each time you save a changed version of your project — called commit — Git creates a snapshot of the file and stores a reference to it. If the file has not changed, Git only stores a reference to the already-stored identical version of it.
</p>

<p style="text-align: justify;">
  <em><strong>Local Operations</strong></em>
</p>

<p style="text-align: justify;">
  Git mostly relies on local operations because most necessary information can be found in local resources. This allows for process expediency because a project’s history resides on the local disk, eliminating the need to fetch history information from the server, and allowing one to continue work on a project even when not online or on a VPN.
</p>

<p style="text-align: justify;">
  <em><strong>Tracking Changes</strong></em>
</p>

<p style="text-align: justify;">
  Every single change applied to any file or directory is tracked by Git. And, as the gatekeeper, Git will always detect file corruption or loss of information in transit.
</p>

<p style="text-align: justify;">
  <em><strong>Loss of Data</strong></em>
</p>

<p style="text-align: justify;">
  Git is set up to greatly minimize the possibility of irreversible damage to files, such as accidentally lost data. Git makes it extremely difficult for a snapshot of your file that is committed to be lost.
</p>

<p style="text-align: justify;">
  <em><strong>States</strong></em>
</p>

<p style="text-align: justify;">
  Files in Git can reside in three main states: committed, modified and staged.
</p>

<p style="text-align: justify;">
  <span><strong><em>Committed</em></strong></span><br /> Data is securely stored in a local database
</p>

<p style="text-align: justify;">
  <span><em><strong>Modified</strong></em></span><br /> File has been changed but not committed to the database
</p>

<p style="text-align: justify;">
  <span><em><strong>Staged</strong></em></span><br /> Flagged a file’s changed version to be committed in the next snapshot
</p>

<p style="text-align: justify;">
  <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image066.png"><img class="aligncenter size-full wp-image-146961" src="https://blog.udemy.com/wp-content/uploads/2015/08/image066.png" alt="image06" width="776" height="624" /></a>
</p>

<h2 style="text-align: justify;">
  <span id="History_of_Git"><span><a id="3"></a>History of Git</span></span>
</h2>

<p style="text-align: justify;">
  Git traces its roots to the open source software project Linux kernel. Developers of this project began using a DVCS called BitKeeper in 2002. In 2005, many of these developers stopped using this DVCS due to tension between the Linux kernel community and the company behind BitKeeper’s and the eventual revocation of the DVCS’ gratis status. Subsequently, Linus Torvalds, the chief architect of the Linux kernel, began creating Git. With the intention of creating a DVCS with a workflow design similar to that of BitKeeper, which was also fast, Git allowed for non-linear development via multiple branches, could support large projects, possessed strong mechanisms preventing corruption, and had a simple design. Since its inception in 2005, Git has become one of the most utilized Version Control Systems in the world.
</p>

<h2 style="text-align: justify;">
  <span id="Getting_Started"><span><a id="4"></a>Getting Started</span></span>
</h2>

<h3 style="text-align: justify;">
  <span id="Download_Git"><span><a id="4_1"></a>Download Git</span></span>
</h3>

<p style="text-align: justify;">
  In order to use Git, your computer must have it available. If you already have Git on your computer, you should make sure you have the latest version.<br /> Git can be installed in three ways:
</p>

<ol style="text-align: justify;">
  <li>
    Install as a package
  </li>
  <li>
    Install via another installer
  </li>
  <li>
    Download and compile the source code.
  </li>
</ol>

<h3 style="text-align: justify;">
  <span id="Mac_OS_X"><span><strong>Mac OS X</strong></span></span>
</h3>

<p style="text-align: justify;">
  <em><strong>Terminal</strong></em>
</p>

<p style="text-align: justify;">
  The simplest method for installing Git on a Mac (for Mavericks 10.9 and above) is running Git from the Terminal. If Git is not installed, you will see a prompt for installation.
</p>

<p style="text-align: justify;">
  <em><strong>Git Website</strong></em>
</p>

<p style="text-align: justify;">
  You can also download Git by visiting this link and following the posted directions:<br /> <span><a href="http://git-scm.com/download/mac">http://git-scm.com/download/mac</a></span>
</p>

<p style="text-align: justify;">
  <strong><em>GitHub</em></strong>
</p>

<p style="text-align: justify;">
  A third option is to install Git as part of the GitHub for Mac install. GitHub is repository hosting service, which we will discuss in a future section.<br /> Download GitHub for Mac via the following link:<br /> <span><a href="http://mac.github.com/">http://mac.github.com</a></span>
</p>

<h3 style="text-align: justify;">
  <span id="Windows"><span><strong>Windows</strong></span></span>
</h3>

<p style="text-align: justify;">
  <em><strong>Git Website</strong></em>
</p>

<p style="text-align: justify;">
  You can download Git by visiting this link and following the posted directions:<br /> <span><a href="http://git-scm.com/download/win">http://git-scm.com/download/win</a></span>
</p>

<p style="text-align: justify;">
  <strong><em>GitHub</em></strong>
</p>

<p style="text-align: justify;">
  Install Git as part of the GitHub for Windows install.<br /> <span><a href="http://windows.github.com/">http://windows.github.com</a></span>
</p>

<h3 style="text-align: justify;">
  <span id="Linux"><span>Linux</span></span>
</h3>

<p style="text-align: justify;">
  <strong><em>Package Manager</em></strong>
</p>

<p style="text-align: justify;">
  You can try installing Git via your distribution’s inherent package management tool.
</p>

<p style="text-align: justify;">
  For Fedora:
</p>

<pre class="lang:default decode:true">$ sudo yum install git</pre>

<p style="text-align: justify;">
  For Ubuntu:
</p>

<pre class="lang:default decode:true">$ sudo apt-get install git</pre>

<p style="text-align: justify;">
  <strong><em>Git Website</em></strong>
</p>

<p style="text-align: justify;">
  To download Git for Linux, visit this link and follow the posted directions:<br /> <span><a href="http://git-scm.com/download/linux">http://git-scm.com/download/linux</a></span>
</p>

<h3 style="text-align: justify;">
  <span id="Graphical_Clients"><span><a id="4_2"></a>Graphical Clients</span></span>
</h3>

<p style="text-align: justify;">
  Git includes inherent Graphical User Interface (GUI) tools. However, users can also utilize third-party tools created for particular platforms.
</p>

<p style="text-align: justify;">
  GUI Clients<br /> You can access a variety of GUI clients for Mac, Windows, and Linux via the following link:
</p>

<p style="text-align: justify;">
  <span><a href="https://git-scm.com/downloads/guis">https://git-scm.com/downloads/guis</a></span>
</p>

<h3 style="text-align: justify;">
  <span id="Initial_Customization"><span><a id="4_3"></a>Initial Customization</span></span>
</h3>

<p style="text-align: justify;">
  After making sure Git has been installed, you should perform some customization steps, which should only need to be completed once on any machine. To change settings, you can repeat these steps.
</p>

<p style="text-align: justify;">
  <em><strong>Configuration of Variables</strong> </em><br /> An inherent Git tool called <code>git config</code> allows the setting of configuration variables that control aspects of Git’s operation and look.
</p>

<p style="text-align: justify;">
  <em><strong>Identity Setting</strong></em><br /> After installing Git, users should immediately set the user name and email address, which will be used for every Git commit.
</p>

<p style="text-align: justify;">
  Type the following into Terminal or Command Line:
</p>

<pre class="lang:default decode:true">git config --global user.name "Jane Smith"
git config --global user.email "example@email.com"</pre>

<p style="text-align: justify;">
  To confirm that you have the correct settings, enter the following command:
</p>

<pre class="lang:default decode:true">git config --global user.name (should return Jane Smith)
git config --global user.email (should return example@email.com)</pre>

<p style="text-align: justify;">
  *By using the –global option, these Git settings apply to anything on the system. To use different identity settings for a specific project, change the working directory to the desired local Git repository and repeat the steps above without using –global.
</p>

<h3 style="text-align: justify;">
  <span id="Default_Text_Editor"><span><a id="4_4"></a>Default Text Editor</span></span>
</h3>

<p style="text-align: justify;">
  Without configuration of a default text editor, Git will use the system’s default editor–most likely Vim. To configure a different text editor, such as Emacs, type the following into your Terminal or Command Line:
</p>

<p style="text-align: justify;">
  <code>&lt;span>$&lt;/span> git config --global core.editor emacs</code>
</p>

<p style="text-align: justify;">
  Note: For some editors, you may need to find specific instructions for default configuration.
</p>

<h3 style="text-align: justify;">
  <span id="Check_Settings"><span><a id="4_5"></a>Check Settings</span></span>
</h3>

<p style="text-align: justify;">
  To check settings, use the <code>git config --list</code> command.
</p>

<p style="text-align: justify;">
  Example:
</p>

<pre class="lang:default decode:true">$ git config --list
user.name=Jane Smith
user.email=example@email.com
color.status=auto
color.branch=auto
color.interactive=auto
...</pre>

<h3 style="text-align: justify;">
  <span id="Getting_Help"><span><a id="4_6"></a>Getting Help</span></span>
</h3>

<p style="text-align: justify;">
  There are three ways to get more information on a particular command, by accessing the manual:
</p>

<pre class="lang:default decode:true">git help command
git command --help
man git-command</pre>

<h2 style="text-align: justify;">
  <span id="Setting_up_a_Git_Repository"><span><a id="5"></a>Setting up a Git Repository</span></span>
</h2>

<h3 style="text-align: justify;">
  <span id="Importing"><span><a id="5_1"></a>Importing</span></span>
</h3>

<p style="text-align: justify;">
  To import an existing project or directory into Git, follow these steps using the Terminal or Command Line:
</p>

<ol style="text-align: justify;">
  <li>
    Switch to the target project’s directory<br /> Example:</p> <pre class="lang:default decode:true">$ cd test (cd = change directory)</pre>
  </li>
</ol>

<ol start="2" style="text-align: justify;">
  <li>
    Use the git init command <pre class="lang:default decode:true">$ git init</pre>

    <p>
      <strong>Note:</strong> At this stage, you have created a new subdirectory named .git that has the repository files. Tracking has not commenced.</li>

      <li>
        To start tracking these repository files, perform an initial commit by typing the following: <pre class="lang:default decode:true">$ git add *.c
$ git add LICENSE
$ git commit -m “any message here”</pre>
      </li></ol>

      <p style="text-align: justify;">
        Now, your files are tracked and there’s an initial commit. We will discuss the particular commands in detail soon.
      </p>

      <h3 style="text-align: justify;">
        <span id="Cloning"><span><a id="5_2"></a>Cloning</span></span>
      </h3>

      <p style="text-align: justify;">
        You can also create a copy of an existing Git repository from a particular server by using the clone command with a repository’s URL:
      </p>

      <pre class="lang:default decode:true">$ git clone https://github.com/test</pre>

      <p style="text-align: justify;">
        By cloning the file, you have copied all versions of all files for a project. This command leads to the creation of a directory called “test,” with an initialized .git directory inside it, which has copies of all versions of all files for the specified project. The command also automatically checks out — or retrieves for editing — a copy of the newest version of the project.
      </p>

      <p style="text-align: justify;">
        To clone a repository into a directory with another name of your choosing, use the following command format:
      </p>

      <pre class="lang:default decode:true">$ git clone https://github.com/test mydirectory</pre>

      <p style="text-align: justify;">
        The command above makes a copy of the target repository in a directory named “mydirectory.”
      </p>

      <h2 style="text-align: justify;">
        <span id="Workflow"><span><a id="6"></a>Workflow</span></span>
      </h2>

      <h3 style="text-align: justify;">
        <span id="Local_Repository_Structure"><span><a id="6_1"></a>Local Repository Structure</span></span>
      </h3>

      <p style="text-align: justify;">
        The local Git repository has three components:
      </p>

      <ol style="text-align: justify;">
        <li>
          Working Directory: The actual files reside here.
        </li>
        <li>
          Index: The area used for staging
        </li>
        <li>
          Head: Points to the most recent commit
        </li>
      </ol>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image036.png"><img class="aligncenter size-full wp-image-146963" src="https://blog.udemy.com/wp-content/uploads/2015/08/image036.png" alt="image03" width="918" height="272" /></a>
      </p>

      <h3 style="text-align: justify;">
        <span id="Saving_Changes"><span><a id="6_2"></a>Saving Changes</span></span>
      </h3>

      <p style="text-align: justify;">
        All files in a checked out (or working) copy of a project file are either in a tracked or untracked state.
      </p>

      <p style="text-align: justify;">
        <em><strong>Tracked</strong> </em><br /> Tracked files can be modified, unmodified, or staged; they were part of the most recent file snapshot.
      </p>

      <p style="text-align: justify;">
        <em><strong>Untracked</strong></em><br /> Untracked files were not in the last snapshot and do not currently reside in the staging area.
      </p>

      <p style="text-align: justify;">
        *After cloning a repository, files have tracked status and are unmodified because they have been checked out but not edited.
      </p>

      <h3 style="text-align: justify;">
        <span id="The_Life_Cycle_of_File_Status"><span><a id="6_3"></a>The Life Cycle of File Status</span></span>
      </h3>

      <ol style="text-align: justify;">
        <li>
          After you edit a file, Git flags it as modified because of changes made after the previous commit.
        </li>
        <li>
          You stage the modified file.
        </li>
        <li>
          Then, you commit staged changes.
        </li>
      </ol>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image006.png"><img class="aligncenter size-full wp-image-146964" src="https://blog.udemy.com/wp-content/uploads/2015/08/image006.png" alt="image00" width="998" height="502" /></a>
      </p>

      <h3 style="text-align: justify;">
        <span id="Check_File_Status"><span><a id="6_4"></a>Check File Status</span></span>
      </h3>

      <p style="text-align: justify;">
        To determine the state of files, utilize the <code>git status</code> command:
      </p>

      <pre class="lang:default decode:true">$ git status</pre>

      <p style="text-align: justify;">
        On branch master<br /> nothing to commit, working directory clean
      </p>

      <p style="text-align: justify;">
        *This information indicates which branch you’re on (we will cover branches in a later section) and states “working directory clean,” which means that files have tracked or modified status at the moment. Also, no untracked files are present because Git has not listed any.
      </p>

      <h3 style="text-align: justify;">
        <span id="Tracking_and_Staging_a_New_File"><span><a id="6_5"></a>Tracking and Staging a New File</span></span>
      </h3>

      <p style="text-align: justify;">
        <em><strong>Single File</strong></em><br /> Track one file only by using the following format:
      </p>

      <pre class="lang:default decode:true">git add filename</pre>

      <p style="text-align: justify;">
        <em><strong>All Files</strong></em><br /> Track all files in a repository by using the following command:
      </p>

      <pre class="lang:default decode:true">$ git add *</pre>

      <p style="text-align: justify;">
        *After using these commands, files are tracked and staged for committing.
      </p>

      <p style="text-align: justify;">
        After adding a new file called EXAMPLE, you would see information regarding changes to be committed when using the <code>git status</code> command:
      </p>

      <pre class="lang:default decode:true">$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD ..." to unstage)</pre>

      <pre class="lang:default decode:true">new file: EXAMPLE</pre>

      <p style="text-align: justify;">
        This information tells us that there are changes to be committed and that the file has been staged.
      </p>

      <h3 style="text-align: justify;">
        <span id="Committing_a_File"><span><a id="6_6"></a>Committing a File</span></span>
      </h3>

      <p style="text-align: justify;">
        After staging one or multiple files, you should commit the changes and record what you did within the commit message:
      </p>

      <pre class="lang:default decode:true">$ git commit -m “made change x,y,z”</pre>

      <p style="text-align: justify;">
        *This step has committed changes for the file or files (you can have one commit message for multiple files, if applicable) to the HEAD.
      </p>

      <h3 style="text-align: justify;">
        <span id="Committing_All_Changes"><span><a id="6_7"></a>Committing All Changes</span></span>
      </h3>

      <pre class="lang:default decode:true">$ git commit -a</pre>

      <p style="text-align: justify;">
        *This command commits a snapshot of all modifications to tracked files in the working directory.
      </p>

      <h3 style="text-align: justify;">
        <span id="Pushing_Changes"><span><a id="6_8"></a>Pushing Changes</span></span>
      </h3>

      <p style="text-align: justify;">
        Next, you would push changes to a remote repository. We will discuss remote repositories in more depth in the next section. For now, we will look at a general overview of pushing changes to remotes.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git push origin master</pre>

      <p style="text-align: justify;">
        *This command pushes changes from the local “master” branch to the remote repository named “origin”.
      </p>

      <p style="text-align: justify;">
        *For cloned repositories, Git will automatically give the name “origin” to the server from which you cloned and the name “master” to your local repository. However, these names can be changed by the user.
      </p>

      <h3 style="text-align: justify;">
        <span id="Stashing_Changes"><span><a id="6_9"></a>Stashing Changes</span></span>
      </h3>

      <p style="text-align: justify;">
        When you are not ready to commit changes but do not want to lose them either,<code>git stash</code> is a great option. This command temporarily removes changes and hides them, giving you a clean working directory. When you are ready to continue working on the changes, simply use the <code>git stash apply</code> command to retrieve the hidden changes.
      </p>

      <h2 style="text-align: justify;">
        <span id="Remote_Repositories"><span><a id="7"></a>Remote Repositories</span></span>
      </h2>

      <p style="text-align: justify;">
        In order to collaborate on Git projects, you must interact with remote repositories, versions of a project residing online or on a network. You can work with multiple repositories, for which you can have read/write or read-only privileges. Teams can use remote repositories to push information to and pull data from.
      </p>

      <h3 style="text-align: justify;">
        <span id="Cloned_Repositories"><span><a id="7_1"></a>Cloned Repositories</span></span>
      </h3>

      <p style="text-align: justify;">
        As mentioned earlier, for cloned repositories, Git will automatically give the name “origin” to the server from which you cloned and the name “master” to your local branch.
      </p>

      <h3 style="text-align: justify;">
        <span id="Seeing_Your_Remotes"><span><a id="7_2"></a>Seeing Your Remotes</span></span>
      </h3>

      <p style="text-align: justify;">
        By running the <code>git remote</code> command, you can view the short names, such as “origin,” of all specified remote handles.
      </p>

      <p style="text-align: justify;">
        By using <code>git remote -v</code>, you can view all the remote URLs next to their corresponding short names.
      </p>

      <pre class="lang:default decode:true">$ cd example
$ git remote -v
remote1 https://github.com/remote1/example (fetch)
remote1 https://github.com/remote1/example (push)
remote2 https://github.com/remote2/example (fetch)
remote2 https://github.com/remote2/example (push)
remote3 https://github.com/remote3/example (fetch)
remote3 https://github.com/remote3/example (push)</pre>

      <h3 style="text-align: justify;">
        <span id="Adding_Remotes"><span><a id="7_3"></a>Adding Remotes</span></span>
      </h3>

      <p style="text-align: justify;">
        To create a new remote Git repository with a short name, use the following format:
      </p>

      <pre class="lang:default decode:true">git remote add shortname url</pre>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git remote
origin
$ git remote add js https://github.com/janesmith/project1
$ git remote -v
origin https://github.com/johndoe/project1 (fetch)
origin https://github.com/johndoe/project1 (push)
js     https://github.com/janesmith/project1 (fetch)
js     https://github.com/janesmith/project1 (push)</pre>

      <p style="text-align: justify;">
        This addition of these remote and short names allows you to use shortnames for Git collaboration.
      </p>

      <h3 style="text-align: justify;">
        <span id="Fetching"><span><a id="7_4"></a>Fetching</span></span>
      </h3>

      <p style="text-align: justify;">
        Fetching entails pulling data that you don’t have from a remote project.
      </p>

      <p style="text-align: justify;">
        Here is the command format:
      </p>

      <pre class="lang:default decode:true">git fetch [remote-name]</pre>

      <p style="text-align: justify;">
        *Now, you should also possess the references to all branches for that remote (more on branching later).
      </p>

      <p style="text-align: justify;">
        Cloned Repositories<br /> For cloned repositories, use the command <code>git fetch origin</code> to pull down any new changes that were pushed to the server since you cloned or last fetched from it.
      </p>

      <hr />

      <p style="text-align: justify;">
        <strong>Note:</strong> <code>git fetch</code> solely pulls new data to a local repository; it does not merge changes with or modify your local work. We will discuss <code>merging</code> in a later section. Later, we will also discuss <code>git pull</code> , which allows for fetching and automatic merging.
      </p>

      <h3 style="text-align: justify;">
        <span id="Pushing"><span><a id="7_5"></a>Pushing</span></span>
      </h3>

      <p style="text-align: justify;">
        To push your changes “upstream” for sharing, you would use the following <code>git push</code> command format:
      </p>

      <pre class="lang:default decode:true">git push [remote-name][branch-name]</pre>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git push origin master</pre>

      <p style="text-align: justify;">
        *This command pushes committed changes from your local “master” branch upstream to the “origin” server.
      </p>

      <p style="text-align: justify;">
        Note: You can only successfully push changes upstream if you have write access for the server from which you cloned, and if someone else has not pushed changes upstream that you haven’t pulled yet. If a collaborator pushed changes upstream after you had cloned, your push will not be successful. You will have to pull new changes and merge them with your branch before you can successfully push your changes upstream.
      </p>

      <h3 style="text-align: justify;">
        <span id="RenamingRemoving_Remotes"><span><a id="7_6"></a>Renaming/Removing Remotes</span></span>
      </h3>

      <p style="text-align: justify;">
        <em><strong>Rename</strong></em><br /> To rename a remote’s short name, use the <code>git remote rename</code> command.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git remote rename js jane
$ git remote
origin
jane</pre>

      <p style="text-align: justify;">
        *In the example above, we can see that the remote’s short name has been changed from js to Jane. The command <code>git remote</code> lists our existing remotes, which jane is now one of. The rename action also alters names of remote branches: js/master would change to jane/master.
      </p>

      <p style="text-align: justify;">
        <em><strong>Remove</strong></em><br /> To remove a remote for whatever reason (e.g., a contributor has left the team, the server has moved), simply use the git remote rm command:
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git remote rm jane
$ git remote
origin</pre>

      <hr />

      <p style="text-align: justify;">
        <strong>NOTE:</strong> Reminder: “origin” is simply the default remote name when you use the <code>git clone</code> command.
      </p>

      <h2 style="text-align: justify;">
        <span id="Undoing_Actions"><span><a id="8"></a>Undoing Actions</span></span>
      </h2>

      <p style="text-align: justify;">
        Git has mechanisms for undoing certain actions.
      </p>

      <h3 style="text-align: justify;">
        <span id="Commit_Mistakes"><span><a id="8_1"></a>Commit Mistakes</span></span>
      </h3>

      <p style="text-align: justify;">
        You can use the –amend command when you need to alter a commit message or forgot to add some files.
      </p>

      <pre class="lang:default decode:true">$ git commit --amend</pre>

      <p style="text-align: justify;">
        In the example above, you can use this command to easily change your commit message, if no changes were made since the newest commit.
      </p>

      <pre class="lang:default decode:true">$ git commit -m “my first commit”
$ git add example_file
$ git commit --amend</pre>

      <p style="text-align: justify;">
        In the above example, a forgotten file is added to a commit.
      </p>

      <h3 style="text-align: justify;">
        <span id="Unstaging_a_File"><span><a id="8_2"></a>Unstaging a File</span></span>
      </h3>

      <pre class="lang:default decode:true">$ git reset HEAD index.html
Unstaged changes after reset:
M index.html</pre>

      <p style="text-align: justify;">
        Above, we see that the <code>git reset HEAD</code> command unstaged the index.html file.
      </p>

      <hr />

      <p style="text-align: justify;">
        <strong>NOTE:</strong> When <code>git reset --hard</code> is used, Git overwrites all changes in the working directory, permanently destroying any uncommitted changes.
      </p>

      <h3 style="text-align: justify;">
        <span id="Undo_a_Committed_Snapshot"><span><a id="8_3"></a>Undo a Committed Snapshot</span></span>
      </h3>

      <p style="text-align: justify;">
        To undo changes resulting from a particular commit, use the <code>git revert</code> command. This command appends a new commit that undoes changes introduced by a specific commit. This prevents Git from losing history.
      </p>

      <pre class="lang:default decode:true">$ git commit -m "Example Commit"
$ git revert HEAD</pre>

      <p style="text-align: justify;">
        *In the example above, a new commit gets appended and rolls back changes from a specific commit.
      </p>

      <h3 style="text-align: justify;">
        <span id="Unmodifying_a_File"><span><a id="8_4"></a>Unmodifying a File</span></span>
      </h3>

      <p style="text-align: justify;">
        To have a file return to its state when you last committed, utilize the <code>git checkout</code>command.<br /> Example:
      </p>

      <pre class="lang:default decode:true">$ git checkout -- index.html</pre>

      <hr />

      <p style="text-align: justify;">
        <strong>NOTE:</strong> <code>git checkout -- file</code> erases any changes made to the file because you are copying another file over it. Almost all committed information in Git can be recovered; however, any uncommitted information can be lost forever.
      </p>

      <h2 style="text-align: justify;">
        <span id="Branching"><span><a id="9"></a>Branching</span></span>
      </h2>

      <p style="text-align: justify;">
        Almost every type of Version Control System incorporates branching. By creating branches of a central repository, collaborators are able to work on a project simultaneously via multiple branches, without affecting this main repository.
      </p>

      <p style="text-align: justify;">
        A collaborator can create a branch, work on it and save commit snapshots within it, switch between various branches, and merge changes. A Git branch is basically a movable pointer that always points to the most recent commit, or snapshot. Git uses the default local branch name “master,” which can be changed. Just because the default name is “master” does not imply that it is higher in importance or has more functionality than other branches. The head is a special pointer which indicates which branch you are currently working within.
      </p>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image016.png"><img class="aligncenter size-full wp-image-146968" src="https://blog.udemy.com/wp-content/uploads/2015/08/image016.png" alt="image01" width="534" height="372" /></a>
      </p>

      <h3 style="text-align: justify;">
        <span id="Creating_a_New_Branch"><span><a id="9_1"></a>Creating a New Branch</span></span>
      </h3>

      <p style="text-align: justify;">
        To create a new branch, use the git branch name format:
      </p>

      <pre class="lang:default decode:true">$ git branch test</pre>

      <p style="text-align: justify;">
        *The above command creates a new branch named “test” that points toward your most recent commit. However, this command does not switch you over to this new branch.
      </p>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image027.png"><img class="aligncenter size-full wp-image-146969" src="https://blog.udemy.com/wp-content/uploads/2015/08/image027.png" alt="image02" width="792" height="380" /></a>
      </p>

      <h3 style="text-align: justify;">
        <span id="Switching_Branches"><span><a id="9_2"></a>Switching Branches</span></span>
      </h3>

      <p style="text-align: justify;">
        To switch to another branch, use the <code>git checkout</code> command.
      </p>

      <p style="text-align: justify;">
        <code>$ git checkout test</code>
      </p>

      <p style="text-align: justify;">
        *This command moves the HEAD pointer to the test branch
      </p>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image086.png"><img class="aligncenter size-full wp-image-146970" src="https://blog.udemy.com/wp-content/uploads/2015/08/image086.png" alt="image08" width="966" height="398" /></a>
      </p>

      <p style="text-align: justify;">
        If you make a commit while on the test branch, only this branch will point to the most recent commit. The master branch will still point at the commit it was pointing to when you checked out to the test branch. So, if you switch back to the master branch, none of your recent changes made on the test branch will exist on the master branch.
      </p>

      <h3 style="text-align: justify;">
        <span id="Create_a_Branch_and_Checkout"><span><a id="9_3"></a>Create a Branch and Checkout</span></span>
      </h3>

      <p style="text-align: justify;">
        To simultaneously create a new branch and switch to it, use the -b switch with the<code>git checkout</code> command:
      </p>

      <pre class="lang:default decode:true">$ git checkout -b test2</pre>

      <p style="text-align: justify;">
        *The command above creates a new branch “test2” and switches you to it.
      </p>

      <h3 style="text-align: justify;">
        <span id="List_Branches"><span><a id="9_4"></a>List Branches</span></span>
      </h3>

      <p style="text-align: justify;">
        You can list available branches by using the <code>git branch</code> command.
      </p>

      <pre class="lang:default decode:true">$ git branch
*master</pre>

      <p style="text-align: justify;">
        Above, we see that we have one local branch, named “master”.
      </p>

      <h2 style="text-align: justify;">
        <span id="Merging"><span><a id="10"></a>Merging</span></span>
      </h2>

      <p style="text-align: justify;">
        When you want to merge changes from one branch into your current one, you can use the <code>git merge</code> command.
      </p>

      <h3 style="text-align: justify;">
        <span id="Fast-Forward_Merging"><span><a id="10_1"></a>Fast-Forward Merging</span></span>
      </h3>

      <p style="text-align: justify;">
        With a fast-forward merge, your current branch’s pointer moves forward to the most recent commit for the branch being merged in – there is no divergent work to merge together because the latter branch is directly upstream in relation to the former one.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git checkout master (switches you to the master branch)
$ git merge test (merges in changes from test branch)
Updating c58d775 .. 8b9205d
Fast-forward
index.html| 4 ++
1 file changed, 4 insertions(+)</pre>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image054.png"><img class="aligncenter size-full wp-image-146971" src="https://blog.udemy.com/wp-content/uploads/2015/08/image054.png" alt="image05" width="1062" height="520" /></a>
      </p>

      <p style="text-align: justify;">
        In the diagram above, the Master branch, which previously pointed at Commit 3, now points at Commit 4, after fast forwarding and merging in changes from the Test branch – made possible by the fact that Master had not diverged from the latter.
      </p>

      <h3 style="text-align: justify;">
        <span id="No_Fast-forward"><span><a id="10_2"></a>No Fast-forward</span></span>
      </h3>

      <p style="text-align: justify;">
        When you use the <code>git merge --no-ff &lt;branch&gt;</code> command, instead of a branch simply moving its pointer forward, a new commit object is created. The –no-ff flag is often used to prevent the loss of historical information regarding a merged-in branch.
      </p>

      <pre class="lang:default decode:true">$ git merge test --no-ff</pre>

      <h3 style="text-align: justify;">
        <span id="Three-way_Merge"><span><a id="10_3"></a>Three-way Merge</span></span>
      </h3>

      <p style="text-align: justify;">
        In cases of branches diverging, fast-forward merges are not an option. A three-way merge can be used, however. This type of merge involves the two latest commit snapshots pointed to by both branches, and their common ancestor. Here is an example of creating a three-way merge:
      </p>

      <pre class="lang:default decode:true">$ git checkout master
$ git merge test</pre>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image075.png"><img class="aligncenter size-full wp-image-146972" src="https://blog.udemy.com/wp-content/uploads/2015/08/image075.png" alt="image07" width="1090" height="676" /></a>
      </p>

      <p style="text-align: justify;">
        Instead of just moving the branch pointer forward, Git creates a new snapshot that results from this three-way merge and automatically creates a new commit that points to it. This is referred to as a merge commit, which is special because it has more than one parent.
      </p>

      <h3 style="text-align: justify;">
        <span id="Fetch_and_Merge"><span><a id="10_4"></a>Fetch and Merge</span></span>
      </h3>

      <p style="text-align: justify;">
        To easily fetch and merge remote changes, use the <code>git pull</code> command in your working directory.
      </p>

      <h3 style="text-align: justify;">
        <span id="Deleting_Branches"><span><a id="10_5"></a>Deleting Branches</span></span>
      </h3>

      <p style="text-align: justify;">
        After you’ve merged branches, and they’re no longer needed, you can easily delete them with the -d flag.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git branch -d test</pre>

      <p style="text-align: justify;">
        *This deletes the “test” branch.
      </p>

      <h3 style="text-align: justify;">
        <span id="Merge_Conflicts"><span><a id="10_6"></a>Merge Conflicts</span></span>
      </h3>

      <p style="text-align: justify;">
        When the two files being merged both have changes in identical sections of a file, Git will not be able to complete the merge cleanly. These conflicts must be dealt with manually. Portions of files with unresolved merge conflicts will be labeled “unmerged”.
      </p>

      <p style="text-align: justify;">
        You can easily locate areas of merge conflict via Git’s conflict resolution markers, which appear in applicable files.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">My name is
 &lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD
 Jane
 =======
 Mary
 &gt;&gt;&gt;&gt;&gt;&gt;&gt; branch-test</pre>

      <p style="text-align: justify;">
        Here, HEAD indicates the branch you checked out before running the merge command. The content above the ======= resides within this branch, while everything below it is within the test branch. As you can see, the conflict is that Jane and Mary conflict with each other. After fixing this conflict and removing the ======= and >>>>>>> lines, run the git add command on fixed files to mark them as resolved.
      </p>

      <p style="text-align: justify;">
        To view unmerged files, use the<code> git status</code> command.
      </p>

      <pre class="lang:default decode:true">$ git status</pre>

      <p style="text-align: justify;">
        # On branch master<br /> # Unmerged paths:<br /> # (use “git add/rm …” as appropriate to mark resolution)<br /> #<br /> # both modified: index.html<br /> #<br /> *Here, we see that there is a merge conflict in index.html.
      </p>

      <h3 style="text-align: justify;">
        <span id="Preview_Changes"><span><a id="10_7"></a>Preview Changes</span></span>
      </h3>

      <p style="text-align: justify;">
        To preview changes for merging, use the <code>git diff</code> command in the following format:
      </p>

      <pre class="lang:default decode:true">git diff &lt;source_branch&gt; &lt;target_branch&gt;</pre>

      <h3 style="text-align: justify;">
        <span id="Listing_Branches"><span><a id="10_8"></a>Listing Branches</span></span>
      </h3>

      <p style="text-align: justify;">
        To list local branches, use the <code>git branch</code> command. The branch currently being worked on will have an asterisk next to its name.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git branch
*master
test</pre>

      <h3 style="text-align: justify;">
        <span id="See_Latest_Commits"><span><a id="10_9"></a>See Latest Commits</span></span>
      </h3>

      <p style="text-align: justify;">
        To see the newest commits for each branch, use the <code>git branch-v</code> command.
      </p>

      <pre class="lang:default decode:true">$ git branch -v
*master 51g222e updated html file
test 52c667a updated JavaScript file</pre>

      <p style="text-align: justify;">
        The useful <code>--merged</code> and <code>--no-merged</code> options can filter this list to branches that you have or have not yet merged into the branch you’re currently on. To see which branches are already merged into the branch you’re on, you can run <code>git branch --merged</code>:
      </p>

      <pre class="lang:default decode:true">$ git branch --merged
test
 * master</pre>

      <p style="text-align: justify;">
        Because you already merged in test earlier, you see it in your list. Branches on this list without the * in front of them are generally fine to delete with git branch -d; you’ve already incorporated their work into another branch, so you’re not going to lose anything.<br /> To see all the branches that contain work you haven’t yet merged in, you can run<code>git branch --no-merged</code>:
      </p>

      <pre class="lang:default decode:true">$ git branch --no-merged
testing</pre>

      <h2 style="text-align: justify;">
        <span id="Rebasing"><span><a id="11"></a>Rebasing</span></span>
      </h2>

      <p style="text-align: justify;">
        A popular alternative to merging is rebasing.
      </p>

      <h3 style="text-align: justify;">
        <span id="The_Basics"><span><a id="11_1"></a>The Basics</span></span>
      </h3>

      <p style="text-align: justify;">
        Rebasing starts with the common ancestor of two branches: the one you’re on and the one you’re rebasing onto. Then, the diffs resulting from each commit of your current branch are saved to temporary files, and the current branch gets reset to the same commit as that of the branch you are rebasing onto. Last but not least, all changes get applied to the branch you are rebasing onto.<br /> This process essentially rewrites a project’s history by replaying all changes committed on one branch to another one, leading to cleaner application of commits on a remote branch and a linear history.<br /> Example:
      </p>

      <pre class="lang:default decode:true">$ git checkout test
$ git rebase master</pre>

      <p style="text-align: justify;">
        <a href="https://blog.udemy.com/wp-content/uploads/2015/08/image046.png"><img class="aligncenter size-full wp-image-146974" src="https://blog.udemy.com/wp-content/uploads/2015/08/image046.png" alt="image04" width="970" height="586" /></a>
      </p>

      <p style="text-align: justify;">
        In this example, changes committed on commit 4 are replayed onto commit 3, allowing a fast-forward merge of master and the creation of a clean, linear history.
      </p>

      <p style="text-align: justify;">
        Rebase Caveat: Avoid rebasing commits that are not within your repository; this could lead to confusion and inefficiency.
      </p>

      <h3 style="text-align: justify;">
        <span id="Rebase_vs_Merge"><span><a id="11_2"></a>Rebase vs. Merge</span></span>
      </h3>

      <p style="text-align: justify;">
        There are opposing viewpoints regarding merging and rebasing.
      </p>

      <p style="text-align: justify;">
        <em><strong>Pro-merge argument</strong></em>
      </p>

      <p style="text-align: justify;">
        The true commit history of a repository should not be altered because it’s a record of occurrences.
      </p>

      <p style="text-align: justify;">
        <strong><em>Pro-rebase argument<br /> </em></strong><br /> Commit histories should be polished and well edited.
      </p>

      <h2 style="text-align: justify;">
        <span id="Log"><span><a id="12"></a>Log</span></span>
      </h2>

      <p style="text-align: justify;">
        You can utilize the <code>git log</code> command to view committed snapshots. You can use the command to see a project’s history, use a filter, and find specific modifications. Here are example uses of the <code>git log</code> command:
      </p>

      <pre class="lang:default decode:true">$ git log</pre>

      <p style="text-align: justify;">
        This command lists the exhaustive commit history in default formatting.
      </p>

      <pre class="lang:default decode:true">$ git log -n 3</pre>

      <p style="text-align: justify;">
        This command line only allows for 3 displayed commits.
      </p>

      <pre class="lang:default decode:true">$ git log --stat</pre>

      <p style="text-align: justify;">
        This command results in the display of regular git log information, as well as information on which files underwent modification and the relative numbers of line deletions and additions from each of them.
      </p>

      <pre class="lang:default decode:true">$ git log -p</pre>

      <p style="text-align: justify;">
        This command shows the full diff of each commit.
      </p>

      <pre class="lang:default decode:true">$ git log --grep="updated"</pre>

      <p style="text-align: justify;">
        With this command, one can search for commits containing the string “updated.”
      </p>

      <pre class="lang:default decode:true">$ git log --author="smith"</pre>

      <p style="text-align: justify;">
        The command above allows for a specific search for commits by an author whose name includes the string “smith.”
      </p>

      <pre class="lang:default decode:true">$ git log index.html</pre>

      <p style="text-align: justify;">
        The command above will display only those commits including the index.html file.
      </p>

      <pre class="lang:default decode:true">$ git log --oneline</pre>

      <p style="text-align: justify;">
        This command presents repository information in a single line, so you can get a high-level overview.
      </p>

      <p style="text-align: justify;">
        *Additional git log commands can be accessed via <code>git log --help</code>.
      </p>

      <h2 style="text-align: justify;">
        <span id="Tagging"><span><a id="13"></a>Tagging</span></span>
      </h2>

      <p style="text-align: justify;">
        With the use of tags, Git can flag certain points in a project’s history as being significant.
      </p>

      <p style="text-align: justify;">
        To list available Git tags, use the <code>git tag</code> command:<br /> Example:
      </p>

      <pre class="lang:default decode:true">git tag
 v1.0
 v2.0
 v3.0</pre>

      <p style="text-align: justify;">
        To list only particular types of tags, use the <code>git tag -l</code> command:
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">git tag -l “v1.2.9”
 v1.2.9
 v.1.2.9.1
 v.1.2.9.2
 v.1.2.9.3</pre>

      <h3 style="text-align: justify;">
        <span id="Create_a_Tag"><span><a id="13_1"></a>Create a Tag</span></span>
      </h3>

      <p style="text-align: justify;">
        There are two main categories of tags in Git: lightweight and annotated.
      </p>

      <p style="text-align: justify;">
        <strong><em>Lightweight</em></strong>
      </p>

      <p style="text-align: justify;">
        A lightweight tag is a pointer to a particular commit – like a branch.
      </p>

      <p style="text-align: justify;">
        <em><strong>Annotated</strong></em>
      </p>

      <p style="text-align: justify;">
        An annotated tag is stored in the Git database as a full object, containing a tagging message, tagger name and email, and tag date. Annotated Tags can also undergo signing and verification with GNU Privacy Guard (GPG). The best practice is to use annotated tags when possible, to allow for storage of valuable information and privacy safeguards.
      </p>

      <p style="text-align: justify;">
        <span><strong><em>Create Annotated Tags</em></strong></span>
      </p>

      <p style="text-align: justify;">
        To create an annotated tag, use <code>-a</code> with the <code>git tag</code> command.<br /> Example:
      </p>

      <pre class="lang:default decode:true">$ git tag -a v2.0 -m “my version 2.0”
$ git tag
v1.0
v1.5
v2.0</pre>

      <p style="text-align: justify;">
        Above, we see that the v2.0 tag has been created. We used <code>-m</code> for a specific tagging message.
      </p>

      <p style="text-align: justify;">
        To see the tag data and corresponding commit, use the <code>git show</code> command.<br /> Example:
      </p>

      <pre class="lang:default decode:true">$ git show v2.0
  tag v2.0

Tagger: Jane Smith
Date: Tues Aug 25 18:16:00 2015 -0700
my version 2.0

commit ka93a8dfg718ce54f00342227218580a92764449
Author: John Doe
Date: Wed Mar 19 20:38:12 2008 -0700</pre>

      <hr />

      <p style="text-align: justify;">
        <strong>Note:</strong> After the word “commit,” we see a checksum – a hash value containing 40 characters – which is stored in a file.
      </p>

      <p style="text-align: justify;">
        <span><strong><em>Create Lightweight Tags</em></strong></span>
      </p>

      <p style="text-align: justify;">
        A lightweight tag for commits only holds a checksum. To create a lightweight tag, simply utilize the <code>git tag</code> command without using <code>-a</code>, <code>-s</code>, or <code>-m</code>.<br /> Example:
      </p>

      <pre class="lang:default decode:true ">$ git tag v2.5
$ git tag
v1.0
v1.5
v2.0
v2.5</pre>

      <pre class="lang:default decode:true ">For the lightweight tag above, git show would only display the commit:</pre>

      <pre class="lang:default decode:true">$ git show v2.5
commit p641d7dff657pc77f33442017208880k91563822
Author: John Doe
Date: Wed Mar 19 20:33:05 2008 -0700

changed the version number</pre>

      <h3 style="text-align: justify;">
        <span id="Tag_Sharing"><span><a id="13_2"></a>Tag Sharing</span></span>
      </h3>

      <p style="text-align: justify;">
        Users must push tags to shared servers after creation because, by default, the <code>git push</code> command does not send tags to remote servers.
      </p>

      <p style="text-align: justify;">
        To push tags to shared servers, use the<code> git push origin [tagname]</code> command.
      </p>

      <p style="text-align: justify;">
        To push all of your tags at once, use the<code> --tags</code> option for the <code>git push</code> command.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">git push origin --tags</pre>

      <hr />

      <p style="text-align: justify;">
        <strong>Note:</strong> Anyone who pulls from your repository or clones it will also receive all of your tags.
      </p>

      <h3 style="text-align: justify;">
        <span id="Tag_Checkout"><span><a id="13_3"></a>Tag Checkout</span></span>
      </h3>

      <p style="text-align: justify;">
        Because you cannot move tags around, they cannot be checked out in Git. However, you can create a new branch at a specific tag with the <code>git checkout -b [branchname][tagname]</code> command.
      </p>

      <h2 style="text-align: justify;">
        <span id="Aliases"><span><a id="14"></a>Aliases</span></span>
      </h2>

      <p style="text-align: justify;">
        Aliases allow users to navigate Git in an easier fashion. To save time and effort, one can create aliases for Git commands, which eliminates the need to type out an entire default Git command.
      </p>

      <p style="text-align: justify;">
        You can set up aliases using the <code>git config</code> command.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <pre class="lang:default decode:true">$ git config --global alias.br branch
$ git config --global alias.st status
$ git config --global alias.co commit</pre>

      <p style="text-align: justify;">
        The above commands allow the user to simply type git br, git st, or git co, instead of git branch, git status, or git commit.<br /> You can also create new commands with an alias.<br /> Example:
      </p>

      <pre class="lang:default decode:true ">$ git config --global alias.stage “add”</pre>

      <p style="text-align: justify;">
        The above command makes <code>git stage</code> equivalent to <code>git add</code>.<br /> Many people create a last command for viewing the most recent commit.
      </p>

      <p style="text-align: justify;">
        Example:
      </p>

      <p style="text-align: justify;">
        <span>$</span> git config –global alias.last “log -1 HEAD”
      </p>

      <h2 style="text-align: justify;">
        <span id="Ignoring_Files"><span><a id="15"></a>Ignoring Files</span></span>
      </h2>

      <p style="text-align: justify;">
        Usually, untracked files consist of uncommitted files recently added to a project or compiled binaries with extensions such as .exe and .obj. Compiled binaries can make it difficult to clearly monitor your repository. Git allows users to avoid this situation by ignoring certain files by sending them to a special file with the name .gitignore.
      </p>

      <p style="text-align: justify;">
        <strong>Note:</strong> It’s always wise to check your repository before committing anything, to avoid accidentally committing certain files.
      </p>

      <ul style="text-align: justify;">
        <li>
          Files that all developers should disregard go into a <span>.gitignore</span> file.
        </li>
        <li>
          Every line within a gitignore file indicates a pattern.
        </li>
      </ul>

      <p style="text-align: justify;">
        Additional information regarding gitignore files can be found <span><a href="http://git-scm.com/docs/gitignore%20and%20https://github.com/github/gitignore" target="_blank">here</a></span>.
      </p>

      <h2 style="text-align: justify;">
        <span id="Distributed_Workflows"><span><a id="16"></a>Distributed Workflows</span></span>
      </h2>

      <p style="text-align: justify;">
        As we mentioned earlier, distributed workflows allow developers collaborating on projects much more flexibility. Every developer using Git can be both a node and a hub; this means he/she can own a main repository which collaborators contribute to and base their work off of while also contributing code to other repositories. This opens up the possibilities for workflows immensely.<br /> For now, we are only going to discuss a couple of the most common distributed workflows.
      </p>

      <h3 style="text-align: justify;">
        <span id="Centralized_Workflow"><span><a id="16_1"></a>Centralized Workflow</span></span>
      </h3>

      <p style="text-align: justify;">
        The centralized workflow entails the existence of one main hub, which can accept code. In this type of structure, many developers synchronize to this central repository, pushing and merging changes to it.
      </p>

      <h3 style="text-align: justify;">
        <span id="Integration-Manager_Workflow"><span><a id="16_2"></a>Integration-Manager Workflow</span></span>
      </h3>

      <p style="text-align: justify;">
        This workflow structure involves multiple remote repositories. In this scenario, there is often one main project repository, and developers can have read access to others’ repositories and write access to their own. Those involved in the Integration-Manager Workflow create clones of the main project repository, push any changes to it, and ask the repository maintainer to pull in the pushed changes. The maintainer can add someone’s repository as a remote, test changes locally, merge them into their branch, and push back to their repository.<br /> This workflow is commonly found with hub-centered resources, such as GitHub (we’ll talk about GitHub in the next section).
      </p>

      <h2 style="text-align: justify;">
        <span id="GitHub"><span><a id="17"></a>GitHub</span></span>
      </h2>

      <p style="text-align: justify;">
        As previously mentioned, GitHub is a hub-focused tool which facilitates Integration-Manager Workflow. It is the largest existing host for Git repositories and is used by millions of developers worldwide. A high percentage of Git repositories reside on GitHub, and this resource is used by many open source projects.
      </p>

      <h3 style="text-align: justify;">
        <span id="Getting_Started-2"><span><a id="17_1"></a>Getting Started</span></span>
      </h3>

      <p style="text-align: justify;">
        First, in order to use GitHub, you must set up a free user account. You can easily do this by visiting <span><a href="https://github.com/" target="_blank">https://github.com</a></span>.
      </p>

      <h3 style="text-align: justify;">
        <span id="Contributing_to_Projects"><span><a id="17_2"></a>Contributing to Projects</span></span>
      </h3>

      <p style="text-align: justify;">
        To contribute to a project which you do not have push privileges for, you can “fork” a project, which means you will have your own copy of it, which you can freely push to. To complete a “fork,” look for the Fork button at the top right of the project page.
      </p>

      <h3 style="text-align: justify;">
        <span id="General_Workflow"><span><a id="17_3"></a>General Workflow</span></span>
      </h3>

      <p style="text-align: justify;">
        GitHub’s collaboration workflow revolves around Pull Requests.
      </p>

      <p style="text-align: justify;">
        Here are the main steps of the GitHub collaboration workflow:
      </p>

      <ol style="text-align: justify;">
        <li>
          Create a topic branch from master.
        </li>
        <li>
          Commit changes for the project.
        </li>
        <li>
          Push the topic branch to the GitHub project.
        </li>
        <li>
          Open a Pull Request.
        </li>
        <li>
          Confer with team and perform additional commits, if applicable.
        </li>
        <li>
          The project owner closes or merges the Pull Request.
        </li>
      </ol>

      <h2 style="text-align: justify;">
        <span id="Additional_Resources"><span><a id="18"></a>Additional Resources</span></span>
      </h2>

      <p style="text-align: justify;">
        <span><a href="http://git-scm.com/" target="_blank">http://git-scm.com</a></span>
      </p>

      <p style="text-align: justify;">
        <span><a href="https://github.com/" target="_blank">https://github.com/</a></span>
      </p>

      <p style="text-align: justify;">
        <span><a href="https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf" target="_blank">https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf</a></span>
      </p>

      <p style="text-align: justify;">
        <span><a href="http://alx.github.io/gitbook/" target="_blank">http://alx.github.io/gitbook/</a></span>
      </p>

      <p style="text-align: justify;">
        <span><a href="https://progit.org/" target="_blank">https://progit.org/</a></span>
      </p>

      <p style="text-align: justify;" class="note_normal">
        <span>Published on Codingpedia.org with the permission of</span><span class="Apple-converted-space"> </span>Udemy<span class="Apple-converted-space"> </span><span>– source </span><a title="http://aredko.blogspot.ch/2015/02/a-fresh-look-on-accessing-database-on.html" href="https://blog.udemy.com/git-tutorial-a-comprehensive-guide/" target="_blank">Git Tutorial: A Comprehensive Introduction</a><span> from</span><span class="Apple-converted-space"> </span><a title="http://aredko.blogspot.com" href="https://blog.udemy.com/" target="_blank">https://blog.udemy.com/</a>
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
