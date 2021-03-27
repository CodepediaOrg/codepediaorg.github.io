---
layout: post
title: Fixing Invalid US-ASCII character "\xE2" on line 54 on MacOS
description: "Shows a solution to fix the infamous Jekyll encoding issue on MacOS."
author: ama
permalink: /ama/how-to-fix-invalid-us-ascii-character-xe2-on-line-54-on-macos
published: true
categories: [article]
tags: [fix, jekyll, encoding, macos]
---

In this post I will show how to fix the infamous Jekyll encoding exception on MacOS:
```
Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss':
Invalid US-ASCII character "\xE2" on line 54
```

Well I got inspired from the solution presented here[^1] when starting a cloud image.
[^1]: <https://github.com/mmistakes/minimal-mistakes/issues/1183>

Instead of starting the server for local development with the usual command:

```shell
bundle exec jekyll serve
```

I use the following environment variables before the command:

```shell
LANG="en_US.UTF-8" LC_ALL="en_US.UTF-8" bundle exec jekyll serve
```

To make the solution permanent, I added the environment variables in my `.bash_profile` file:

```shell
# lang variables
export LC_ALL="en_US.UTF-8"
export LANG="en_US.UTF-8"
export LANGUAGE="en_US.UTF-8"
```

## References
