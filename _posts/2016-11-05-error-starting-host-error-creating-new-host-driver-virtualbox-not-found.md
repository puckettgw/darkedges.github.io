---
id: 245
title: 'Error starting host: Error creating new host: Driver &#8220;virtualbox&#8221; not found.'
date: 2016-11-05T13:50:12+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=245
permalink: /2016/11/05/error-starting-host-error-creating-new-host-driver-virtualbox-not-found/
categories:
  - kubernetes
tags:
  - kubernetes
---
I was receiving the following error when using `minikube start`
```bash
Starting local Kubernetes cluster...
E1106 07:39:29.894515    4616 start.go:92] Error starting host: Error creating new host: Driver "virtualbox" not found. Do you have the plugin binary accessible in your PATH?. Retrying.
E1106 07:39:29.919517    4616 start.go:98] Error starting host:  Error creating new host: Driver "virtualbox" not found. Do you have the plugin binary accessible in your PATH?
================================================================================
An error has occurred. Would you like to opt in to sending anonymized crash
information to minikube to help prevent future errors?
To opt out of these messages, run the command:
        minikube config set WantReportErrorPrompt false
================================================================================
```

Turns out I had named the file `minikube` instead of `minikube.exe`

Thanks to [https://github.com/kubernetes/minikube/issues/458](https://github.com/kubernetes/minikube/issues/458) for the answer.