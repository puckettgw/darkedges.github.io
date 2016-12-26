---
id: 155
title: Create a wallet for OHS
date: 2015-08-14T18:25:37+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=155
permalink: /2015/08/14/create-a-wallet-for-ohs/
categories:
  - OHS
  - Oracle
---
As the CA User

[code]
keytool -v -importkeystore -srckeystore wildcard.darkedges.vm.p12 -srcstoretype PKCS12 -destkeystore wildcard.darkedges.vm.jks -deststoretype JKS
keytool -import -alias Root -keystore wildcard.darkedges.vm.jks -trustcacerts -file /etc/pki/CA/certs/ca.cert.pem
keytool -import -alias Intermediate -keystore wildcard.darkedges.vm.jks -trustcacerts -file /etc/pki/CA/intermediate/certs/intermediate.cert.pem keytool -list -keystore wildcard.darkedges.vm.jks &lt;/span&gt;&lt;/p&gt;
[/code]

<!-- more --> 

As the oracle user

[code]
cd /u01/app/oracle/shared/instances/instance1/config/OHS/ohs1/keystores/darkedges/
orapki wallet create -wallet ./ -pwd Passw0rd -auto_login
orapki wallet jks_to_pkcs12 -wallet ./ -pwd Passw0rd -keystore /tmp/wildcard/wildcard.darkedges.vm.jks -jkspwd Passw0rd[/code]
