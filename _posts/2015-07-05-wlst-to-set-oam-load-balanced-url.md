---
id: 123
title: WLST to set OAM Load Balancer URL
date: 2015-07-05T22:05:07+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=123
permalink: /2015/07/05/wlst-to-set-oam-load-balanced-url/
categories:
  - Oracle Access Manager
---
Found this snippet @ <a href="https://community.oracle.com/thread/3744036">https://community.oracle.com/thread/3744036</a>

<!-- more --> 

[code lang="python"]
domainRuntime()
name = ObjectName(&quot;oracle.oam&quot;, &quot;type&quot;, &quot;Config&quot;);
writeSig = [&quot;java.lang.String&quot;,&quot;javax.management.openmbean.CompositeData&quot;]
oamHostKey = &quot;DeployedComponent/Server/NGAMServer/Profile/OAMServerProfile/OAMSERVER/serverhost&quot;
oamPortKey = &quot;DeployedComponent/Server/NGAMServer/Profile/OAMServerProfile/OAMSERVER/serverport&quot;
oamProtKey = &quot;DeployedComponent/Server/NGAMServer/Profile/OAMServerProfile/OAMSERVER/serverprotocol&quot;
mbs.invoke(name, &quot;applyStringProperty&quot;, [oamHostKey,StringSettings(oamHostKey,&quot;myhostname&quot;).toCompositeData(StringSettings.toCompositeType())], writeSig)
mbs.invoke(name, &quot;applyStringProperty&quot;, [oamPortKey,StringSettings(oamPortKey,&quot;443&quot;).toCompositeData(StringSettings.toCompositeType())], writeSig)
mbs.invoke(name, &quot;applyStringProperty&quot;, [oamProtKey,StringSettings(oamProtKey,&quot;https&quot;).toCompositeData(StringSettings.toCompositeType())], writeSig)
[/code]
