---
id: 33
title: 'Installing Carbon and using it correctly'
date: 2016-12-22T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=33
permalink: /2017/02/05/powershell-carbon-clobber/
categories:
  - powershell

---

Learning the official way to develop using DSC and found a good helper module called [http://get-carbon.org/](Carbon)

When I tried to install it I received an error and so began my adventure on how to release the issue.

<!-- more -->

## Install Carbon

I used the command to install the `Carbon` Module

```PowerShell
Install-Module Carbon -Force -AllowClobber
```

and received the following after a while.

```PowerShell
PackageManagement\Install-Package : A command with name 'Get-ScheduledTask' is already available on this system. This
module 'Carbon' may override the existing commands. If you still want to install this module 'Carbon', use
-AllowClobber parameter.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\1.0.0.1\PSModule.psm1:1772 char:21
+ ...          $null = PackageManagement\Install-Package @PSBoundParameters
+                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (Microsoft.Power....InstallPackage:InstallPackage) [Install-Package],
   Exception
    + FullyQualifiedErrorId : CommandAlreadyAvailable,Validate-ModuleCommandAlreadyAvailable,Microsoft.PowerShell.Pack
   ageManagement.Cmdlets.InstallPackage
```   

I went searching in the `Carbon` bitbucket repository for an issues and found 
[https://bitbucket.org/splatteredbits/carbon/issues/220/installation-of-carbon-module-asks-about](https://bitbucket.org/splatteredbits/carbon/issues/220/installation-of-carbon-module-asks-about)
which talks about using `-AllowClobber` to install the module and then using a `prefix` when importing the module.

## Using a prefix on module importing

To resolve any conflicts there is a `-Prefix` argument to `Import-Module` so this means I can use the 
following

```PowerShell
Import-Module Carbon -Prefix C -PassThru
```

returns

```PowerShell
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     2.4.0      Carbon                              {Add-CGroupMember, Add-CTrustedHost, Assert-CAdminPrivileg...
```

Further checking via

```PowerShell
Get-Command -Module Carbon
```

reveals

```PowerShell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Add-CGroupMembers                                  2.4.0      Carbon
Alias           Add-CTrustedHosts                                  2.4.0      Carbon
Alias           Assert-CAdminPrivileges                            2.4.0      Carbon

         :

Function        Write-CFile                                        2.4.0      Carbon
Filter          Protect-CString                                    2.4.0      Carbon
Filter          Unprotect-CString                                  2.4.0      Carbon
```

So I can call `Carbon` commands by prefixing `C`, so instead of calling `Add-GroupMembers` it will began
`Add-CGroupMembers`.