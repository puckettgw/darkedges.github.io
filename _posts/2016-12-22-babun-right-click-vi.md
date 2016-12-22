---
id: 30
title: 'Babun, vi/vim and Right Click to paste'
date: 2016-12-11T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=31
permalink: /2016/12/21/babun-right-click-to-paste/
categories:
  - babun
  - vi

---

I could not Right Click to paste in `vi` / `vim` via `Babun` which was driving me crazy.

<!-- more -->

## How to fix

To solve the issue I had to add the following

```Bash
echo "set mouse-=a" >> ~/.vimrc
```

