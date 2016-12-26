---
id: 179
title: 'cannot find namespace &#8216;firebase&#8217;'
date: 2016-09-08T22:52:45+00:00
author: nirving
layout: post
guid: http://www.darkedges.com/blog/?p=179
permalink: /2016/09/08/cannot-find-namespace-firebase/
categories:
  - Angular
  - Firebase
---
I was having this issue after trying to follow the instructions at <a href="https://github.com/angular/angularfire2/blob/master/docs/1-install-and-setup.md">https://github.com/angular/angularfire2/blob/master/docs/1-install-and-setup.md</a>
<pre>cannot find namespace 'firebase'</pre>

<!-- more --> 

After reading <a href="http://stackoverflow.com/questions/39118836/angularfire2-cannot-find-namespace-firebase">http://stackoverflow.com/questions/39118836/angularfire2-cannot-find-namespace-firebase</a> I added

[code]

&quot;types&quot;: [
&quot;jasmine&quot;,
&quot;firebase&quot;
]
[/code]

to <code>src/tsconfig.json</code> and fired up <code>ng serve</code> and the issue was gone.

## Update
When I moved to a Windows environment I discovered the same issue, but the above did not work. Working through the various reports I discovered that the following in `src\app\app.module.ts` resolved the issue
*before*
```
import { AngularFireModule } from 'angularfire2';
```
*after*
```
import { AngularFireModule } from 'angularfire2';
import * as firebase from 'firebase'
```

This works for `angular-cli` version
```
angular-cli: 1.0.0-beta.15
node: 6.6.0
os: win32 x64
```

It would appear that a future update would resolve this issue [fix(imports)](https://github.com/angular/angularfire2/commit/c3a954cd857fc297438f53d3a1e3bf69b6af01c2).