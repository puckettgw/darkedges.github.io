---
id: 34
title: 'Running up Apache Zeppelin with Vagrant and Ansible'
date: 2016-12-22T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=34
permalink: /2017/02/13/apache-zeppelin-vagrant-ansible/
categories:
  - zeppelin
  - ansible

---

Wanting to review [Apache Zeppelin](https://zeppelin.apache.org/), a web based notebook that enables data analytics, for some projects I found this article [http://arjon.es/2015/08/23/vagrant-spark-zeppelin-a-toolbox-to-the-data-analyst/](http://arjon.es/2015/08/23/vagrant-spark-zeppelin-a-toolbox-to-the-data-analyst/).
I tried to get that example up and running and it failed, so I thought I would update it and make
it available at [https://github.com/darkedges/vagrant-zeppelin](https://github.com/darkedges/vagrant-zeppelin)

<!-- more -->

## Installation

```bash
git clone https://github.com/darkedges/vagrant-zeppelin.git
cd vagrant-zeppelin
vagrant up
ansible-galaxy install -r ansible/requirements.yml
ansible-playbook -i ansible/inventory ansible/deployZeppelin.yml
```

## Usage

Once completed open a web browser to [http://zeppelin.local.darkedges.com:8080/#/notebook/2AYDJMVVQ](http://zeppelin.local.darkedges.com:8080/#/notebook/2AYDJMVVQ) to start working on the Star Wars Kid Data Dump 
[http://waxy.org/2008/05/star_wars_kid_the_data_dump/](http://waxy.org/2008/05/star_wars_kid_the_data_dump/)
