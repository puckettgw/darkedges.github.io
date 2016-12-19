---
id: 31
title: 'Ansible Password Salt Generation'
date: 2016-12-11T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=31
permalink: /2016/12/19/ansible-salt-password/
categories:
  - Jekyll
tags:
  - Ansible
---

I need to create a Password Salt for Ansible User Creation and so used the following command.

<!-- more -->

```bash
python -c 'import crypt; import getpass; print crypt.crypt(getpass.getpass(), "$1$SomeSalt$")'
```
