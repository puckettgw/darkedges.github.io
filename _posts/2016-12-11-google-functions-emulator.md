---
id: 30
title: Google Functions Emulator
date: 2016-12-11T20:48:53+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=30
permalink: /2016/12/11/2016-12-11-google-functions-emulator/
categories:
  - Google Cloud
tags:
  - Functions
---

Wanting to learn Google Cloud Functions and not waste time uploading and publishing
I tried the Google Cloud Functions Emulator within my Windows environment. Here is my
write up of how I deployed my first HTTP Function locally.

<!-- more -->

## Pre-Requisites

You need [Node.js 6.9.1](https://nodejs.org/dist/v6.9.1/node-v6.9.1-x64.msi), otherwise you will get the following error
`Node.js v6.9.1 is required to run the emulator!`
Anything later and it will not work.

## Install 'functions'

Open a command prompt and start the emulator installation

    npm install -g @google-cloud/functions-emulator


## Authenticate

*Note:* only if you need to access Google Services do you need to do this.
You need to have a valid Google Tokens configured locally, and they will be the same as he one used when deployed.

[Follow the instructions here](https://github.com/GoogleCloudPlatform/cloud-functions-emulator/blob/master/README.md#authentication)

## HTTP Hello World

Lets start by creating a simple HTTP example HelloWorld by creating a `functions` directory and adding and `index.js`
file containing the folowing.

```
/**
 * HTTP Cloud Function.
 *
 * @param {Object} req Cloud Function request context.
 * @param {Object} res Cloud Function response context.
 */
exports.helloWorldHttp = function helloWorldHttp (req, res) {
  res.send(`Hello ${req.body.name || 'World'}!`);
};
```

First lets start the emualtor

    functions start

Should return similair to

```
Starting Google Cloud Functions Emulator...
Google Cloud Functions Emulator STARTED
┌───────────────┬────────────┬────────────────────────────────────────────────────┐
│ Name          │ Type       │ Path                                               │
├───────────────┴────────────┴────────────────────────────────────────────────────┤
│ No functions deployed _\_(?)_/_.  Run "functions deploy" to deploy a function   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

Next deploy to the emulator using the following command

    functions deploy helloWorldHttp ./ --trigger-http

Should return similair to

```
Function helloWorldHttp deployed.
┌──────────┬──────────────────────────────────────────────────────────────────────┐
│ Property │ Value                                                                │
├──────────┼──────────────────────────────────────────────────────────────────────┤
│ Name     │ helloWorldHttp                                                       │
├──────────┼──────────────────────────────────────────────────────────────────────┤
│ Type     │ HTTP                                                                 │
├──────────┼──────────────────────────────────────────────────────────────────────┤
│ Path     │ C:\development\gcloud\functions                                      │
├──────────┼──────────────────────────────────────────────────────────────────────┤
│ Url      │ http://localhost:8008/helloWorldHttp                                 │
└──────────┴──────────────────────────────────────────────────────────────────────┘
```

Now request the function with the following

    curl -X POST http://localhost:8008/helloWorldHttp -H "Content-Type: application/json" -d "{\"name\": \"Test\"}"

The output should be

    Hello Test!

## Conclusion

That was a simple walkthrough of getting the Google Cloud Functions Emulator running a simple HTTP Function. You can
use it for other Functions testing as well and for further details checkout [cloud-functions-emulator](https://github.com/GoogleCloudPlatform/cloud-functions-emulator)
