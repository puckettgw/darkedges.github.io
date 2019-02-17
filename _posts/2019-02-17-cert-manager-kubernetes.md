---
id: 39
title: "Installing cert-manager on Kubenetes with CloudFlare DNS"
date: 2019-02-17T16:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=39
permalink: /2019/02/17/cert-manager-kubernetes-cloudflare-dns/
categories:
  - coreos
  - cert-manager
  - kubernetes
  
---
The following is a quick start quide to deploying [cert-manager](https://docs.cert-manager.io/en/latest/getting-started/install.html#verifying-the-installation) on a single node CoreOS Kubernetes instance.

You will need to ensure that you have followed the instructions at [2019/02/17/cert-manager-failing-to-start/](2019/02/17/cert-manager-failing-to-start/) to get CoreOS Kubenetes configured correctly.

<!-- more -->

## Base Install

```bash
kubectl create namespace cert-manager
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/00-crds.yaml
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/cert-manager.yaml --validate=false
```

You can confirm it configure correctly via the following

1. Pods are running

    ```bash
    kubectl get pods -n cert-manager
    ```

    should return similair to

    ``` bash
    NAME                                    READY     STATUS      RESTARTS   AGE
    cert-manager-688b97b9f4-n7b5q           1/1       Running     0          1m
    cert-manager-webhook-859cfcbc57-4sw42   1/1       Running     0          1m
    cert-manager-webhook-ca-sync-mhzn9      0/1       Completed   0          51s
    ````

1. Credentials installed

    ```bash
    kubectl get crd | grep certmanager
    ```

    should return similair to

    ```bash
    certificates.certmanager.k8s.io     3h
    challenges.certmanager.k8s.io       3h
    clusterissuers.certmanager.k8s.io   3h
    issuers.certmanager.k8s.io          3h
    orders.certmanager.k8s.io           3h
    ```

2. Certificates and Issues installed

    ``` bash
    kubectl get issuer,certificate -n cert-manager
    ```

    should return similair to

    ```bash
    NAME                                                      AGE
    issuer.certmanager.k8s.io/cert-manager-webhook-ca         3m
    issuer.certmanager.k8s.io/cert-manager-webhook-selfsign   3m

    NAME                                                              AGE
    certificate.certmanager.k8s.io/cert-manager-webhook-ca            3m
    certificate.certmanager.k8s.io/cert-manager-webhook-webhook-tls   3m
    ```

## Create the CloudFlare DNS issuer

1. Get your CloudFlare Global API Key from the CloudFlare Dashboard
   - https://dash.cloudflare.com/
   - Select Domain
   - Click `Get your API key` link
   - Click `View` next to `Global API Key`
   - Enter your details and pass the I am not a robot challenge and click `View`

2. Issue the following to generate your CloudFlare API Key Secret

    ```bash
    API_KEY=$(echo xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx | base64 -)
   cat <<EOF | kubectl apply -f -
   ---
   apiVersion: v1
    kind: Secret
    metadata:
      name: cloudflare-api-key
      namespace: cert-manager
    type: Opaque
    data:
      api-key.txt: ${API_KEY}
   EOF
   ```

3. Create the `ClusterIssuer` configuration

    ```bash
    EMAIL_ADDRESS="xxxx@xxxx.xxx"
    cat <<EOF | kubectl apply -f -
    ---
    apiVersion: certmanager.k8s.io/v1alpha1
    kind: ClusterIssuer
    metadata:
      name: letsencrypt-staging
    spec:
      acme:
        server: https://acme-staging-v02.api.letsencrypt.org/directory
        email: ${EMAIL_ADDRESS}
        privateKeySecretRef:
          name: letsencrypt-staging
        dns01:
          providers:
            - name: cf-dns
              cloudflare:
                email: ${EMAIL_ADDRESS}
                apiKeySecretRef:
                  name: cloudflare-api-key
                  key: api-key.txt
    EOF
    ```

4. Create the Test Certficate

   ```bash
   DOMAIN_NAME="xxxxx.xxxxx.xxx"
   cat <<EOF | kubectl apply -f -
   ---
   apiVersion: certmanager.k8s.io/v1alpha1
   kind: Certificate
   metadata:
     name: $(echo $DOMAIN_NAME | tr . -)
     namespace: cert-manager
   spec:
     secretName: $(echo $DOMAIN_NAME | tr . -)
     issuerRef:
       name: letsencrypt-staging
       kind: ClusterIssuer
     commonName: '${DOMAIN_NAME}'
     dnsNames:
     - ${DOMAIN_NAME}
     acme:
       config:
       - dns01:
           provider: cf-dns
         domains:
           - ${DOMAIN_NAME}
   EOF
   ```

5. Confirm it has been created (may take a minute or 2 to generate) by issuing

   ```bash
   kubectl describe certificate $(echo $DOMAIN_NAME | tr . -) -n cert-manager
   ```

   should return similair to

   ```text
    Name:         xxxx-xxxx-xxx
    Namespace:    cert-manager
    Labels:       <none>
    Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"certmanager.k8s.io/v1alpha1","kind":"Certificate","metadata":{"annotations":{},"name":"xxxx-xxxx-xxx","namespace":"cert-...
    API Version:  certmanager.k8s.io/v1alpha1
    Kind:         Certificate
    Metadata:
      Cluster Name:
      Creation Timestamp:  2019-02-16T22:44:17Z
      Generation:          1
      Resource Version:    3053428
       Self Link:           /apis/certmanager.k8s.io/v1alpha1/namespaces/cert-manager/certificates/xxxx-xxxx-xxx
      UID:                 649849bf-323c-11e9-9371-00237d495614
    Spec:
      Acme:
        Config:
        Dns 01:
            Provider:  cf-dns
        Domains:
            xxxx.xxxx.xxx
      Common Name:  xxxx.xxxx.xxx
      Dns Names:
        xxxx.xxxx.xxx
      Issuer Ref:
        Kind:       ClusterIssuer
        Name:       letsencrypt-staging
      Secret Name:  xxxx-xxxx-xxx
    Status:
      Conditions:
        Last Transition Time:  2019-02-16T22:45:37Z
        Message:               Certificate is up to date and has not expired
        Reason:                Ready
        Status:                True
        Type:                  Ready
      Not After:               2019-05-17T21:45:36Z
    Events:
      Type     Reason         Age                From          Message
      ----     ------         ----               ----          -------
      Normal   OrderCreated   43m                cert-manager  Created Order resource "xxxx-xxxx-xxx-2862881673"
      Normal   OrderComplete  42m                cert-manager  Order "xxxx-xxxx-xxx-2862881673" completed successfully
      Normal   CertIssued     42m                cert-manager  Certificate issued successfully
      Warning  BadConfig      10m (x2 over 10m)  cert-manager  Resource validation failed: spec: Invalid value: "no issuer specified for Issuer '/letsencrypt-staging'": no issuer specified for Issuer '/letsencrypt-staging'

   ```

## Remove

```bash
kubectl delete -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/cert-manager.yaml
kubectl delete -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/00-crds.yaml
kubectl delete namespace cert-manager
```