---
id: 30
title: 'Babun, Jekyll and Windows Ruby'
date: 2016-12-11T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=31
permalink: /2016/12/21/babun-jekyll/
categories:
  - babun
  - jekyll

---

I had installed `Jekyll` via `git bash` and it was working fine. I tried to run it via `Babun`
and was getting the following

```bash
C:\Ruby23-x64\bin\ruby.exe: No such file or directory -- /cygdrive/c/Ruby23-x64/bin/jekyll (LoadError)
```

<!-- more -->

## How to fix

To solve the issue I had to add the following

```bash
alias ruby='/cygdrive/c/Ruby23-x64/bin/jekyll' 
alias gem='/cygdrive/c/Ruby23-x64/bin/gem.cmd' 
alias jekyll='/cygdrive/c/Ruby23-x64/bin/jekyll.bat'
alias irb='/cygdrive/c/Ruby21-x64/bin/irb.bat' 
```

