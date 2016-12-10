---
id: 167
title: Getting Apex to play nicely with AWS API Gateway
date: 2016-05-18T19:40:05+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=167
permalink: /2016/05/18/getting-apex-to-play-nicely-with-aws-api-gateway/
categories:
  - Apex
  - AWS
---
Whilst learning <a href="https://apex.run/">Apex</a> I found an interesting issue getting the AWS API Gateway integration working.

Following the instructions the order of play is
<ol>
 	<li>
<pre>apex init</pre>
</li>
 	<li>
<pre>apex infra plan</pre>
</li>
 	<li>
<pre>apex infra apply</pre>
</li>
 	<li>
<pre>apex deploy</pre>
</li>
</ol>
If you copy the required files from the <a href="https://github.com/apex/apex/tree/master/_examples/api-gateway">example</a>  in the following order
<ol>
 	<li>
<pre>apex init</pre>
</li>
 	<li>
<pre>cp /tmp/apex/_example/infrastructure/dev/api-gateway* infrastructure/dev/</pre>
</li>
 	<li>
<pre>apex infra plan</pre>
</li>
</ol>
It errors out as follows.
<pre>Errors:

* 1 error(s) occurred:

* Required variable not set: apex_function_hello
⨯ Error: exit status 1
</pre>
<p class="p3">Appears the correct order is</p>

<ol>
 	<li>
<pre>apex init</pre>
</li>
 	<li>
<pre>apex infra plan</pre>
</li>
 	<li>
<pre>apex infra apply</pre>
</li>
 	<li>
<pre>apex deploy</pre>
</li>
 	<li>
<pre>cp /tmp/apex/_example/infrastructure/dev/api-gateway* infrastructure/dev/</pre>
</li>
 	<li>
<pre>apex infra plan</pre>
</li>
 	<li>
<pre>apex infra apply</pre>
</li>
</ol>
You can then go and check the API works in the AWS Console.