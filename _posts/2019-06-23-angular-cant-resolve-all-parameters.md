---
id: 39
title: "Angular can't resolve all parameters for"
date: 2019-06-23T06:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=42
permalink: /2019/06/23/angular-cant-resolve-all-parameters/
categories:
  - angular
  - error
  
---

When I was building a production version my Angular Application I was getting the followng error

```bash
$ ng build --aot=true --buildOptimizer=true --prod=true

Date: 2019-06-22T19:56:25.883Z
Hash: 1ef627fb1316ebc9d288
Time: 11397ms
chunk {0} runtime.26209474bfa8dc87a77c.js (runtime) 1.41 kB [entry] [rendered]
chunk {1} es2015-polyfills.c5dd28b362270c767b34.js (es2015-polyfills) 56.4 kB [initial] [rendered]
chunk {2} main.fa4681da67b0e16d34dc.js (main) 128 bytes [initial] [rendered]
chunk {3} polyfills.41559ea504b9f00b6dea.js (polyfills) 130 bytes [initial] [rendered]
chunk {4} styles.49750c7a31f4506c6aeb.css (styles) 61.4 kB [initial] [rendered]

ERROR in : Can't resolve all parameters for ɵa in C:/xxxxxx/node_modules/@darkedges/framauthui/darkedges-framauthui.d.ts: ([object Object], ?).
```

Here is how I fixed it.

<!-- more -->

looking at  `C:/xxxxxx/node_modules/@darkedges/framauthui/darkedges-framauthui.d.ts` it was having an issue with

```bash
export { OpenAMHTTPService as ɵa } from './lib/services/http.service';
```

I found that it was complaining about

```bash
constructor(private injector: Injector, private framauthui: FRAMAuthUI) {
  }
```

It could not find `FRAMAuthUI`, but it was being imported

```bash
import { FRAMAuthUI } from '../core/';
```

turns out it was noit liking the barrel format and by changing to

```bash
import { FRAMAuthUI } from '../core/framauthui-core';
```

the error would disappear. Not sure why it was fine in `development` mode and fails in `production`.
