---
id: 30
title: 'Deploying minikube on Windows 7'
date: 2016-12-21T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=31
permalink: /2016/12/21/minikube-windows/
categories:
  - windows
  - minikube

---

This quick-start guide will help deploy `minikube` on Windows 7.
__**Note:**__ This should work on later versions, but you need to
disable Hyper-V.

<!-- more -->

## Install Chocolatey

Open a PowerShell window as an Administrator and issue the following command

``` PowerShell
powershell.exe -executionpolicy unrestricted -command 'iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex'
```

Close down the PowerShell window.

## Install Babun / VirtualBox / Docker

Open a PowerShell window as an Administrator and issue the following command

``` PowerShell
choco install babun
choco install virtualbox
choco install docker
```

**Note:** Ensure that `C:\Program Files\Oracle\VirtualBox` has been added to the Path.
Open a Powershell and issue

``` PowerShell
vboxmanage
```

If it comes back with

```PowerShell
vboxmanage : The term 'vboxmanage' is not recognized as the name of a cmdlet, function, script file, or operable prog
ram. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ vboxmanage2
+ ~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (vboxmanage2:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

Then use the following to add it to the Path

```PowerShell
$oldpath = (Get-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH).path
$newpath = "$oldpath;C:\Program Files\Oracle\VirtualBox"
Set-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH –Value $newPath
```

## Enable right click to paste in vi/vim for Babun

Open Babun and issue the following command

```Bash
echo "set mouse-=a" >> ~/.vimrc
```

## Install minikube

Open Babun and issue the following command

```bash
curl -Lo minikube.exe https://storage.googleapis.com/minikube/releases/v0.14.0/minikube-windows-amd64.exe \
    && chmod +x minikube.exe \
    && mv minikube.exe /usr/bin/
minikube version
```

should return

```
minikube version: v0.14.0
```

## Install kubectl

Open Babun and issue the following command

``` bash
curl -LO curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/windows/amd64/kubectl.exe \
    && chmod +x kubectl.exe \
    && mv kubectl.exe /usr/bin/
kubectl version
```

should return

```
Client Version: version.Info{Major:"1", Minor:"5", GitVersion:"v1.5.1", GitCommit:"82450d03cb057bab0950214ef122b67c83fb11df", GitTreeState:"clean", BuildDate:"2016-12-14T00:57:05Z", GoVersion:"go1.7.4", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"5", GitVersion:"v1.5.1", GitCommit:"82450d03cb057bab0950214ef122b67c83fb11df", GitTreeState:"clean", BuildDate:"1970-01-01T00:00:00Z", GoVersion:"go1.7.1", Compiler:"gc", Platform:"linux/amd64"}
```

where `GitVersion` is the same as 

``` bash
curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt
```


## Start minikube and configure Docker Environment

Open Babun and issue the following command

```bash
minikube start
eval $(minikube docker-env)
```

Confirm it has started and configured correctly

``` bash
docker ps
```

Should return similar to

```
CONTAINER ID        IMAGE                                                        COMMAND                  CREATED             STATUS              PORTS               NAMES
cd7ff8b578b7        gcr.io/google_containers/kubernetes-dashboard-amd64:v1.5.0   "/dashboard --port=90"   4 minutes ago       Up 4 minutes                            k8s_kubernetes-dashboard.e2f5dab8_kubernetes-dashboard-t113s_kube-system_df60faf4-c7cb-11e6-b337-f2761b47e9cf_8cf97924
81a5657b7530        gcr.io/google_containers/pause-amd64:3.0                     "/pause"                 4 minutes ago       Up 4 minutes                            k8s_POD.2225036b_kubernetes-dashboard-t113s_kube-system_df60faf4-c7cb-11e6-b337-f2761b47e9cf_75b0f932
05a770116263        gcr.io/google-containers/kube-addon-manager:v6.1             "/opt/kube-addons.sh"    4 minutes ago       Up 4 minutes                            k8s_kube-addon-manager.96c28b3c_kube-addon-manager-minikube_kube-system_014fb8f91f3d52450a942179a984bc15_b579f2e5
97d6ddeedc67        gcr.io/google_containers/exechealthz-amd64:1.2               "/exechealthz '--cmd="   5 minutes ago       Up 5 minutes                            k8s_healthz.31e3f95_kube-dns-v20-q4005_kube-system_535b1f98-aa9d-11e6-a125-6abe3fb03862_6a85271a
48b566268b9e        gcr.io/google_containers/kube-dnsmasq-amd64:1.4              "/usr/sbin/dnsmasq --"   5 minutes ago       Up 5 minutes                            k8s_dnsmasq.bea311d9_kube-dns-v20-q4005_kube-system_535b1f98-aa9d-11e6-a125-6abe3fb03862_63e40afd
ab667e03d615        gcr.io/google_containers/kubedns-amd64:1.8                   "/kube-dns --domain=c"   5 minutes ago       Up 5 minutes                            k8s_kubedns.ec18573d_kube-dns-v20-q4005_kube-system_535b1f98-aa9d-11e6-a125-6abe3fb03862_9d57df9d
8ac253463b99        gcr.io/google_containers/kubernetes-dashboard-amd64:v1.4.2   "/dashboard --port=90"   5 minutes ago       Up 5 minutes                            k8s_kubernetes-dashboard.e7acdab9_kubernetes-dashboard-fe9qz_kube-system_535a9ebe-aa9d-11e6-a125-6abe3fb03862_8a8bc52f
1d0b30133560        gcr.io/google_containers/pause-amd64:3.0                     "/pause"                 5 minutes ago       Up 5 minutes                            k8s_POD.d8dbe16c_kube-addon-manager-minikube_kube-system_014fb8f91f3d52450a942179a984bc15_a77a82be
7973925b9536        gcr.io/google_containers/pause-amd64:3.0                     "/pause"                 5 minutes ago       Up 5 minutes                            k8s_POD.2225036b_kubernetes-dashboard-fe9qz_kube-system_535a9ebe-aa9d-11e6-a125-6abe3fb03862_8a6c70ab
69cc7e5e2b77        gcr.io/google_containers/pause-amd64:3.0                     "/pause"                 5 minutes ago       Up 5 minutes                            k8s_POD.a6b39ba7_kube-dns-v20-q4005_kube-system_535b1f98-aa9d-11e6-a125-6abe3fb03862_ed4eb26d
```

## Conclusion

That is all that is needed to get `minikube` running on Windows 7.
