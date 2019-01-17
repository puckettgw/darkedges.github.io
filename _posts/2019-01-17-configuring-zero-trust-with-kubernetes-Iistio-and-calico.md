---
id: 38
title: "Configuring Zero Trust with Kubernetes, Istio and Calico"
date: 2019-01-17T16:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=38
permalink: /2019/01/17/configuring-zero-trust-with-kubernetes-Iistio-and-calico/
categories:
  - coreos
  - istio
  - calico
  
---

Taken the various guides for deploying Calico and Istio on Kubernetes to generate this one pager. It includes a sample application from Istio converted to use Calico.

<!-- more -->

# Zero Trust Networking with Kuberenets, Istio and Calico

This has been honed over a couple of days as I found some of the tutorials a little hard to get working. This should use my [CoreOS Single Node Cluster](/2019/01/17/configuring-kubernets-single-node-cluster-with-coreos/) guide as a foundation as it has been updated to support the nuances of that platform.

## Installing Calico for policy and flannel for networking

```bash
curl https://docs.projectcalico.org/v3.4/getting-started/kubernetes/installation/hosted/canal/canal.yaml -O
kubectl apply -f canal.yaml
```

## Enabling application layer policy

```bash
curl https://docs.projectcalico.org/v3.4/getting-started/kubernetes/installation/manifests/app-layer-policy/kubernetes-datastore/flannel/calico-node.yaml -O
sed -i -e "s#/usr/libexec/kubernetes/kubelet-plugins/volume/exec/#/var/lib/kubelet/volumeplugins/#g" calico-node.yaml
kubectl apply -f calico-node.yaml
```

## Installing Istio

```bash
curl -L https://git.io/getLatestIstio | sh -
export ISTIO_DIR=$(ls -d istio-*)
kubectl apply -f ${ISTIO_DIR}/nstall/kubernetes/istio-demo-auth.yaml
```

## Updating the Istio sidecar injector

```bash
kubectl apply -f https://docs.projectcalico.org/v3.4/getting-started/kubernetes/installation/manifests/app-layer-policy/istio-inject-configmap.yaml
```

## Adding Calico authorization services to the mesh

```bash
kubectl apply -f https://docs.projectcalico.org/v3.4/getting-started/kubernetes/installation/manifests/app-layer-policy/istio-app-layer-policy.yaml
```

## Deploy calicoctl

```bash
sudo mkdir -p /etc/calico
sudo cat > /etc/calico/calicoctl.cfg <<EOF
apiVersion: projectcalico.org/v3
kind: CalicoAPIConfig
metadata:
spec:
  datastoreType: "kubernetes"
  kubeconfig: "/home/core/.kube/config"
EOF
```

## Adding namespace labels

```bash
kubectl create ns policy-demo
kubectl label namespace policy-demo istio-injection=enabled
kubectl get namespace -L istio-injection
```

## Deploying bookinfo application

[https://istio.io/docs/examples/bookinfo/#deploying-the-application]{https://istio.io/docs/examples/bookinfo/#deploying-the-application}

```bash
kubectl apply -f ${ISTIO_DIR}/samples/bookinfo/platform/kube/bookinfo.yaml -n policy-demo
kubectl apply -f ${ISTIO_DIR}/samples/bookinfo/networking/bookinfo-gateway.yaml -n policy-demo
```

## Apply default destination rules

[https://istio.io/docs/examples/bookinfo/#apply-default-destination-rules](https://istio.io/docs/examples/bookinfo/#apply-default-destination-rules)

```bash
kubectl apply -f ${ISTIO_DIR}/samples/bookinfo/networking/destination-rule-all-mtls.yaml -n policy-demo
```

## Apply a virtual service

[https://istio.io/docs/tasks/traffic-management/request-routing/#apply-a-virtual-service](https://istio.io/docs/tasks/traffic-management/request-routing/#apply-a-virtual-service)

```bash
kubectl apply -f ${ISTIO_DIR}/samples/bookinfo/networking/virtual-service-all-v1.yaml -n policy-demo
```

## Route based on user identity

[https://istio.io/docs/tasks/traffic-management/request-routing/#route-based-on-user-identity](https://istio.io/docs/tasks/traffic-management/request-routing/#route-based-on-user-identity)

```bash
kubectl apply -f ${ISTIO_DIR}/samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml -n policy-demo
```

## Configure Zero Trust

### Configure Application Ingress

```bash
kubectl create -n policy-demo -f https://docs.projectcalico.org/v3.4/getting-started/kubernetes/tutorials/stars-policy/policies/default-deny.yaml
/home/core/calicoctl apply --namespace policy-demo -f - <<EOF
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: summary
spec:
  selector: app == 'productpage'
  ingress:
   - action: Allow
  egress:
    - action: Allow
EOF
```

### Configuire via Istio Policy

the following istio `NetworkPolicy`

```bash
kubectl apply -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-details
  namespace:  policy-demo
spec:
  podSelector:
    matchLabels:
      app: details
  ingress:
    - from:
      - podSelector:
          matchLabels:
            app: productpage
EOF

kubectl apply -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-reviews
  namespace:  policy-demo
spec:
  podSelector:
    matchLabels:
      app: reviews
  ingress:
    - from:
      - podSelector:
          matchLabels:
            app: productpage
EOF

kubectl apply -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-ratings
  namespace:  policy-demo
spec:
  podSelector:
    matchLabels:
      app: ratings
  ingress:
    - from:
      - podSelector:
          matchLabels:
            app: reviews
EOF
```

### Configuire via Calico Policy

can be replaced with the Calico `NetworkPolicy`

```bash
/home/core/calicoctl apply --namespace policy-demo -f - <<EOF
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: productpage-allow-tcp-9080
  namespace: policy-demo
spec:
  selector: app in { 'details', 'reviews' }
  types:
  - Ingress
  - Egress
  ingress:
  - action: Allow
    protocol: TCP
    source:
      selector: app == 'productpage'
    destination:
      ports:
      - 9080
  egress:
  - action: Allow
EOF

/home/core/calicoctl apply --namespace policy-demo -f - <<EOF
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: reviews-allow-tcp-9080
  namespace: policy-demo
spec:
  selector: app == 'ratings'
  types:
  - Ingress
  - Egress
  ingress:
  - action: Allow
    protocol: TCP
    source:
      selector: app == 'reviews'
    destination:
      ports:
      - 9080
  egress:
  - action: Allow
EOF
```

## Determining the ingress IP and port

[https://istio.io/docs/examples/bookinfo/#determining-the-ingress-ip-and-port](https://istio.io/docs/examples/bookinfo/#determining-the-ingress-ip-and-port)

```bash
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

## Confirm the app is running

[https://istio.io/docs/examples/bookinfo/#confirm-the-app-is-running](https://istio.io/docs/examples/bookinfo/#confirm-the-app-is-running)

```bash
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
```