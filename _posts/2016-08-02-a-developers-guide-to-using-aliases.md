---
layout: post
title: A developer's guide to using aliases
description: "In this post, I am presenting a short introduction to aliases and how I am using them to make my everyday developer life easier"
author: ama
permalink: /ama/a-developers-guide-to-using-aliases/
published: true
categories: [dev tools]
tags: [linux shell, alias, dev tools, linux, unix]
---

Not long time ago I have rediscovered an old friend - Bash[^1] the Alias[^2]. We were never best friends until now. We got acquainted at the beginning of my computer science studies. I visited back then a course held by Cisco, "Linux Essentials" or something like that. The trainer mentioned at one point what were aliases and how handy they could be. Well 12 years later, and I still had not gotten that, until recently, when the alias presented himself in a new light to me. Since then we've been best friends. Read on to learn why.

[^1]: <https://www.gnu.org/software/bash/manual/html_node/What-is-Bash_003f.html>
[^2]: <http://tldp.org/LDP/abs/html/aliases.html>

<!--more-->

## What are aliases?

So what are aliases? According to The Linux Documentation Project[^2] - "A Bash alias is essentially nothing more than a keyboard shortcut, an abbreviation, a means of avoiding typing a long command sequence. If, for example, we include **`alias lm="ls -l | more"`** in the **~/.bashrc** file, then each **lm[^3]** typed at the command-line will automatically be replaced by a **`ls -l | more`**. This can save a great deal of typing at the command-line and avoid having to remember complex combinations of commands and options. Setting alias **rm="rm -i"** (interactive mode delete) may save a good deal of grief, since it can prevent inadvertently deleting important files." In other words it's a real time saver, and you know time is the most valuable asset, right?

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


### Make aliases permanent

If you use the alias command as shown above it will only be valid for the current terminal session, that's why I hardly use it like this, but rather have them permanently persisted in a an appropriate configuration file that is sourced[^5] by every login session, whose name or location may vary according to the system.
These files are usually __.bashrc__, __.bash_profile__ or __.profile__ in the user's home directory.
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
A good introduction to the .dot files can be found if you visit <a href="http://mywiki.wooledge.org/DotFiles" target="_blank">Configuring your login sessions with dot files</a>
</p>

### Removing aliases

To remove an alias we use the `unalias` command:

```
unalias [-a] name(s)
```

Remove each name from the list of defined  aliases. If **-a** is supplied,  all  alias definitions are removed (thank god this is true only for the current terminal session)

## Approach to using aliases

Ok, now that you have a good understanding of what are aliases and how to define them, let me tell you a bit about my approach to using them.

### Alias for alias commands

First of all, as mentioned before, I make them permanent in the **~/.bash_aliases** file

Secondly, I have come up with some aliases for the `alias` command:

```
alias alias-vim='vim ~/.bash_aliases'
alias alias-source='source ~/.bash_profile'
alias alias-grep='alias | grep'
```

The reason behind this is that when I want to add a new alias I edit the aliases file with the `alias-vim` command and then I source it.

The third `alias-grep` command I find it useful when I need some information, I know I can find inside an alias - so for example if I need to find the IP of my GoDaddy server I would use

```
alias-grep ssh-g
```

> I knowing I probably have defined a alias to connect via ssh to GoDaddy host and I get the expected result:

```
alias ssh-godaddy='ssh ama1983@188.121.50.241'
```

### Alias format

The next points will highlight some of the formats I use for aliases.

> For aliases involving more words, I separate the words via dashes (-)

#### Shortcuts

I use easy to remember shortcuts, that have no meaning per se. For example here are some snippets from ls[^6] and maven[^7] commands:

```
#ls
alias ll="ls -lha"; # list files and directories in long format (-l), also hidden ones (-a) with human-readable sizes (-h)

#maven
alias mci="mvn clean install"
alias mcp="mvn clean package"
alias mcis="mvn clean install -DskipTests"
```

