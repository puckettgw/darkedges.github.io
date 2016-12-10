---
id: 183
title: 'error TS2307: Cannot find module &#8216;crypto-js&#8217;.'
date: 2016-10-05T11:45:55+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=183
permalink: /2016/10/05/error-ts2307-cannot-find-module-crypto-js/
categories:
  - Angular
---
I am using `cryptojs` to develop a Android TOTP generator for Ionic and as all good projects started with
```
npm install crypto-js --save
```

When I started the `tsc` process I was getting the following error
```
src/totp.component.ts(4,27): error TS2307: Cannot find module 'crypto-js'.
```

I used the following command to resolve that issue
```
typings install dt~crypto-js --global --save
```

I tried
```
npm install --save-dev @types/crypto-js
```
but this did not add the necessary details to `typings.json` that the `typings` command added.
