---
id: 30
title: 'Babun, Jekyll and Windows Ruby'
date: 2016-12-11T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=31
permalink: /2016/12/21/babun-docker-winpty/
categories:
  - babun
  - jekyll

---

I had installed `babun` via `git bash` and it was working fine with `docker` except for when
I was trying to connect to a `bash` shell via a `run` command.

```bash
{ ~ }  » docker run -it forgerock/opendj /bin/bash
the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'
```

<!-- more -->

## How to fix

Seems that `winpty` is deployed in the home directory and so to solve the issue I had to add the following, just remember to use the correct path for your
version of docker.

```bash
alias docker="~/.winpty/winpty.exe /cygdrive/c/Program Files/Docker Toolbox/docker"
```

