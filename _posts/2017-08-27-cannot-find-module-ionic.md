---
id: 33
title: "Cannot find module '@angular/tsc-wrapped/src/tsc'"
date: 2016-12-22T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=33
permalink: /2017/08/27/cannot-find-module-ionic/
categories:
  - ionic

---
During a recent development issues with npm I upgraded to v5.2.0.

When running an `ionic serve` I was getting the following error

```bash
Error: Cannot find module '@angular/tsc-wrapped/src/tsc'
```

<!-- more -->

```bash
{ authenticatorApp } master » ionic serve

> authenticatorApp@0.0.1 ionic:serve C:\development\ionic\authenticatorApp
> ionic-app-scripts serve "--v2" "undefined" "--address" "0.0.0.0" "--port" "8100" "--livereload-port" "35729"

module.js:491
    throw err;
    ^

Error: Cannot find module '@angular/tsc-wrapped/src/tsc'
    at Function.Module._resolveFilename (module.js:489:15)
    at Function.Module._load (module.js:439:25)
    at Module.require (module.js:517:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (C:\development\ionic\authenticatorApp\node_modules\@ionic\app-scripts\dist\aot\aot-compiler.js:7:13)
    at Module._compile (module.js:573:30)
    at Object.Module._extensions..js (module.js:584:10)
    at Module.load (module.js:507:32)
    at tryModuleLoad (module.js:470:12)
    at Function.Module._load (module.js:462:3)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! authenticatorApp@0.0.1 ionic:serve: `ionic-app-scripts serve "--v2" "undefined" "--address" "0.0.0.0" "--port" "8100" "--livereload-port" "35729"`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the authenticatorApp@0.0.1 ionic:serve script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\NIrving\AppData\Roaming\npm-cache\_logs\2017-08-27T01_02_00_104Z-debug.log
There was an error serving your Ionic application: There was an error with the spawned command: serve
```

Turns out there is an issue with `npm 5.2.0` and the following resolved it [https://forum.ionicframework.com/t/cannot-find-angular-tsc-wrapped-while-doing-an-ionic-serve/97853/5](https://forum.ionicframework.com/t/cannot-find-angular-tsc-wrapped-while-doing-an-ionic-serve/97853/5)

1. create a file on the Project Root called `.npmrc`
1. put the following code inside the File: `package-lock=false`
1. delete the `package-lock.json` File
1. run `npm cache clear --force`
1. run `npm install`
1. run `ionic serve`

``` bash
echo >> .npmrc <<EOF
package-lock=false
EOF
rm -rf package-lock.json
npm cache clear --force
npm install
ionic serve
```


