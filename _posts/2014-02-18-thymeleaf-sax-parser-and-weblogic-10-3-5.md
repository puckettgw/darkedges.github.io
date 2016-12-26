---
id: 49
title: ThymeLeaf, SAX Parser and WebLogic 10.3.5
date: 2014-02-18T00:14:24+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=49
permalink: /2014/02/18/thymeleaf-sax-parser-and-weblogic-10-3-5/
categories:
  - ThymeLeaf
---
Using ThymeLeaf on WebLogic 10.3.5 I was getting the following warning message.

<!-- more --> 

[code lang="java"]
WARN : org.thymeleaf.templateparser.xmlsax.XhtmlAndHtml5NonValidatingSAXTemplateParser - [THYMELEAF] The SAX Parser implementation being used (&quot;weblogic.xml.jaxp.RegistrySAXParser&quot;) does not implement the &quot;reset&quot; operation. This will force Thymeleaf to re-create parser instances each time they are needed for parsing templates, which is more costly. Enabling template cache is recommended, and also using a parser library which implements &quot;reset&quot; such as xerces version 2.9.1 or newer.
[/code]

Pretty self explanatory, so how did I go about fixing this.

Added the following to pom.xml
[code lang="xml" title="pom.xml"]
&lt;dependency&gt;
	&lt;groupId&gt;xerces&lt;/groupId&gt;
	&lt;artifactId&gt;xercesImpl&lt;/artifactId&gt;
	&lt;version&gt;2.9.1&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;xalan&lt;/groupId&gt;
	&lt;artifactId&gt;xalan&lt;/artifactId&gt;
	&lt;version&gt;2.7.1&lt;/version&gt;
&lt;/dependency&gt;
[/code] 

Edited weblogic-application.xml
[code lang="xml" title="src/main/application/META-INF/weblogic-application.xml"]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;weblogic-application
	xmlns=&quot;http://xmlns.oracle.com/weblogic/weblogic-application&quot;
	xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
	xsi:schemaLocation=&quot;http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/javaee_5.xsd http://xmlns.oracle.com/weblogic/weblogic-application http://xmlns.oracle.com/weblogic/weblogic-application/1.4/weblogic-application.xsd&quot;&gt;
	&lt;xml&gt;
		&lt;parser-factory&gt;
			&lt;saxparser-factory&gt;org.apache.xerces.jaxp.SAXParserFactoryImpl&lt;/saxparser-factory&gt;
			&lt;document-builder-factory&gt;weblogic.xml.jaxp.WebLogicDocumentBuilderFactory&lt;/document-builder-factory&gt;
			&lt;transformer-factory&gt;org.apache.xalan.processor.TransformerFactoryImpl&lt;/transformer-factory&gt;
		&lt;/parser-factory&gt;
	&lt;/xml&gt;
	&lt;prefer-application-packages&gt;
		&lt;package-name&gt;org.springframework.*&lt;/package-name&gt;
        	&lt;package-name&gt;org.apache.xerces.parsers.*&lt;/package-name&gt;
        	&lt;package-name&gt;org.apache.xalan.*&lt;/package-name&gt;
	&lt;/prefer-application-packages&gt;
&lt;/weblogic-application&gt;
[/code]

Finally updated weblogic.xml
[code lang="xml" title="src/main/webapp/WEB-INF/weblogic.xml"]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;weblogic-web-app xmlns=&quot;http://xmlns.oracle.com/weblogic/weblogic-web-app&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot; xsi:schemaLocation=&quot;http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd http://xmlns.oracle.com/weblogic/weblogic-web-app http://xmlns.oracle.com/weblogic/weblogic-web-app/1.0/weblogic-web-app.xsd&quot;&gt;
	&lt;container-descriptor&gt;
		&lt;prefer-web-inf-classes&gt;true&lt;/prefer-web-inf-classes&gt;
	&lt;/container-descriptor&gt;
	&lt;context-root&gt;xxxxxx&lt;/context-root&gt;
&lt;/weblogic-web-app&gt;
[/code]

Deployed and the warning message was gone.