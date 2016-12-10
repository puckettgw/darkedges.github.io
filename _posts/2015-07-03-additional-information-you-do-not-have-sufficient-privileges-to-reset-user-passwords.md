---
id: 121
title: 'Additional Information:  You do not have sufficient privileges to reset user passwords'
date: 2015-07-03T23:38:33+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=121
permalink: /2015/07/03/additional-information-you-do-not-have-sufficient-privileges-to-reset-user-passwords/
categories:
  - OUD
---
When trying to modify a users password via OUD I was encountering the following error

[code lang="bash"]

/u01/app/oracle/fmw/asinst_1/OUD/bin/ldapmodify -h localhost -p 1389 -Duid=oamLDAP,cn=systemids,dc=example,dc=com -w xxxxxx-f /tmp/weblogic.ldif

Processing MODIFY request for cn=Test User,cn=users,dc=example,dc=com
MODIFY operation failed
Result Code: 50 (Insufficient Access Rights)
Additional Information: You do not have sufficient privileges to reset user passwords

[/code]

Turns out I was missing a privilege on the user performing the request, even though they were granted the write permission for the attribute

[code lang="bash"]

/u01/app/oracle/fmw/asinst_1/OUD/bin/ldapsearch -h localhost -p 1389 -D&quot;cn=Directory Manager&quot; -w Passw0rd -g &quot;dn: uid=oamLDAP,cn=systemids,dc=example,dc=com&quot; -b cn=users,dc=example,dc=com &quot;(uid=test.user@darkedges.com)&quot; aclRights &quot;userPassword&quot;
dn: cn=Test User,cn=users,dc=example,dc=com
userPassword: {SSHA512}xxxxxxxx
aclRights;attributeLevel;userpassword: search:1,read:1,compare:1,write:1,selfwrite_add:0,selfwrite_delete:0,proxy:0
aclRights;entryLevel: add:1,delete:1,read:1,write:1,proxy:0

[/code]

To resolve I had to create the following ldif file

[code]

dn: oamLDAP,cn=systemids,dc=example,dc=com
changetype: modify
add: ds-privilege-name
ds-privilege-name: password-reset

[/code]

and execute

[code]

/u01/app/oracle/fmw/asinst_1/OUD/bin/ldapmodify -h localhost -p 1389 -D&quot;cn=Directory Manager&quot; -w xxxxxx -f /tmp/weblogic2.ldif
Processing MODIFY request for uid=oamLDAP,cn=systemids,dc=example,dc=com
MODIFY operation successful for DN uid=oamLDAP,cn=systemids,dc=example,dc=com

[/code]

Now I get the following

[code]

/u01/app/oracle/fmw/asinst_1/OUD/bin/ldapmodify -h localhost -p 1389 -Duid=oamLDAP,cn=systemids,dc=example,dc=com -w xxxxxx -f /tmp/weblogic.ldif
Processing MODIFY request for cn=Test User,cn=users,dc=example,dc=com
MODIFY operation successful for DN cn=Test User,cn=users,dc=example,dc=com

[/code]
