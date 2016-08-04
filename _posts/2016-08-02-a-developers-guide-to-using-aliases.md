---
layout: post
title: A developer's guide to using aliases
description: "In this post, I am presenting a show introduction to aliases and how I am using bash aliases to make my everyday developer life easier"
author: ama
permalink: /ama/a-developers-guide-to-using-aliases/
published: true
categories: [dev tools]
tags: [linux shell, alias, dev tools]
---

Not long time ago I have rediscovered an old friend - Bash[^1] Alias[^2]. We got acquainted at the beginning of my computer science studies, when I went to course held by Cisco "Linux Essentials" or something similar, where the trainer mentioned at one point what were aliases and how handy they could be. Well 12 years later, and I still had not got that,
until recently where a flash of illumination struck me and since then I've been using them extensively in my everyday developer life. In this post I will present a short introduction to aliases and then my way of using them.

[^1]: <https://www.gnu.org/software/bash/manual/html_node/What-is-Bash_003f.html>
[^2]: <http://tldp.org/LDP/abs/html/aliases.html>

<!--more-->

## What are aliases?

So what are aliases? According to The Linux Documentation Project[^2] - "A Bash alias is essentially nothing more than a keyboard shortcut, an abbreviation, a means of avoiding typing a long command sequence. If, for example, we include **`alias lm="ls -l | more"`** in the **~/.bashrc** file, then each **lm[^3]** typed at the command-line will automatically be replaced by a **`ls -l | more`**. This can save a great deal of typing at the command-line and avoid having to remember complex combinations of commands and options. Setting alias **rm="rm -i"** (interactive mode delete) may save a good deal of grief, since it can prevent inadvertently deleting important files." Briefly said, it's a real time saver, and you know time is the most valuable asset, right?

The alias command is built into a number of shells including __ash__, __bash__ (the default shell on most Linux systems), __csh__ and __ksh__. Aliases are recognized only by the shell in which they are created, and they apply only for the user that creates them, unless that user is the __root__ (i.e., administrative) user, which can create aliases for any user.[^4]

[^3]: ... as the first word of a command string. Obviously, an alias is only meaningful at the __beginning__ of a command.
[^4]: <http://www.linfo.org/alias.html>

### Listing and creating aliases

In bash shell the command is

```
alias [-p] [name="value"]
```

for example

```
alias ls-dir="ls -al | grep ^d"
```

which lists all of the directories in the current directory - list in **long format** including entries that start with a (.)
and filter those (grep) which start with **d**

`Alias` with no arguments or with the `-p` option prints the list of aliases  in  the form alias `name=value` on standard output.


### Make permanent aliases

If you use the alias command as shown above it will only be valid for the current terminal session, that's why I hardly use it like this, but rather have them permanently persisted in a an appropriate configuration file that is sourced[^5] by every login session, whose name or location may vary according to the system.
These files are usually __.bashrc__, __.bash_profile__ or __.profile in the user's home directory.
I prefer holding them in a separate __.bash_aliases__ file and insert it into one of the files mentioned before. Here is a snippet from the __.bash_profile__ file from my MAC OS X machine:

```
less ~/.bash_profile

....
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
......
```

[^5]: <http://ss64.com/bash/source.html>

<p class="note_normal">
A good introduction to the .dot files can be found if you visit <a href="http://mywiki.wooledge.org/DotFiles" target="_blank">Configuring your login sessions with dot files</a> and if want to publish on Codingpedia.org I recommend you use them too.
</p>

### Removing aliases

To remove an alias we use the `unalias` command:

```
unalias [-a] name(s)
```

Remove each name from the list of defined  aliases. If **-a** is supplied,  all  alias definitions are removed (thank god this is true only for the current terminal session)

## Approach to using aliases

Ok, now that you have a good understanding of alias and how to define them let me tell you about my approach.
Well, as mentioned before I store the aliases in the **~/.bash_aliases** file.

### Shortcuts

For short command I use shortcuts I can easily remember. For example for maven[^6] commands:

```
#maven
alias mci="mvn clean install"
alias mcp="mvn clean package"
```

[^6]<https://maven.apache.org/>

### Command plus relevant text - `{command}-{relevant}-{text}`
The second approach applies for longer commands or I use the command itself plus relevant text I can understand.

