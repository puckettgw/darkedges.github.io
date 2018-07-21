---
id: 35
title: "'default backend - 404' when using minikube and TLS for ingress"
date: 2018-07-22T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=35
permalink: /2018/07/22/default-backend-404/
categories:

- minikube
  
---

I was having an issue getting my minikube ingress controller working, as it was returning a `default backend - 404` when I hit the `https://` URL. Turns out a configuration that was working with nginx, does not necessarily work with ingress-nginx

<!-- more -->

At first I found this [https://github.com/kubernetes/minikube/issues/1701](https://github.com/kubernetes/minikube/issues/1701) and tried everything in there, but it still did not solve my problem.

Next I found this [https://stackoverflow.com/questions/49545732/how-to-get-kubernetes-ingress-to-terminate-ssl-and-proxy-to-service](https://stackoverflow.com/questions/49545732/how-to-get-kubernetes-ingress-to-terminate-ssl-and-proxy-to-service) and this told me to go and check the ingress controller logs. 

One command later `kubectl logs -n kube-system nginx-ingress-controller-67956bf89d-5c9zx` and I found this.

```bash
W0721 22:34:37.322152       6 controller.go:1027] Validating certificate against DNS names. This will be deprecated in a future version.
W0721 22:34:37.322162       6 controller.go:1032] ssl certificate default/darkedges-com-tls does not contain a Common Name or Subject Alternative Name for host as.tpp.forgerockdev.darkedges.com. Reason: x509: certificate is valid for *.darkedges.com, darkedges.com, not as.tpp.forgerockdev.darkedges.com
W0721 22:34:37.322388       6 controller.go:1026] unexpected error validating SSL certificate default/darkedges-com-tls for host as.bank.forgerockdev.darkedges.com. Reason: x509: certificate is valid for *.darkedges.com, darkedges.com, not as.bank.forgerockdev.darkedges.com
```

So off I went to [https://kubernetes.slack.com](https://kubernetes.slack.com) joined the `ingress-nginx` channel and spoke with `@aledbf`. At first it looked liked this was not going to get fixed and I pointed to the following articles

- [https://github.com/kubernetes/ingress-nginx/issues/195](https://github.com/kubernetes/ingress-nginx/issues/195)
- [https://github.com/kubernetes/ingress-nginx/issues/1873](https://github.com/kubernetes/ingress-nginx/issues/1873)

The latter article suited me perfectly and then it dawned on me, I should go and check my Origin Certificate within CloudFlare and that is when I found out I could generate a new certificate that covered all my SAN and CN needs. 

One creation later and I had a new set of key/cert pair and I plugged it into my kubernetes secrets.

```bash
kubectl create secret tls darkedges-com-tls \
    --key c:\development\forgerock\tls\darkedges.com.key \
    --cert c:\development\forgerock\tls\darkedges.com.pem
```

I had to re-apply my ingress, and checked the ingress controller logs to see no problem and when I re-hit the `https://` URL it was being served with the CloudFlare certificate. It gave me a `502 Bad Gateway` error, but at least I was being served via the correct certificate.