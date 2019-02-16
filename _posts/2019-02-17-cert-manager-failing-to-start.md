---
id: 38
title: "cert-manager failing to start"
date: 2019-02-17T16:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=39
permalink: /2019/02/17/cert-manager-failing-to-start/
categories:
  - coreos
  - cert-manager
  
---
Whilst verifying [cert-manager](https://docs.cert-manager.io/en/latest/getting-started/install.html#verifying-the-installation) I was finding it was crashing.

<!-- more -->

```bash
kubectl get pods -n cert-manager
```

returns

```bash
NAME                                    READY     STATUS             RESTARTS   AGE
cert-manager-688b97b9f4-jdlkf           1/1       Running            0          41m
cert-manager-webhook-859cfcbc57-z4tcf   0/1       CrashLoopBackOff   12         41m
cert-manager-webhook-ca-sync-rnhhp      0/1
kubectl log cert-manager-webhook-859cfcbc57-z4tcf -n cert-manager
```

returns

```bash
Error: cluster doesn't provide requestheader-client-ca-file
Usage:
   [flags]

Flags:

   :

F0216 20:41:33.345307       1 cmd.go:42] cluster doesn't provide requestheader-client-ca-file

```

resolved by adding

```bash
    - --requestheader-client-ca-file=/etc/kubernetes/ssl/ca.pem
    - --requestheader-allowed-names=aggregator
    - --requestheader-extra-headers-prefix=X-Remote-Extra-
    - --requestheader-group-headers=X-Remote-Group
    - --requestheader-username-headers=X-Remote-User
```

to `/etc/kubernetes/manifests/kube-apiserver.yml`

```bash
kubectl get pods -n cert-manager
```

now returns (after deleteing the pod)

```bash
NAME                                    READY     STATUS        RESTARTS   AGE
cert-manager-688b97b9f4-jdlkf           1/1       Running       0          1h
cert-manager-webhook-859cfcbc57-sjrdz   1/1       Running       0          9s
cert-manager-webhook-859cfcbc57-z4tcf   0/1       Terminating   16         1h
cert-manager-webhook-ca-sync-rnhhp      0/1       Completed     0          1h

```

Not sure what consequences this has, but so far no further issues with my Kubernetes instance