---
id: 30
title: 'Deploying helm on Windows 7'
date: 2016-12-21T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=31
permalink: /2016/12/21/helm-windows/
categories:
  - windows
  - helm

---

This quick-start guide will help deploy `helm` on Windows 7.

<!-- more -->

## Install helm 

``` bash
curl -LO http://storage.googleapis.com/kubernetes-helm/helm-v2.1.2-windows-amd64.zip \
    && unzip helm-v2.1.2-windows-amd64.zip \
    && mv windows-amd64/helm.exe /usr/bin/
helm version
```

should return

```bash
Client: &version.Version{SemVer:"v2.1.2", GitCommit:"58e545f47002e36ca71ac5d1f7a987b56e1937b3", GitTreeState:"clean"}
```

## Deploy helm

Ensure minikube is up and running and issue the following command

```bash
helm init
```

## Conclusion

That is all that is needed to get `helm` running on Windows 7.
