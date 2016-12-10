---
id: 27
title: status is STATUS_FAIL for GET_AUTHN_SCHEME event. Error code OAM-02073
date: 2013-04-13T14:05:57+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=27
permalink: /2013/04/13/status-is-status_fail-for-get_authn_scheme-event-error-code-oam-02073/
categories:
  - OPMN
  - Oracle
  - Oracle Access Manager
  - Oracle HTTP Server
tags:
  - OPMN
---
This morning I was updating a WebGate to use SERVER_NAME as Preferred Host when I started to find this error message appearing in the Access Manager logs.
[java]
&lt;Apr 13, 2013 10:19:26 AM EST&gt; &lt;Warning&gt; &lt;oracle.oam.controller&gt; &lt;OAM-02073&gt; &lt;Error while checking if the resource is protected or not.&gt;
&lt;Apr 13, 2013 10:19:26 AM EST&gt; &lt;Error&gt; &lt;oracle.oam.proxy.oam&gt; &lt;OAM-04029&gt; &lt;Error in generating AMEvent. Details Event Response status is STATUS_FAIL for GET_AUTHN_SCHEME event. Error code OAM-02073 status fail isExcluded false&gt;
&lt;Apr 13, 2013 10:19:26 AM EST&gt; &lt;Error&gt; &lt;oracle.oam.proxy.oam&gt; &lt;OAM-04020&gt; &lt;Exception encountered while processing the request message:
oracle.security.am.proxy.oam.requesthandler.OAMProxyException: Event Response status is STATUS_FAIL for GET_AUTHN_SCHEME event. Error code OAM-02073 status fail isExcluded false
        at oracle.security.am.proxy.oam.requesthandler.NGProvider.checkProtected(NGProvider.java:4272)
        at oracle.security.am.proxy.oam.requesthandler.NGProvider.getIsRescProtectedResponse(NGProvider.java:1335)
        at oracle.security.am.proxy.oam.requesthandler.NGProvider.getResponse(NGProvider.java:336)
        at oracle.security.am.proxy.oam.requesthandler.RequestHandler.handleRequest(RequestHandler.java:346)
        at oracle.security.am.proxy.oam.requesthandler.RequestHandler.handleMessage(RequestHandler.java:169)
        at oracle.security.am.proxy.oam.requesthandler.ControllerMessageBean.getResponseMessage(ControllerMessageBean.java:75)
        at oracle.security.am.proxy.oam.requesthandler.ControllerMessageBean_eo7ylc_MDOImpl.__WL_invoke(Unknown Source)
        at weblogic.ejb.container.internal.MDOMethodInvoker.invoke(MDOMethodInvoker.java:35)
        at oracle.security.am.proxy.oam.requesthandler.ControllerMessageBean_eo7ylc_MDOImpl.getResponseMessage(Unknown Source)
        at oracle.security.am.proxy.oam.mina.ObClientToProxyHandler.messageReceived(ObClientToProxyHandler.java:205)
        at org.apache.mina.common.DefaultIoFilterChain$TailFilter.messageReceived(DefaultIoFilterChain.java:743)
        at org.apache.mina.common.DefaultIoFilterChain.callNextMessageReceived(DefaultIoFilterChain.java:405)
        at org.apache.mina.common.DefaultIoFilterChain.access$1200(DefaultIoFilterChain.java:40)
        at org.apache.mina.common.DefaultIoFilterChain$EntryImpl$1.messageReceived(DefaultIoFilterChain.java:823)
        at org.apache.mina.common.IoFilterEvent.fire(IoFilterEvent.java:54)
        at org.apache.mina.common.IoEvent.run(IoEvent.java:62)
        at oracle.security.am.proxy.oam.mina.CommonJWorkImpl.run(CommonJWorkImpl.java:41)
        at weblogic.work.j2ee.J2EEWorkManager$WorkWithListener.run(J2EEWorkManager.java:184)
        at weblogic.work.ExecuteThread.execute(ExecuteThread.java:256)
        at weblogic.work.ExecuteThread.run(ExecuteThread.java:221)
&gt;[/java]
Turns out that OHS was presenting an uknkown host to Access Manager via the WebGate, due to the Health Check being performed by <code>OPMN</code>. Turns out that <code>OPMN </code>make a <code>HEAD </code>request to <code>/index.html</code> using the Server Hostname as HOST quite often and I was seeing the following in <code>${ORACLE_INSTANCE}/diagnostics/logs/${COMPONENT_TYPE}/${COMPONENT_NAME}/oblog.log</code>

[shell]
2013/04/09@23:02:06.643000	2156	2556	ACCESS_GATE	ERROR	0x0000151A	..\src\isprotected.cpp:294	&quot;Failure to connect to Access Server&quot;	HTTPStatus^500	Error^The AccessGate is unable to contact any Access Servers.	
2013/04/09@23:02:06.644000	2156	2556	WEB	ERROR	0x0000151F	..\src\apache2_req_info.cpp:202	&quot;WebGate Error Report&quot;	Message^The WebGate plug-in is unable to contact any Access Servers.	ReqReq^HEAD  /index.html HTTP/1.1	ReqProto^HTTP/1.1	ReqHost^win-51nk3n8aiq8	ReqStatLine^	ReqStatus^200	ReqRawUri^/index.html	ReqUri^/index.html	ReqFilename^C:/Oracle/Middleware/Oracle_WT1/instances/instance1/config/OHS/ohs1/htdocs/index.html	ReqPath^	ReqArgs^	
[/shell]
By adding the server host name (<code>win-51nk3n8aiq8</code>) to the Host Identifier registered by the WebGate the error message disappeared from both logs.