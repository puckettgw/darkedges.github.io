---
id: 216
title: Experiences with Packer and Google Compute
date: 2016-10-28T13:15:36+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=216
permalink: /2016/10/28/experiences-with-packer-and-google-compute/
categories:
  - Google Compute
  - Packer
---
I am using Packer to build some Google Computer Images for a new project and I came undone a number of times whilst setting up my Packer file.

The first issue I hit was this
```
Build 'googlecompute' errored: google: could not find default credentials.
See https://developers.google.com/accounts/docs/application-default-credentials for more information.
```

Turns out I had downloaded the wrong JSON file, I originally downloaded the JSON file from `API Manager`. Instead create the Service Account JSON file under `IAM &Admin / Service Accounts/ Compute Engine default service account` by selecting `Create key` from the hamburger on the right.

The next issue I hit was
```
==> googlecompute: Error waiting for SSH: ssh: handshake failed: ssh: unable to authenticate, attempted methods [none publickey], no supported methods remain
```

Turns out I needed to specify `"ssh_username": "darkedges"` in my Packer file to specify a user to log in with.

Next I received this error when installing Ansible via a script.

```
    googlecompute: sudo: sorry, you must have a tty to run sudo
```

After a quick search I found http://ilostmynotes.blogspot.com.au/2015/08/provisioning-rhel-gce-machine-with.html where it seems Centos/RHEL requires a tty to work. The post recommended that I add `"ssh_pty": "true"`.

Finally I was able to build an Image.

My Packer File looked like https://gist.github.com/darkedges/911a4a6506f0aad19cd613fdebf38d32