---
layout: post
title: The alias way to backup mysql database from command line
description: "Tired of searching for the right command every time you need to back up a MySql database via mysqldump. Well make an alias out of it, and it should work for a while."
author: ama
permalink: /ama/the-alias-way-to-backup-a-mysql-database-from-command-line
published: true
categories: [database]
tags: [mysql, bash, linux]
---

As I have told you [before](http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/), I am really hooked on bash aliases[^2].
 This blog entry emphasizes that point and presents how to make a MySql database backup via `mysqldump` with only three words, even with three letters, if you will.

[^1]: <http://tldp.org/LDP/abs/html/aliases.html>
[^2]: <http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/>

So, without further ado, let's see the alias:

```

$ alias mysql-backup-db_name='mysqldump db_name -u db_user -p -h 127.0.0.1 --port 3306 --single-transaction > path_to_back_up_directory/db_name_$(date "+%Y-%m-%d_%H:%M").sql'
$ mysql-backup-db_name
```

or, same with three letters:

```

$ alias mbd='mysqldump db_name -u db_user -p -h 127.0.0.1 --port 3306 --single-transaction > path_to_back_up_directory/db_name_$(date "+%Y-%m-%d_%H:%M").sql'
$ mbd
```

<!--more-->

The option `-u`, will ask you for the password. The port number is not mandatory (as it defaults to **3306**), but if you are using other port, you need to specify it.

> I personally prefer the whole name approach, as then it is more clear to me. I can start typing `mysql-` and then **there is auto complete**. Besides that I can always `alias-grep` it[^1] - (`alias alias-grep='alias | grep'`),
 if I need to see how it looks like - using the alias in this case as sort of documentation...

Below there is a concrete example, where I backup the MySQL keycloak database:

```
$ mysql-backup-keycloak-prod
    aka
$ mysqldump keycloak -u keycloak -p -h 127.0.0.1 --port 3306 --single-transaction > ~/backup/db/keycloak_db_$(date "+%Y-%m-%d_%H:%M").sql
```

That will generate a _.sql_ file in the specified path:

```
$ ls -lrt ~/backup/db
-rw-rw-r-- 1 ama ama 199219 Mar 10 07:04 keycloak_db_2017-03-10_07:04.sql
-rw-rw-r-- 1 ama ama 198489 Apr  3 06:28 keycloak_db_2017-04-03_06:27.sql
```

We can take things even further, and back-up a remote database from the local machine, after we build a ssh-tunnel[^3] with an alias of course:

```
alias mysql-tunnel-linode='ssh -L 127.0.0.1:3305:127.0.0.1:3306 ama@w.x.y.z -N'
alias mysql-backup-keycloak-prod='mysqldump keycloak -u keycloak -p -h 127.0.0.1 --port 3305 --single-transaction > ~/backup/db/keycloak_db_prod_$(date "+%Y-%m-%d_%H:%M").sql'
```

Same back-up command as before, only the port differs now and is pointing to the tunnel port - 3305.

With this I think I've made my point about the power and versatility of bash aliases.


[^3]: <https://en.wikipedia.org/wiki/Tunneling_protocol>

## References
