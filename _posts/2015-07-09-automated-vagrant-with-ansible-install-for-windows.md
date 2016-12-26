---
id: 139
title: Automated Vagrant with Ansible install for Windows
date: 2015-07-09T15:24:07+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=139
permalink: /2015/07/09/automated-vagrant-with-ansible-install-for-windows/
categories:
  - Ansible
  - Provisioning
  - Uncategorized
  - Vagrant
  - Virtual Box
  - Virtualization
---
At <a href="https://github.com/darkedges/WinVagInsAns">https://github.com/darkedges/WinVagInsAns</a> is a Powershell script for Windows that installs <a href="http://babun.github.io/">Babun</a>, <a href="https://www.vagrantup.com/">Vagrant</a>, <a href="https://www.virtualbox.org/">VirtualBox</a> and <a href="http://www.ansible.com/">Ansible</a>. It does all the configuration so that you don't have to and once completed will give you a fully working Ansible Provisioner in Vagrant.
<!-- more --> 
<h2><a id="user-content-installation" class="anchor" href="https://github.com/darkedges/WinVagInsAns#installation"></a>Installation</h2>
This has been tested on Windows 7 Default Powershell V2. Other versions of OS / Powershell may have issues. It requires an environmental WINVAGINSANS to be set for where the software is to be installed. <strong>Note: This is for a clean install only. Existing installations will not work.</strong>

It performs the following steps
<ul>
	<li>Installs Babun.</li>
	<li>Installs Vagrant.</li>
	<li>Installs Virtual Box.</li>
	<li>Creates the necessary Environmental Variables.</li>
	<li>Creates the ansible-playbook.bat wrapper in the Vagrant Installation directory.</li>
	<li>Creates the InstallAnsible.sh wrapper for installation and configuration of Ansible in Babun.</li>
	<li>Executes the InstallAnsible.sh wrapper.</li>
	<li>Rebases Babun.</li>
</ul>
<h3><a id="user-content-powershell" class="anchor" href="https://github.com/darkedges/WinVagInsAns#powershell"></a>Powershell</h3>

[code]&lt;code&gt;$env:WINVAGINSANS=&quot;d:\vm&quot;; iex((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/darkedges/WinVagInsAns/v1.0/WinVagInsAns.ps1'))[/code]

&nbsp;
<h3><a id="user-content-windows-cmd" class="anchor" href="https://github.com/darkedges/WinVagInsAns#windows-cmd"></a>Windows CMD</h3>

[code]@powershell -NoProfile -ExecutionPolicy Bypass -Command &quot;$env:WINVAGINSANS=&quot;d:\vm&quot;; iex((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/darkedges/WinVagInsAns/v1.0/WinVagInsAns.ps1'))&quot;[/code]

<h2><a id="user-content-acknowledgements" class="anchor" href="https://github.com/darkedges/WinVagInsAns#acknowledgements"></a>Acknowledgements</h2>
The following resources have been helpful in generating this process.
<ul>
	<li><a href="http://stackoverflow.com/questions/9300722/cygwin-error-bash-fork-retry-resource-temporarily-unavailable">Babun DLL Issue</a></li>
	<li><a href="http://www.azavea.com/blogs/labs/2014/10/running-vagrant-with-ansible-provisioning-on-windows/">Ansible Playbook Wrapper</a></li>
</ul>