> **Very important** - I take full advantage of autocomplete functionality you can use in most terminals when you use the **tab** button

For example:

```
alias ssh-godaddy='ssh ama1983@188.121.50.241'
```
I usually need to type `"ssh-g" + tab` and the alias gets autocompleted.

or

```
alias ps-grep="ps aux | grep" # e.g."ps-grep java" will list processes that have java in description
```

### Content plus command plus relevant text - `{context}-{command}-{relevant}-{text}`

## List of personal aliases

I will now list in alphabetical order some of the aliases I use, so that you might find some inspiration for your own:

```
#alias
alias alias-source="source ~/.bash_profile"
alias alias-vim="vim ~/.bash_aliases"
alias alias-grep="alias | grep"
```

```
#ls -- list directory contents
alias ls="ls -CF" # make ls display in columns and with a file type indicator by default("/")
alias ll="ls -l" # -l list in long format
alias llrt="ls -lrt" # list in long format, in reverse order of modification time (I find it easier to see last modified items at the end)
alias ls-dir="ls -al | grep ^d" #  listing of directories in the current directory
```

```
#cd -- change directory
alias ..="cd .."
alias cd..="cd .."
alias cd-projects="cd /Users/ama/projects/"
alias cd-jboss-eap-6.1-logs="cd ~/dev/as/jboss-eap-6.1/standalone/logs
```

> `cd` alone will change to the user's home directory

#df -- display free disk space

```
alias df="df -h" # "human-readable" output
```

```
#maven
alias mci="mvn clean install"
alias mcp="mvn clean package"
alias mcis="mvn clean install -DskipTests"
```

```
#jekyll
alias jekyll-serve-dev="bundle exec jekyll serve --config _config.yml,_config.dev.yml"
```

```
#keycloak 2.0
alias kc2-cd-home="cd ~/dev/as/jboss-eap-7.0-keycloak-server/"
alias kc2-export-JBOSS_HOME="export JBOSS_HOME=~/dev/as/jboss-eap-7.0-keycloak-server/"
alias kc2-vim-standalone.xml="vim ~/dev/as/jboss-eap-7.0-keycloak-server/standalone/configuration/standalone.xml"
alias kc2-start="~/dev/as/jboss-eap-7.0-keycloak-server/bin/standalone.sh -Djboss.socket.binding.port-offset=200 -Djdk.tls.client.protocols=TLSv1 -b 0.0.0.0 --server-config=standalone.xml"
```

```
#mkdir -- make directory
alias mkdir="mkdir -pv" # make necessary parent directory, and make it verbose to help avoid typos
```

```
#ps -- process status
alias ps-grep="ps aux | grep" # e.g."ps-grep java" will list processes that have java in description
```

```
#rhc -- OpenShift client tools
alias rhc-tail-jbosseap.log="rhc tail -o "-n100" -f app-root/logs/jbosseap.log -a heappbackendlarge -n bkwtest"
alias rhc-tail-homeenergy.log="rhc tail -o "-n100" -f app-root/logs/homeenergy.log -a heappbackendlarge -n bkwtest"
alias rhc-ssh-heappbackendlarge="rhc ssh -a heappbackendlarge -n bkwtest"
alias rhc-port-forward-heappbackendlarge="rhc port-forward -a heappbackendlarge -n bkwtest"
```
> here you can truly see how powerful and time saving aliases can be...

```
# Tomcat
alias tomcat-start="/opt/tomcat/bin/startup.sh"
alias tomcat-stop="/opt/tomcat/bin/shutdown.sh"
alias tomcat-status="sudo ps aux | grep tomcat"
alias tomcat-cd-webapps="cd /opt/tomcat/webapps"
alias tomcat-copy-root-to-apps="sudo cp $HOME/projects/podcastpedia/web-ui/target/ROOT.war /opt/tomcat/webapps"
alias tomcat-show-log="tail -f /opt/tomcat/logs/catalina.out"
alias tomcat-backup="cp /opt/tomcat/webapps/ROOT.war \"$HOME/podcastpedia/ROOT.war.backup.$(date +%F_%R)\""
alias tomcat-remove-root="rm -rf /opt/tomcat/webapps/ROOT*"
```

## Conclusion

All aliases are good aliases as long as they relevant for you and make your terminal life easier. I hope you've enjoyed this and if you have any suggestion please leave a comment below.
