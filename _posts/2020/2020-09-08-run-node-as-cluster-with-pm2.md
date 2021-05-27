---
layout: post
title: My troubles getting to run a Node application with PM2 in cluster mode with NVM
description: "In this blog post I will list of the trouble I went through getting to run PM2 in cluster mode on an
Ubuntu system, on which I had Node.js both standalone installed and managed with NVM."
author: ama
permalink: /ama/my-troubles-getting-to-run-a-node-application-with-pm2-in-cluster-mode-with-nvm
published: true
categories: [article]
tags:
    - cluster-computing
    - pm2
    - systemd
    - systemctl
    - node.js
---

In this blog post I will list of the troubles I went through getting to run the [Bookmarks.dev](https://www.codever.land) API on
PM2[^1] in cluster mode on an Ubuntu system with Node.js managed by NVM. The PM2 setup was on a single node (aka `fork mode`)
until now with no problems, but I decided to take advantage of the multi-core capability and enable cluster mode with PM2.

[^1]: <https://pm2.keymetrics.io/>

<!--more-->

## TLDR
Configure PM2 in cluster mode as advised in the documentation[^1]. Make sure you use the Node.js version you want PM2 to run on
and update PM2 to the latest version[^3] (Thus you find out you need to remove `interpreter` from the configuration). Disable the old version
 of installed pm2 service in `systemd` (`systemctl disable pm2.service`) and enable the new one (e.g. `systemctl enable pm2-ama.service`).
  Reboot your system.

> To run the API on different versions of Node.js easily I use [nvm](https://github.com/nvm-sh/nvm). Solving this nvm issue[^2],
> resulted in an elegant solution how you can do that .
> I am using a `.json` file to start the pm2 process and below is the working configuration in single mode (`fork mode`):
```json
 {
   apps : [{
     name : 'bookmarks.dev-api-node-10.15.0',
     script : 'bin/www',
     interpreter : "node@10.15.0",
     env: {
       "NODE_ENV": "development",
       "YOUTUBE_API_KEY" : "AIxxx..."
     },
     env_production : {
        "NODE_ENV": "production",
        "YOUTUBE_API_KEY" : "AIxxx...",
        "OTHER_ENV_VARIABLES" : "some-values"
     }
  }]
 }
 ```
[^2]: <https://github.com/Unitech/pm2/issues/1034>

## Running in `cluster` mode
According to the documentation[^3]  it should be fairly easy.
I would just start the application with the following new parameters:
```
    instances : "max",
    exec_mode : "cluster"
```

[^3]: <https://pm2.keymetrics.io/docs/usage/cluster-mode/>

So I though the `.json` config file should look something like the following:
```json
{
  apps : [{
    name : 'bookmarks.dev-api-node-10.15.0',
    script : 'bin/www',
    interpreter : "node@10.15.0",
    instances : "max",
    exec_mode : "cluster",
    env: {
      "NODE_ENV": "development",
      "YOUTUBE_API_KEY" : "AIzaSyCCo1VvwO8iMfxgquoYzyYM-A5eeyvJR4Q"
    },
    env_production : {
       "NODE_ENV": "production",
       "YOUTUBE_API_KEY" : "AIxxx...",
       "OTHER_ENV_VARIABLES" : "xxxxx"
    }
 }]
}
```

It did not work, the cluster started instances errored with the following message:
```shell
usersRouter.get('/:userId', keycloak.protect(), async (request, response) => {
                                                      ^
SyntaxError: Unexpected token (
    at Object.exports.runInThisContext (vm.js:78:16)
    at Module._compile (module.js:543:28)
```

Looking more exactly with `pm2 monit`, I saw the instances started with the wrong Node version (`7.4.0`) when
I was actually expecting `10.15.0`:
```
App Name              bookmarks.dev-api-node-10.15.0-cluster                                                                                                       │
Namespace             undefined                                                                                                                                    │
Version               undefined                                                                                                                                    │
Restarts              82                                                                                                                                           │
Uptime                0s                                                                                                                                           │
Script path           /home/ama/projects/bookmarks.dev/backend/bin/www                                                                                             │
Script args           N/A                                                                                                                                          │
Interpreter           node@v10.15.0                                                                                                                                │
Interpreter args      N/A                                                                                                                                          │
Exec mode             cluster                                                                                                                                      │
Node.js version       7.4.0
```

That explains the error above, because `async-await` was not supported until version 8 of Node.

> So next is to find out is why it was starting with an old version. This was not even a version managed
by nvm (`nvm list`).

## Unlink old node executable in /usr/local/bin
The old version (7.4.0) is from an initial installation of Node.js I used before switching to NVM. It was even
configured in `/usr/local/bin`:
```shell
whereis node
node: /usr/local/bin/node /opt/node/bin/node
```

This might be it - change this symlink to the newer Node.js version.

### Update to the latest LTS Node.js version with NVM (optional)
It was time to update to the newest LTS version. Find the latest LTS version:
```shell
nvm ls-remote --lts | grep -i latest

v4.9.1   (Latest LTS: Argon)
v6.17.1   (Latest LTS: Boron)
v8.17.0   (Latest LTS: Carbon)
v10.22.0   (Latest LTS: Dubnium)
v12.18.3   (Latest LTS: Erbium)
```

Then install the version v12 with the following command `nvm install v12.18.3`

Verify the version is installed, use it and set it as default :
```shell
$ nvm list
         v8.5.0
         v8.9.4
->       v10.15.0
         v12.18.3

nvm use 12
nvm alias default 12
```

Now I could update the `/usr/local/bin` symlink.
```shell
unlink /usr/local/bin/node
ln -s /home/ama/.nvm/versions/node/v12.18.3/bin/node /usr/local/bin/node
```

Reboot, but it still doesn't work - it means pm2 does not use the new Node as configured.

## First - update PM2
The pm2 version I was using was 2.7.0, and I thought it might be time for an update. This is very easy done by following the instructions[^4]
 in the documentation:
```shell
pm2 save #save all your processes
npm install pm2 -g #then install the latest PM2 version from NPM
pm2 update #finally update the in-memory PM2 process
```

Update the startup script because I have upgraded the Node.js version
```shell
pm2 unstartup
pm2 startup
```

This generates the following startup script:
```
[PM2] Init System found: systemd
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/home/ama/.nvm/versions/node/v12.18.3/bin /home/ama/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2 startup systemd -u ama --hp /home/ama
```

[^4]: <https://pm2.keymetrics.io/docs/usage/update-pm2/>

You need to execute the `sudo env ...` command.


That did not do it either, the app was still starting with the old Node.js version. The advantage now wasthe error message clearly indicated
that I cannot specify an `interpreter` when starting the app  in `cluster` mode.
"Yes in fork mode you can defined the interpreter but in cluster mode it's not possible
 as we rely on the current Node.js version that runs PM2 to use the cluster module
  ([https://github.com/Unitech/PM2/blob/master/lib/God/ClusterMode.js#L50](https://github.com/Unitech/PM2/blob/master/lib/God/ClusterMode.js#L50)" [^5]

[^5]: <https://github.com/Unitech/pm2/issues/1575>

Another strange thing noticed when running the `pm2 status` was it was complaining
that in memory I was still using the older version and need to execute `pm2 update` to use the new. But
when I ran `pm2 --version` in the terminal it would display the latest one.

> Reboot did not help either

## Investigate startup services
Thinking about possible causes led me to investigate the startup services I configured long time ago with `systemd`.

I got lucky with the following command:
```shell
systemctl list-units --type=service | grep -i pm2
pm2.service                    loaded active running PM2 process manager
```

Having now a closer look at it:
```shell
systemctl cat pm2.service

[Unit]
Description=PM2 process manager
Documentation=https://pm2.keymetrics.io/
After=network.target

[Service]
User=ama
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
TimeoutStartSec=8
Environment=PATH=/opt/node/bin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
Environment=PM2_HOME=/home/ama/.pm2
Restart=always
RestartSec=3

ExecStart=/usr/local/lib/node_modules/pm2/bin/pm2 resurrect --no-daemon
ExecReload=/usr/local/lib/node_modules/pm2/bin/pm2 reload all
ExecStop=/usr/local/lib/node_modules/pm2/bin/pm2 kill

[Install]
WantedBy=multi-user.target
```

I see the "old" node version is still in place, both in the Environment config `Environment=PATH=/opt/node/bin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin` and
in the entries `ExecStart`,`ExecReload` and `ExecStop`.


But, what about the new PM2 that I installed in the newer Node.js version? Well, it seems to be `inactive` and
 it can be found under the name `pm2-ama.service`.
```
systemctl status pm2-ama.service

● pm2-ama.service - PM2 process manager
   Loaded: loaded (/etc/systemd/system/pm2.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: https://pm2.keymetrics.io/
```

The solution that proved successful was to enable the new service and deactivate the other:
```
sudo systemctl disable pm2.service
sudo systemctl enable pm2-ama.service
sudo reboot
```

Verify and confirm everything looks good now:
```
$ systemctl cat pm2-ama.service
# /etc/systemd/system/pm2-ama.service
[Unit]
Description=PM2 process manager
Documentation=https://pm2.keymetrics.io/
After=network.target

[Service]
Type=forking
User=ama
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
Environment=PATH=/home/ama/bin:/home/ama/.local/bin:/home/ama/.nvm/versions/node/v12.18.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/java-8-oracle/bin:/usr/lib/jvm/java-8-orac
Environment=PM2_HOME=/home/ama/.pm2
PIDFile=/home/ama/.pm2/pm2.pid
Restart=on-failure

ExecStart=/home/ama/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2 resurrect
ExecReload=/home/ama/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2 reload all
ExecStop=/home/ama/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2 kill

[Install]
WantedBy=multi-user.target
```

This fixed the problem, and the application runs now with pm2 in `cluster` mode.


It's been a while since I did this kind of "operations" investigation, but this can be as fun.

## References


