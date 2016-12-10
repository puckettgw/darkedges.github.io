---
id: 248
title: 'perl.exe: error while loading shared libraries: ?: cannot ed object file: No such file or directory'
date: 2016-11-20T16:34:40+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=248
permalink: /2016/11/20/perl-exe-error-while-loading-shared-libraries-cannot-ed-object-file-no-such-file-or-directory/
categories:
  - babun
---
I was experiencing the following when I installed ruby via `pact install ruby`

```
{ ~ }  » perl -v
C:/Development/.babun/cygwin/bin/perl.exe: error while loading shared libraries: ?: cannot ed object file: No such file or directory
```

Found the out the following

```
{ ~ }  » cygcheck.exe /bin/perl
C:\Development\.babun\cygwin\bin\perl.exe
  C:\Development\.babun\cygwin\bin\cygperl5_22.dll
    C:\Development\.babun\cygwin\bin\cyggcc_s-1.dll
      C:\Development\.babun\cygwin\bin\cygwin1.dll
        C:\WINDOWS\system32\KERNEL32.dll
          C:\WINDOWS\system32\api-ms-win-core-rtlsupport-l1-2-0.dll
          C:\WINDOWS\system32\ntdll.dll
          C:\WINDOWS\system32\KERNELBASE.dll
          C:\WINDOWS\system32\api-ms-win-core-processthreads-l1-1-2.dll
          C:\WINDOWS\system32\api-ms-win-core-registry-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-heap-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-memory-l1-1-2.dll
          C:\WINDOWS\system32\api-ms-win-core-handle-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-synch-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-file-l1-2-1.dll
          C:\WINDOWS\system32\api-ms-win-core-delayload-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-io-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-job-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-threadpool-legacy-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-threadpool-private-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-libraryloader-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-namedpipe-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-datetime-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-sysinfo-l1-2-1.dll
          C:\WINDOWS\system32\api-ms-win-core-timezone-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-localization-l1-2-1.dll
          C:\WINDOWS\system32\api-ms-win-core-localization-private-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-processenvironment-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-string-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-debug-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-errorhandling-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-fibers-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-util-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-profile-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-security-base-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-comm-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-wow64-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-realtime-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-systemtopology-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-processtopology-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-namespace-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-file-l2-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-xstate-l2-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-localization-l2-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-normalization-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-sidebyside-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-appcompat-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-windowserrorreporting-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-console-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-console-l2-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-psapi-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-psapi-ansi-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-psapi-obsolete-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-security-appcontainer-l1-1-0.dll
    C:\Development\.babun\cygwin\bin\cygssp-0.dll
cygcheck: track_down: could not find cygcrypt-0.dll
```

Found that I had to do

```
pact remove libcrypt0 crypt
```

and then

```
pact install crypt
```

Finally I ran

```
{ ~ }  » cygcheck.exe /bin/perl
C:\Development\.babun\cygwin\bin\perl.exe
  C:\Development\.babun\cygwin\bin\cygperl5_22.dll
    C:\Development\.babun\cygwin\bin\cyggcc_s-1.dll
      C:\Development\.babun\cygwin\bin\cygwin1.dll
        C:\WINDOWS\system32\KERNEL32.dll
          C:\WINDOWS\system32\api-ms-win-core-rtlsupport-l1-2-0.dll
          C:\WINDOWS\system32\ntdll.dll
          C:\WINDOWS\system32\KERNELBASE.dll
          C:\WINDOWS\system32\api-ms-win-core-processthreads-l1-1-2.dll
          C:\WINDOWS\system32\api-ms-win-core-registry-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-heap-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-memory-l1-1-2.dll
          C:\WINDOWS\system32\api-ms-win-core-handle-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-synch-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-file-l1-2-1.dll
          C:\WINDOWS\system32\api-ms-win-core-delayload-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-io-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-job-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-threadpool-legacy-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-threadpool-private-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-libraryloader-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-namedpipe-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-datetime-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-sysinfo-l1-2-1.dll
          C:\WINDOWS\system32\api-ms-win-core-timezone-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-localization-l1-2-1.dll
          C:\WINDOWS\system32\api-ms-win-core-localization-private-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-processenvironment-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-string-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-debug-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-errorhandling-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-fibers-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-util-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-profile-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-security-base-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-comm-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-wow64-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-realtime-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-systemtopology-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-processtopology-l1-2-0.dll
          C:\WINDOWS\system32\api-ms-win-core-namespace-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-file-l2-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-xstate-l2-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-localization-l2-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-normalization-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-sidebyside-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-appcompat-l1-1-1.dll
          C:\WINDOWS\system32\api-ms-win-core-windowserrorreporting-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-console-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-console-l2-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-psapi-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-psapi-ansi-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-core-psapi-obsolete-l1-1-0.dll
          C:\WINDOWS\system32\api-ms-win-security-appcontainer-l1-1-0.dll
    C:\Development\.babun\cygwin\bin\cygssp-0.dll
    C:\Development\.babun\cygwin\bin\cygcrypt-0.dll
{ ~ }  » perl -v

This is perl 5, version 22, subversion 2 (v5.22.2) built for cygwin-thread-multi-64int

Copyright 1987-2015, Larry Wall

Perl may be copied only under the terms of either the Artistic License or the
GNU General Public License, which may be found in the Perl 5 source kit.

Complete documentation for Perl, including FAQ lists, should be found on
this system using "man perl" or "perldoc perl".  If you have access to the
Internet, point your browser at http://www.perl.org/, the Perl Home Page.

```

Problem solved and I am not sure why it happened.


