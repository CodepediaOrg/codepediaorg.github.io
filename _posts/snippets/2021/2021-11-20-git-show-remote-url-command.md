---
layout: post
title: Git show remote url command
description: "Git show remote url command code snippet"
author: ama
permalink: /snippets/6194ba49fb0221040e48b6c5/git-show-remote-url-command
published: true
snippetId: 6194ba49fb0221040e48b6c5
categories: [snippets]
tags: [git, shell, terminal, codever-snippets]
---

The simplest way

```shell
git remote -v

#result something similar to the following
origin  git@github.com:CodepediaOrg/codepediaorg.github.io.git (fetch)
origin  git@github.com:CodepediaOrg/codepediaorg.github.io.git (push)
```

Tip to get only the remote URL:



```shell
git config --get remote.origin.url

#result something similar to the following
git@github.com:CodepediaOrg/codepediaorg.github.io.git
```

In order to get more details about a particular remote, use the `git remote show [remote-name] command`

```shell
git remote show origin # here "origin" is the remote name

#result something similar to the following
* remote origin
  Fetch URL: git@github.com:CodepediaOrg/codepediaorg.github.io.git
  Push  URL: git@github.com:CodepediaOrg/codepediaorg.github.io.git
  HEAD branch: master
  Remote branches:
    _posts/2016-08-26-migrate-rest-api-from-jboss-eap6-to-eap7 tracked
    add_multiple_authors_functionality                         tracked
    dependabot/npm_and_yarn/bl-1.2.3                           tracked
    dependabot/npm_and_yarn/grunt-1.3.0                        tracked
    dependabot/npm_and_yarn/hosted-git-info-2.8.9              tracked
    dependabot/npm_and_yarn/ini-1.3.7                          tracked
    dependabot/npm_and_yarn/is-svg-4.3.1                       tracked
    dependabot/npm_and_yarn/lodash-4.17.21                     tracked
    dependabot/npm_and_yarn/path-parse-1.0.7                   new (next fetch will store in remotes/origin)
    dependabot/npm_and_yarn/websocket-extensions-0.1.4         tracked
    feature/add-copy-button-for-code-snippets                  tracked
    feature/add-scroll-up-for-posts                            tracked
    feature/mongo-commands-via-collections                     tracked
    gh-pages                                                   tracked
    master                                                     tracked
    post/multiple-authors-jekyll                               tracked
    refs/remotes/origin/dependabot/npm_and_yarn/lodash-4.17.19 stale (use 'git remote prune' to remove)
    run-grunt                                                  tracked
  Local branches configured for 'git pull':
    master    merges with remote master
    run-grunt merges with remote run-grunt
  Local refs configured for 'git push':
    master    pushes to master    (up to date)
    run-grunt pushes to run-grunt (up to date)
```



<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6194ba49fb0221040e48b6c5" %}
