---
id: 161
title: Enable SSL on Port 443 for non root user
date: 2015-08-14T19:21:50+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=161
permalink: /2015/08/14/enable-ssl-on-port-443-for-non-root-user/
categories:
  - Uncategorized
---
As the Oracle user

[code]
/u01/app/oracle/shared/instances/instance1/bin/opmnctl stopproc ias-component=ohs1&lt;/span&gt;
cat &lt;&lt;EOF &gt; /u01/app/oracle/shared/instances/instance1/config/OHS/ohs1/httpd.conf
User oracle
Group install
EOF
[/code]

<!-- more --> 

&nbsp;

As the Root user

[code]

cd /u01/app/oracle/fmw/Oracle_WT1/ohs/bin/
chown root .apachectl
chmod 6750 .apachectl 

[/code]

&nbsp;

As the Oracle user

[code]

/u01/app/oracle/shared/instances/instance1/bin/opmnctl startproc ias-component=ohs1

[/code]
