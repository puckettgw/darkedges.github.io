---
id: 202
title: typings WARN deprecated
date: 2016-10-05T12:20:29+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=202
permalink: /2016/10/05/typings-warn-deprecated/
categories:
  - Angular
  - typings
---
I was getting the following when running `typings install`

<!-- more --> 

```
typings WARN deprecated 9/29/2016: "registry:dt/node#6.0.0+20160621231320" is deprecated (updated, replaced or removed)
```

It would appear that I can find the correct version by issuing the following command

```
typings info dt~node --versions
```

This gives a list of versions as follows

```
TAG                    VERSION DESCRIPTION COMPILER LOCATION
                                                                         UPDATED

6.0.0+20160928143418   6.0.0                        github:DefinitelyTyped/DefinitelyTyped/node/node.d.ts#7be68adbdff4bea14f33f54f27d3bdb5822e3b10       2016-09-28T14:34:18.000Z
0.12.0+20160906152630  0.12.0                       github:DefinitelyTyped/DefinitelyTyped/node/node-0.12.d.ts#03f3ca4333161dea8c5fa0ba47d55b02da92d40d  2016-09-06T15:26:30.000Z
0.11.13+20160906152630 0.11.13                      github:DefinitelyTyped/DefinitelyTyped/node/node-0.11.d.ts#03f3ca4333161dea8c5fa0ba47d55b02da92d40d  2016-09-06T15:26:30.000Z
0.10.1+20160906152630  0.10.1                       github:DefinitelyTyped/DefinitelyTyped/node/node-0.10.d.ts#03f3ca4333161dea8c5fa0ba47d55b02da92d40d  2016-09-06T15:26:30.000Z
0.8.8+20160906152630   0.8.8                        github:DefinitelyTyped/DefinitelyTyped/node/node-0.8.8.d.ts#03f3ca4333161dea8c5fa0ba47d55b02da92d40d 2016-09-06T15:26:30.000Z
```

All you need is the correct `TAG` to use in `typings.json`