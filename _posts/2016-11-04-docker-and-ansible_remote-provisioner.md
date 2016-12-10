---
id: 236
title: Packer, Docker and ansible_remote provisioner
date: 2016-11-04T03:17:16+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=236
permalink: /2016/11/04/docker-and-ansible_remote-provisioner/
categories:
  - Docker
  - Uncategorized
---
Whilst configuring Packer for builder a `Docker` image using an `ansible_remote` Provisioner I was getting the following.

```bash
==> docker: SSH proxy: serving on 127.0.0.1:45113
    docker:  [WARNING]: Optional dependency 'cryptography' raised an exception, falling
    docker: back to 'Crypto'
    docker: statically included: /etc/ansible/roles/sardpost.modsecurity/tasks/EL.yml
    docker:
    docker: PLAY [default] *****************************************************************
    docker:
    docker: TASK [setup] *******************************************************************
    docker: SSH proxy: accepted connection
==> docker: authentication attempt from 127.0.0.1:50558 to 127.0.0.1:45113 as vagrant using none
==> docker: authentication attempt from 127.0.0.1:50558 to 127.0.0.1:45113 as vagrant using publickey
    docker: fatal: [default]: FAILED! => {"failed": true, "msg": "failed to transfer file to /root/.ansible/tmp/ansible-tmp-1478251525.09-198162303421559/setup:\n\n\n"}
```

A quick search and I found this link https://github.com/mitchellh/packer/issues/3260#issuecomment-199233230

I changed my `packer.json` from

```json
{
    "builders": [
        {
            "type": "docker",
            "image": "centos",
            "commit": true
        }
    ],
    "provisioners": [
        {
            "type": "ansible",
            "playbook_file": "../ansible/mod_security.yml"
        }
    ],
    "post-processors": [
        [
            {
                "type": "docker-tag",
                "repository": "darkedges/packer-ansible",
                "tag": "0.1"
            }
        ]
    ]
}

```

to

```json
{
    "variables": {
        "ansible_host": "default",
        "ansible_connection": "docker"
    },
    "builders": [
        {
            "type": "docker",
            "image": "centos",
            "commit": true,
            "run_command": [
                "-d", "-i", "-t", "--name", "{{user `ansible_host`}}", "{{.Image}}", "/bin/bash"
            ]
        }
    ],
    "provisioners": [
        {
            "type": "ansible",
            "groups": [ "webserver" ],
            "playbook_file": "../ansible/mod_security.yml",
            "extra_arguments": [
                "--extra-vars",
                "ansible_user=root ansible_host={{user `ansible_host`}} ansible_connection={{user `ansible_connection`}}"
            ]
        }
    ],
    "post-processors": [
        [
            {
                "type": "docker-tag",
                "repository": "darkedges/packer-ansible",
                "tag": "0.1"
            }
        ]
    ]
}
```
