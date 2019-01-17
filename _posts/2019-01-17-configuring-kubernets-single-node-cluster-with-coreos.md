---
id: 37
title: "Configuring Kubernetes Single Node Cluster With CoreOS"
date: 2019-01-17T16:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=37
permalink: /2019/01/17/configuring-kubernets-single-node-cluster-with-coreos/
categories:
  - coreos
  - istio
  - calico
  
---

Very rough guide to deploying a Kubernetes Single Node Cluster on CoreOS. I leave the actual depoyment of CoreOS to the reader as I am not 100% sure I have delivered that properly, but it works for me.


<!-- more -->

1. Connect to your CoreOS instance via SSH
1. Issue the following commands to start the process
   
   ```bash
   git clone https://github.com/m3adow/k8single/; 
   cd k8single
   ./kubeform.sh [myip-address]
   ```

I further enhanced my instance by adding additional IP Address and [MetalLB](https://metallb.universe.tf/)


## Static IP Range

The following will generate the Static IP Addresses needed to use MetalLB

```bash
sudo su -
cat > /etc/systemd/network/static.network << EOF
[Match]
Name=enp0s25

[Network]
Address=10.0.0.36/24
Address=10.0.0.240/24
Address=10.0.0.241/24
Address=10.0.0.242/24
Address=10.0.0.243/24
Address=10.0.0.244/24
Address=10.0.0.245/24
Address=10.0.0.246/24
Address=10.0.0.247/2
Address=10.0.0.248/24
Address=10.0.0.249/24
Address=10.0.0.250/24
Gateway=10.0.0.1
DNS=10.0.0.1
EOF
systemctl daemon-reload
systemctl restart network
```

## MetalLB

MetalLB is a load-balancer implementation for bare metal Kubernetes clusters, using standard routing protocols.

## Installation
[https://metallb.universe.tf/installation/#installation-with-kubernetes-manifests](https://metallb.universe.tf/installation/#installation-with-kubernetes-manifests)

```bash
https://metallb.universe.tf/installation/#installation-with-kubernetes-manifests
```

### Configuration

[https://metallb.universe.tf/configuration/#layer-2-configuration](https://metallb.universe.tf/configuration/#layer-2-configuration)

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 10.0.0.240-10.0.0.250
EOF
```