[^6]: <http://linuxcommand.org/man_pages/ls1.html>
[^7]: <https://maven.apache.org/>

#### Commands plus meaningful text - `{command}-{relevant}-{text}`

> **Very very important** - In this take full advantage of autocomplete functionality you can use in most terminals when you use the **tab** button

The second format I use is having the command itself plus some that has meaning to me.
For example:

```
alias ssh-godaddy='ssh ama1983@188.121.50.241'
```
I usually need to type `"ssh-g" + tab` and the alias gets autocompleted.

or

```
alias ps-grep="ps aux | grep" # e.g."ps-grep java" will list processes that have java in description
```

#### Context plus command plus meaningful text - `{context}-{command}||{context-options}-{relevant}-{text}`

In this case I use aliases that apply to a tool command (e.g. tomcat, mysql, nginx etc.), followed by a shell command or options valid in that context. For example:

```
alias tomcat-start="sudo initctl start tomcat" # start tomcat via setup service on Ubuntu System command
alias tomcat-tail-log="tail -f /opt/tomcat/logs/catalina.out" # tail tomcat log command
alias mysql-vim-my.conf="sudo vim /etc/mysql/my.cnf" # edit mysql configuration command
alias nginx-config-verify="sudo nginx -t -c /etc/nginx/nginx.conf" # verify validity of nginx configuration file command
```

### Advanced aliases

Bash aliases does not support parameters directly, but if need some with parameters you can create a function and alias the function. For example, if I want to execute backup of files with the current timestamp and some text at the end separated by dashes:

```
alias nginx-config-backup=nginxconfigbackup

nginxconfigbackup(){
  sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.$(date "+%Y-%m-%d_%H:%M")-$1 #the parameter ending is the comment use dashes "-" between words
}
```

So when I execute for example `nginx-config-backup add-proxy-support` will result in a file called `nginx.conf.2016-08-04_17:18-add-proxy-support` in the `/etc/nginx/` directory.

Or if I want to `grep` recursively for a `text` starting from the current directory I would write something like this:

```
grepRecursivelyForText(){
  grep -ri $1 .
}

alias grep-recursively=grepRecursivleyForText

```

and use it like `grep-recursively some_text`.

> I have to admit that in this second example the alias is longer then the command itself, but it's easier for me to remember and with the use of the autocomplete is no problem...

## List of personal aliases

I will now list some of the aliases I usein alphabetical order, so that you might find some inspiration for your own:

### Alias

```
#alias
alias alias-source="source ~/.bash_profile"
alias alias-vim="vim ~/.bash_aliases"
alias alias-grep="alias | grep"
```

### Ls

```
#ls -- list directory contents
alias ls="ls -CF" # make ls display in columns and with a file type indicator by default("/")
alias ll="ls -l" # -l list in long format
alias llrt="ls -lrt" # list in long format, in reverse order of modification time (I find it easier to see last modified items at the end)
alias ls-dir="ls -al | grep ^d" #  listing of directories in the current directory
```

### Cd

```
#cd -- change directory
alias ..="cd .."
alias cd..="cd .."
alias cd-projects="cd /Users/ama/projects/"
alias cd-codingpedia.github.io='cd ~/projects/blogs/codingpedia.github.io'
alias cd-jboss-eap-6.1-logs="cd ~/dev/as/jboss-eap-6.1/standalone/logs
```

> `cd` alone will change to the user's home directory

### Df
```
#df -- display free disk space
alias df="df -h" # "human-readable" output
```

### Maven

```
#maven
alias mci="mvn clean install"
alias mcp="mvn clean package"
alias mcis="mvn clean install -DskipTests"
```

### Jekyll
```
#jekyll
alias jekyll-serve-dev="bundle exec jekyll serve --config _config.yml,_config.dev.yml"
```

