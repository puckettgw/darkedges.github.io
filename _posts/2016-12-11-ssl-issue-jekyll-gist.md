---
id: 30
title: 'Jekyll Gist - Liquid Exception: SSL_connect'
date: 2016-12-11T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=30
permalink: /2016/12/11/ssl-issue-jekyll-gist/
categories:
  - Jekyll
tags:
  - Gist
  - Windows
---

Trying to use `jekyll-gist` and I encountered an issue with Windows

```bash
Liquid Exception: SSL_connect returned=1 errno=0 state=error: certificate verify failed in C:/development/github/darkedges.github.io/_posts/2016-12-11-google-functions-emulator.md
```

<!-- more -->

Turns out there is an issue with Ruby and OpenSSL using the wrong location.
The following commands resolves that issue. Just remember to add it to the Environment Variables.

```bash
curl -o /c/development/cacert.pem https://curl.haxx.se/ca/cacert.pem
export SSL_CERT_FILE="c:\development\cacert.pem"
```