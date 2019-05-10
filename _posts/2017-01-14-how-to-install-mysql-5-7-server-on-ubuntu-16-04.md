---
layout: post
title: How to install MySQL 5.7 Server on Ubuntu 16.04
description: "This post describes how to install a MySQL 5.7 Community Edition on a fresh Ubuntu 16.04 system. At the end are some suggestions and links on how to tweak the configuration of the server"
author: ama
permalink: /ama/how-to-install-mysql-server-5-7-on-ubuntu-16-04
published: true
categories: [mysql, ubuntu]
tags: [mysql, ubuntu, setup]
---

This is not the first time I need to install and configure a MySql Community Server on a fresh system with Ubuntu (16.04) running on it. So instead next time to have to
google for the individual steps again, I decided to write a post. Find out also how to tweak the Server configuration.

<!--more-->

## Install MySQL Server
This guide is based on the official MySQL documentation - [A Quick Guide to Using the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/), where my own data is used instead.

### Add the MySQL APT Repository
Go to the download page for the MySQL APT repository at http://dev.mysql.com/downloads/repo/apt/ and select the package for my Linux distribution. As said mine is
Ubuntu 16.04, which is covered by the `mysql-apt-config_0.8.1-1_all.deb` package at the time of this writing:

```bash
shell> wget http://dev.mysql.com/get/mysql-apt-config_0.8.1-1_all.deb
```

Next install the release package with the following command:

```bash
shell> sudo dpkg -i mysql-apt-config_0.8.1-1_all.deb

Selecting previously unselected package mysql-apt-config.
(Reading database ... 27837 files and directories currently installed.)
Preparing to unpack mysql-apt-config_0.8.1-1_all.deb ...
Unpacking mysql-apt-config (0.8.1-1) ...
Setting up mysql-apt-config (0.8.1-1) ...
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
/usr/bin/locale: Cannot set LC_CTYPE to default locale: No such file or directory
/usr/bin/locale: Cannot set LC_ALL to default locale: No such file or directory
OK
```

> During installation I chose the MySQL 5.7 Server version

The next **mandatory step** is to update the package information from the MySQL APT repository

```bash
shell> sudo apt-get update

Hit:1 http://mirrors.linode.com/ubuntu xenial InRelease
Get:2 http://mirrors.linode.com/ubuntu xenial-updates InRelease [102 kB]
Get:3 http://mirrors.linode.com/ubuntu xenial-backports InRelease [102 kB]
Get:4 http://repo.mysql.com/apt/ubuntu xenial InRelease [14.2 kB]
Hit:5 http://ppa.launchpad.net/webupd8team/java/ubuntu xenial InRelease
Get:6 http://security.ubuntu.com/ubuntu xenial-security InRelease [102 kB]
Get:7 http://repo.mysql.com/apt/ubuntu xenial/mysql-5.7 Sources [886 B]
Get:8 http://repo.mysql.com/apt/ubuntu xenial/mysql-apt-config amd64 Packages [567 B]
Get:9 http://repo.mysql.com/apt/ubuntu xenial/mysql-apt-config i386 Packages [567 B]
Get:10 http://repo.mysql.com/apt/ubuntu xenial/mysql-5.7 amd64 Packages [2709 B]
Get:11 http://repo.mysql.com/apt/ubuntu xenial/mysql-5.7 i386 Packages [2712 B]
Get:12 http://repo.mysql.com/apt/ubuntu xenial/mysql-tools amd64 Packages [2608 B]
Get:13 http://repo.mysql.com/apt/ubuntu xenial/mysql-tools i386 Packages [1928 B]
Fetched 333 kB in 0s (603 kB/s)
Reading package lists... Done
```

### Installing MySQL with APT
Now to install MySQL execute the following command:
```bash
shell> sudo apt-get install mysql-server
```

> You will be asked for the root password during the installation. **Make a note of it if** you want to spare the time needed to reset it later[^1]

[^1]: <http://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html>

The server is started automatically after installation. You can check the version of the server by issuing the following command:

```bash
shell> mysql --version

mysql  Ver 14.14 Distrib 5.7.17, for Linux (x86_64) using  EditLine wrapper
```

### Starting and stopping the MySQL Server

You can check the status:

```bash
shell> sudo service mysql status
```

stop it:

```bash
shell> sudo service mysql stop
```

and start it again:

```bash
shell> sudo service mysql start
```

Because I am lazy and I will probably repeat these commands several times, at least in the beginning, I defined some aliases for them - snippet from my .bash_aliases file:

```bash
# MySql
alias mysql-start="sudo service mysql start"
alias mysql-stop="sudo service mysql stop"
alias mysql-restart="sudo service mysql restart"
alias mysql-status="sudo service mysql status"
alias mysql-connect-root="mysql -uroot -p"
alias mysql-vim-mysql.conf.d.my.cnf="sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf"
```

> See my post [A developer's guide to using aliases](http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/) to learn how I've become best buddies with the Bash alias. O, and I've
also created a video with it.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Emlc7mkZDQ4" frameborder="0" allowfullscreen></iframe>


## Tune MySQL configuration

Most of the default configuration values are fine, but you can tweak it if you don't have enough memory [^2] (like my server running on a 2GB RAM machine) or want to improve
performance [^3].

[^2]: <https://www.linode.com/docs/websites/hosting-a-website>
[^3]: <http://www.speedemy.com/17-key-mysql-config-file-settings-mysql-5-7-proof/>

> I have personally written about [optimizing MySQL Server Settings](http://www.codingpedia.org/ama/optimizing-mysql-server-settings/), but this remains a science for itself to me. Anyway before you are ready to override
 a default value in `my.cnf`, have a look in the official documentation about [Server System Variables](https://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html) and try to reason what each
 really means...

Now which configuration file should I use, because there are bunch of them under the `/etc/mysql` directory:

```bash
/etc/mysql/
|-- conf.d
|   -- mysql.cnf
|-- my.cnf -> /etc/alternatives/my.cnf
|-- my.cnf.fallback
|-- mysql.cnf
|-- mysql.cnf.2017-01-13-original
|-- mysql.conf.d
    -- mysqld.cnf
```

Well, because these are server settings, I want to override I need to put them below the [mysld] tag, which is to be found in `mysqld.conf`. So edit this file:

```bash
shell> sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

which looks like the following:

```bash
#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
log-error       = /var/log/mysql/error.log
# By default we only accept connections from localhost
bind-address    = 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

# my overrides
max_allowed_packet = 1M
max_connections = 75
table_open_cache = 32M
key_buffer_size = 32M
```

> Remember these are values for a small 2GB RAM server. It is supposed to run one MySQL Database among a bunch of other servers... Also very important to notice is that you can now change the configuration values at runtime, but they won't be persisted when the server is restarted.


## References