### Keycloak 2.0
```
#keycloak 2.0
alias kc2-cd-home="cd ~/dev/as/jboss-eap-7.0-keycloak-server/"
alias kc2-export-JBOSS_HOME="export JBOSS_HOME=~/dev/as/jboss-eap-7.0-keycloak-server/"
alias kc2-vim-standalone.xml="vim ~/dev/as/jboss-eap-7.0-keycloak-server/standalone/configuration/standalone.xml"
alias kc2-start="~/dev/as/jboss-eap-7.0-keycloak-server/bin/standalone.sh -Djboss.socket.binding.port-offset=200 -Djdk.tls.client.protocols=TLSv1 -b 0.0.0.0 --server-config=standalone.xml"
```

### Mkdir
```
#mkdir -- make directory
alias mkdir="mkdir -pv" # make necessary parent directory, and make it verbose to help avoid typos
```

### MySql
```
# MySql
alias mysql-start="sudo service mysql start"
alias mysql-stop="sudo service mysql stop"
alias mysql-restart="sudo service mysql restart"
alias mysql-status="sudo service mysql status"
alias mysql-connect-root="mysql -uroot -p"
alias mysql-vim-my.conf="sudo vim /etc/mysql/my.cnf"
```

### Nginx
```
# Nginx
alias nginx-start="sudo nginx"
alias nginx-stop="sudo nginx -s stop"
alias nginx-restart="sudo service nginx restart"
alias nginx-status="sudo ps aux | grep nginx"
alias nginx-reload="sudo kill -HUP `cat /usr/local/var/run/nginx.pid`"
alias nginx-vim-nginx.conf="sudo vim /usr/local/etc/nginx/nginx.conf"
alias nginx-config-verify="sudo nginx -t -c /usr/local/etc/nginx/nginx.conf"
alias nginx-config-backup=nginxconfigbackup

nginxconfigbackup(){
  sudo cp /usr/local/etc/nginx/nginx.conf /usr/local/etc/nginx/nginx.conf.$(date "+%Y-%m-%d_%H:%M")-$1 #the parameter ending is the comment use dashes "-" between words
}
```

### Npm - https://www.npmjs.com/
```
#npm
alias npmi="npm install"
alias npmig="npm install -g" # -g install globally
```

### Ps
```
#ps -- process status
alias ps-grep="ps aux | grep" # e.g."ps-grep java" will list processes that have java in description
```

### Rhc
```
#rhc -- OpenShift client tools
alias rhc-tail-jbosseap.log="rhc tail -o "-n100" -f app-root/logs/jbosseap.log -a heappbackendlarge -n bkwtest"
alias rhc-tail-homeenergy.log="rhc tail -o "-n100" -f app-root/logs/homeenergy.log -a heappbackendlarge -n bkwtest"
alias rhc-ssh-heappbackendlarge="rhc ssh -a heappbackendlarge -n bkwtest"
alias rhc-port-forward-heappbackendlarge="rhc port-forward -a heappbackendlarge -n bkwtest"
```
> here you can truly see how powerful and time saving aliases can be...

### Tomcat
```
# tomcat
alias tomcat-start="/opt/tomcat/bin/startup.sh"
alias tomcat-stop="/opt/tomcat/bin/shutdown.sh"
alias tomcat-status="sudo ps aux | grep tomcat"
alias tomcat-cd-webapps="cd /opt/tomcat/webapps"
alias tomcat-copy-root-to-apps="sudo cp $HOME/projects/podcastpedia/web-ui/target/ROOT.war /opt/tomcat/webapps"
alias tomcat-show-log="tail -f /opt/tomcat/logs/catalina.out"
alias tomcat-backup="cp /opt/tomcat/webapps/ROOT.war \"$HOME/podcastpedia/ROOT.war.backup.$(date +%F_%R)\""
alias tomcat-remove-root="rm -rf /opt/tomcat/webapps/ROOT*"
```

## Video introduction to the concept

<iframe width="560" height="315" src="https://www.youtube.com/embed/Emlc7mkZDQ4" frameborder="0" allowfullscreen></iframe>

## Conclusion

All aliases are good aliases as long as they make your terminal life easier :). I hope you've enjoyed this and please leave a comment below if you have any suggestions.
