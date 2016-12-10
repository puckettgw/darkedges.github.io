---
id: 146
title: Installing CoreOS from ISO
date: 2015-07-12T21:37:23+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=146
permalink: /2015/07/12/installing-coreos-from-iso/
categories:
  - Uncategorized
tags:
  - CoreOS
---
Installing CoreOS on your own hardware is quite simple when you know what you are doing. Here are the steps I used to install CoreOS on an old PC Chasis I had laying around.

<!-- more --> 

<ol>
	<li>Download the latest CoreOS ISO.
First you need to choose the Channel you want to use from <a href="https://coreos.com/docs/running-coreos/platforms/iso/">https://coreos.com/docs/running-coreos/platforms/iso/</a></li>
	<li>Burn the ISO to a CD or USB.
I used <a href="http://www.linuxliveusb.com/">http://www.linuxliveusb.com/</a> to create a Bootable USB Disk.</li>
	<li>Generate a Private Key.
Since I was using windows I downloaded and installed PuTTYGen from <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html">http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html</a></li>
	<li>Create a <span style="font-family: 'courier new', courier, monospace;">cloud-config.yaml</span> file.
This file is required to ensure that you can log into the CoreOS device after installation.
Mine consisted of the following

[code]#cloud-config
users:- name: nirving
ssh-authorized-keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEA9yiF4mgijCjTJfEd/loA/C648krW05+fBXYtxNbLnfzwkrRZmbl9k0nIzJcKV4w3N9mmnI/udF7YTAHrSxTnHQhlsa445I6T+Y7OTvEQmqimBdymeUeiNiXUEmdgMMkGgN/EvHgc6Gs+xmGY1A4J0FZdwhe/0C4UpZgzLU3n2imVfFCEnJC+c7K+gG1EjIhw6NjSjGy+gXDsGOYcuia/ERTjUK2XxdGsGIktyBC0rYIg04XQ6+k6rvoYxW4HjK7wZj2vaOdnxqqbnXQxAxjY0WfMer2gqSP+xNTHAEITDpHbD2EosAAYfVfoWKpg5mnVqDf9BGCZHyoHuZiH5ynomQ== rsa-key-20150711
groups:
  - sudo[/code]

Make sure that this is saved in Unix format, otherwise it will cause issues later.
I am not of the exact syntax to enable root access, but the user <span style="font-family: 'courier new', courier, monospace;">nirving</span> has sudo so it can create the necessary configuration after installation.
<span style="text-decoration: underline;"><strong>Note:</strong></span> replace the <span style="font-family: 'courier new', courier, monospace;">ssh-rsa</span> entry with the public key generated previously.</li>
	<li>Boot the PC device of the USB.</li>
	<li>When it has finished loading, you will need to issue the following commands

[code lang="bash"]
sudo su -
mount /dev/sdb1 /mnt
coreos-install -d /dev/sda -c /mnt/cloud-config.yaml
[/code]

</li>
	<li>When finished you can reboot the PC Device and wait for it to start up.</li>
	<li>Connect to the PC Device via Putty or SSH using the Private Key created before and the user configured in the <span style="font-family: 'courier new', courier, monospace;">cloud-config.yaml</span> file.</li>
</ol>
<h3>Add Static IP</h3>
You will need to create a file containing the following, just remember to replace with the details you require for your network.


[code]

[Match]
Name=enp2s0

[Network]
Address=10.0.0.100/24
Gateway=10.0.0.1
DNS=10.0.0.1

[/code]

<h3>Allow Root to SSH in</h3>
Log in as the user created in the cloud-config.yaml file and issue the following commands


[code]

sudo su -
cp -rf ~/nirving/.ssh .

[/code]


You should now be able to connect using the same Private Key, although it is advisable to use a different key.

&nbsp;