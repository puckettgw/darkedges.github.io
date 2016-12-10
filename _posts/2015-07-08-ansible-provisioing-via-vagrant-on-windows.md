---
id: 129
title: Ansible Provisioning via Vagrant on Windows
date: 2015-07-08T21:42:30+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=129
permalink: /2015/07/08/ansible-provisioing-via-vagrant-on-windows/
categories:
  - Ansible
  - Provisioning
---
I have had a working Windows Vagrant / Ansible environment for a while, and now have decided to document the process (see xxxx). First though I am documenting the gotchas I have found.
<!-- more --> 
<h2>Gotcha 1 - Spaces in Directory Path</h2>
When using Ansible on a a <a href="http://www.cygwin.com">Cygwin</a> / <a href="http://babun.github.io/">Babun</a> shell, make sure that the users home directory does not contain any spaces. i.e <strong><span style="font-family: 'courier new', courier, monospace;">/home/NIrving Development</span></strong> , as this causes some issues. The first issue is the following

[code lang="bash"]

{ test } » vagrant provision ~/vm/test
==&gt; default: Running provisioner: ansible...
PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=true ANSIBLE_HOST_KEY_CHECKING=false ANSIBLE_SSH_ARGS='-o UserKnownHostsFile=/dev/null -o ControlMaster=auto -o ControlPersist=60s' ansible-playbook --private-key=&lt;strong&gt;D:/vm/babun/.babun/cygwin/home/NIrving Development/vm/test/.vagrant/machines/default/virtualbox/private_key&lt;/strong&gt; --user=vagrant --connection=ssh --limit='default' --inventory-file=&lt;strong&gt;D:/vm/babun/.babun/cygwin/home/NIrving Development/vm/test/.vagrant/provisioners/ansible/inventory&lt;/strong&gt; playbook.yml
Usage: ansible-playbook playbook.yml

Options:

.........

--version show program's version number and exit
Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.

[/code]

Notice the spaces. To fix this move the Vagrant container to a directory that does not have any spaces e.g <span style="font-family: 'courier new', courier, monospace;"><strong>/opt/vm/test</strong></span> and run again.
<h2>Gotcha 2 - File Permissions</h2>
Once the directory path was resolved, the next issue was related to file permissions of the private key.

[code lang="bash"]
GATHERING FACTS ***************************************************************
fatal: [default] =&gt; private_key_file (D:/vm/babun/.babun/cygwin/opt/vm/test/.vagrant/machines/default/virtualbox/private_key) is group-readable or world-readable and thus insecure - you will probably get an SSH failure

TASK: [update apt cache] ******************************************************
FATAL: no hosts matched or all hosts have already failed -- aborting
PLAY RECAP ********************************************************************
to retry, use: --limit @/home/NIrving Development/playbook.retry

default : ok=0 changed=0 unreachable=1 failed=0

Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.

[/code]

This is not so easily fixed, as changing the file permissions do not alter the error. To resolve the check has to be removed from <span style="font-family: 'courier new', courier, monospace;">/usr/lib/python2.7/site-packages/ansible/runner/connection.py</span>
<h2>Gotcha 3 - SSH Connections</h2>
The next issue was related to connecting via SSH.

[code lang="bash"]
GATHERING FACTS ***************************************************************
fatal: [default] =&gt; SSH Error: mux_client_request_session: send fds failed
while connecting to 127.0.0.1:2222
It is sometimes useful to re-run the command using -vvvv, which prints SSH debug output to help diagnose the issue.

TASK: [update apt cache] ******************************************************
FATAL: no hosts matched or all hosts have already failed -- aborting
PLAY RECAP ********************************************************************
to retry, use: --limit @/home/NIrving Development/playbook.retry

default : ok=0 changed=0 unreachable=1 failed=0

Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.

[/code]

This can be resolved by creating the <span style="font-family: 'courier new', courier, monospace;">~/.ansible.cfg</span> with the following details

[code lang="bash"]
[ssh_connection]
control_path = /tmp
[/code]

<h2>Gotcha 4 - DLL issues</h2>
<pre>Next I was experiencing issues with Babun and DLLs.</pre>

[code]
GATHERING FACTS ***************************************************************
 171 [main] python2.7 4988 child_info_fork::abort: address space needed by '_speedups.dll' (0xA50000) is already occupied
fatal: [default] =&gt; Traceback (most recent call last):
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/__init__.py&quot;, line 582, in _executor
 exec_rc = self._executor_internal(host, new_stdin)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/__init__.py&quot;, line 785, in _executor_internal
 return self._executor_internal_inner(host, self.module_name, self.module_args, inject, port, complex_args=complex_args)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/__init__.py&quot;, line 1032, in _executor_internal_inner
 result = handler.run(conn, tmp, module_name, module_args, inject, complex_args)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/action_plugins/normal.py&quot;, line 57, in run
 return self.runner._execute_module(conn, tmp, module_name, module_args, inject=inject, complex_args=complex_args)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/__init__.py&quot;, line 480, in _execute_module
 self._transfer_str(conn, tmp, module_name, module_data)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/__init__.py&quot;, line 307, in _transfer_str
 conn.put_file(afile, remote)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/connection_plugins/ssh.py&quot;, line 423, in put_file
 (p, stdin) = self._run(cmd, indata)
 File &quot;/usr/lib/python2.7/site-packages/ansible/runner/connection_plugins/ssh.py&quot;, line 111, in _run
 stdout=subprocess.PIPE, stderr=subprocess.PIPE)
 File &quot;/usr/lib/python2.7/subprocess.py&quot;, line 710, in __init__
 errread, errwrite)
 File &quot;/usr/lib/python2.7/subprocess.py&quot;, line 1223, in _execute_child
 self.pid = os.fork()
OSError: [Errno 11] Resource temporarily unavailable

TASK: [update apt cache] ******************************************************
FATAL: no hosts matched or all hosts have already failed -- aborting

PLAY RECAP ********************************************************************
 to retry, use: --limit @/home/NIrving Development/playbook.retry

default : ok=0 changed=0 unreachable=1 failed=0

 217 [main] python2.7 4204 child_info_fork::abort: address space needed by '_speedups.dll' (0xA50000) is already occupied
Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.

[/code]

Reading <a href="http://stackoverflow.com/questions/9300722/cygwin-error-bash-fork-retry-resource-temporarily-unavailable">http://stackoverflow.com/questions/9300722/cygwin-error-bash-fork-retry-resource-temporarily-unavailable</a> gave the answer.
<ol>
	<li>Open a Command Prompt and change to the  cygwin directory of the Babun install
<span style="font-family: 'courier new', courier, monospace;">D:\vm\babun\.babun\cygwin\bin</span></li>
	<li>Issue the following command
<span style="font-family: 'courier new', courier, monospace;">ash.exe</span></li>
	<li>In the prompt enter
<span style="font-family: 'courier new', courier, monospace;">/usr/bin/rebaseall -v</span></li>
	<li>Retry the provisioning.</li>
</ol>
So far these are all the gotcha I have encountered, any more and I will update this page.