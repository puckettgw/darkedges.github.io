---
id: 96
title: DCC Authentication Failure
date: 2015-07-01T13:04:35+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=96
permalink: /2015/07/01/dcc-authentication-failure/
categories:
  - 11.1.2.3.x
  - Oracle Access Manager
---
After finally building my OAM 11.1.2.3.0 environment, I was playing with configuring a DCC with a custom Look and Feel. I thought I would have a look at the excellent article <a href="http://www.ateam-oracle.com/part-2-custom-login-and-logout-with-detached-credential-collector-dcc/">http://www.ateam-oracle.com/part-2-custom-login-and-logout-with-detached-credential-collector-dcc/</a> to give me a kick start to getting the ball rolling.

<!-- more --> 

In there the following code is present

[code lang="java"]
&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=ISO-8859-1&quot; pageEncoding=&quot;ISO-8859-1&quot;%&gt;
&lt;!DOCTYPE HTML PUBLIC &quot;-//W3C//DTD HTML 4.01 Transitional//EN&quot;&gt;
&lt;html&gt;
&lt;head&gt;&lt;/head&gt;
&lt;body&gt;
&lt;h2&gt; Enter your credentials &lt;/h2&gt;
&lt;form action=&quot;/oam/server/auth_cred_submit&quot; method=&quot;post&quot; name=&quot;form&quot;&gt;
Username: &lt;input name=&quot;username&quot; type=&quot;text&quot; maxlength=&quot;25&quot;&gt;
Password: &lt;input name=&quot;password&quot; type=&quot;password&quot;&gt;
&lt;input name=&quot;OAM_REQ&quot; type=&quot;hidden&quot; value=”&lt;%= request.getHeader(&quot;OAM_REQ”) %&gt;“&gt;
&lt;input type=&quot;image&quot; name=&quot;login&quot; src=&quot;images/login.png&quot; onclick=&quot;document.form.submit();&quot;/&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;[/code]

<div class="line number14 index13 alt1">When I tried that I would get the following error screen on submission of a successful password after an initial password failure.</div>
<div class="line number14 index13 alt1"><a href="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCErrorMessage.png"><img class=" size-medium wp-image-107 aligncenter" src="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCErrorMessage-300x142.png" alt="DCCErrorMessage" width="300" height="142" /></a></div>
<div class="line number14 index13 alt1"></div>
<div class="line number14 index13 alt1">I could not figure out what the issue was straight away, as I was successfully POSTing the <span style="font-family: 'courier new', courier, monospace;">OAM_REQ</span> data required. Then I decided to have a look at what actually was happening.</div>
<div class="line number14 index13 alt1"><a href="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCErrorCapture.png"><img class=" size-medium wp-image-108 aligncenter" src="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCErrorCapture-300x151.png" alt="DCCErrorCapture" width="300" height="151" /></a></div>
<div class="line number14 index13 alt1"></div>
<div class="line number14 index13 alt1">When the request failed, it was making a request for  <span style="font-family: 'courier new', courier, monospace;">images/login.png</span>, which was overwriting the cookie of <span style="font-family: 'courier new', courier, monospace;">DCCtxCookie_xxxxxxx</span> with a new value, causing there to be a mismatch in states between the <span style="font-family: 'courier new', courier, monospace;">OAM_REQ</span> value and the <span style="font-family: 'courier new', courier, monospace;">DCCtxCookie_xxxxxxx</span> value.</div>
<div class="line number14 index13 alt1"><a href="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCIssue.png"><img class=" size-full wp-image-109 aligncenter" src="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCIssue.png" alt="DCCIssue" width="292" height="84" /></a></div>
<div class="line number14 index13 alt1"></div>
<div class="line number14 index13 alt1">After changing the JSP to not use JavaScript for submission, it worked.</div>
<div class="line number14 index13 alt1">

[code lang="java"]
&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=ISO-8859-1&quot; pageEncoding=&quot;ISO-8859-1&quot;%&gt;
&lt;!DOCTYPE HTML PUBLIC &quot;-//W3C//DTD HTML 4.01 Transitional//EN&quot;&gt;
&lt;html&gt;
&lt;head&gt;&lt;/head&gt;
&lt;body&gt;
&lt;h2&gt; Enter your credentials &lt;/h2&gt;
&lt;form action=&quot;/oam/server/auth_cred_submit&quot; method=&quot;post&quot; name=&quot;form&quot;&gt;
Username: &lt;input name=&quot;username&quot; type=&quot;text&quot; maxlength=&quot;25&quot;&gt;
Password: &lt;input name=&quot;password&quot; type=&quot;password&quot;&gt;
&lt;input name=&quot;OAM_REQ&quot; type=&quot;hidden&quot; value=”&lt;%= request.getHeader(&quot;OAM_REQ”) %&gt;“&gt;
&lt;input type=&quot;submit&quot; name=&quot;login&quot;/&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;[/code]

</div>
<div class="line number14 index13 alt1"><a href="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCWorksCapture.png"><img class=" size-medium wp-image-110 aligncenter" src="http://www.darkedges.com/blog/wp-content/uploads/2015/07/DCCWorksCapture-300x266.png" alt="DCCWorksCapture" width="300" height="266" /></a></div>
<div class="line number14 index13 alt1">So note to self, when having issues with OAM always check to see what is going on between the browser and OAM, as 9 times out of 10, it is something you have done, rather than OAM itself.</div>