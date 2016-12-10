---
id: 113
title: Issue with Password Policy Validation Module
date: 2015-07-02T15:04:03+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=113
permalink: /2015/07/02/issue-with-password-policy-validation-module/
categories:
  - 11.1.2.3.x
  - Oracle Access Manager
---
When enabling the Password Policy Validation Module I was receiving the following.

[code]

&lt;03/07/2015 7:32:40 AM AEST&gt; &lt;Error&gt; &lt;oracle.oam.user.identity.provider&gt; &lt;OAMSSA-20092&gt; 
&lt;Could not modify user attribute for user : uid=weblogic,cn=users,dc=example,dc=com, 
attributes : OUD, for idstore oracle.igf.ids.AuthorizationException: Insufficient Access 
rights to perform the operation: entity=uid=weblogic,cn=users,dc=example,dc=com 
op=modify AdditionalInfo: LDAP Error 50 : [LDAP: error code 50 - The entry 
uid=weblogic,cn=users,dc=example,dc=com cannot be modified due to insufficient access 
rights] with exception {3}.&gt;
[/code]

Seems that I forgot to run the idmconfigtool for OAM Configuration, but here is the quick way to resolve.
<ol>
	<li>Create an OAM Administration Group in your directory.
<ol style="list-style-type: lower-alpha;">
	<li>For example cn=OAMAdministrators,cn=groups,dc=example,dc=com</li>
	<li>Add a uniquemember that matches the Bind DN in the User Identity Store you are using.

[code]dn: cn=OAMAdministrators,cn=groups,dc=example,dc=com
objectClass: top
objectClass: groupofUniqueNames
cn: OAMAdministrators
uniquemember: uid=oamadmin,cn=users,dc=example,dc=com
uniquemember: uid=oamldap,cn=systemids,dc=example,dc=com[/code]

</li>
	<li>Add using ldapadd

[code]/u01/app/oracle/fmw/asinst_1/OUD/bin/ldapmodify -h localhost -p 1389 -D&quot;cn=Directory Manager&quot; -w Passw0rd -f /tmp/oamadmin.ldif[/code]

</li>
</ol>
</li>
	<li>Create an ACI file suitable for an ldapmodify command
<ol style="list-style-type: lower-alpha;">
	<li>Create a file containing

[code]
dn: dc=example,dc=com
changetype: modify
add: aci
aci: (targetattr=&quot;*&quot;)(version 3.0; acl &quot;Allow OAMAdminGroup add, read and write access to all attributes&quot;; allow(add,read,search,compare,write,delete,import,export) groupdn=&quot;ldap:///cn=OAMAdministrators,cn=groups,dc=example,dc=com&quot;;)
[/code]

</li>
	<li>Add using ldapmodify

[code]/u01/app/oracle/fmw/asinst_1/OUD/bin/ldapmodify -h localhost -p 1389 -D&quot;cn=Directory Manager&quot; -w Passw0rd -f /tmp/oamadmin.ldif[/code]

</li>
</ol>
</li>
	<li>Confirm that the error has gone away.</li>
</ol>