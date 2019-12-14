---
title: Linux知识&&问题
url: 755.html
id: 755
categories:
  - Linux
date: 2015-12-23 16:51:32
tags:
---

记录一些自己linux知识以及遇到的一些小问题

**一些问题：**
---------

### 1\. 软件中心（software center）闪退（[software-center 闪退怎么办？](https://forum.ubuntu.org.cn/viewtopic.php?f=80&t=453617)）

sudo update-software-center

### 2. “Problem with MergeList” or “status file could not be parsed” error（[How do I fix a “Problem with MergeList” or “status file could not be parsed” error when trying to do an update?](http://askubuntu.com/questions/30072/how-do-i-fix-a-problem-with-mergelist-or-status-file-could-not-be-parsed-err)）

sudo rm -vf /var/lib/apt/lists/*
sudo apt-get update

### 3\. ubuntu 无法链接vpn（[\[VPN\] connect-failed while starting a VPN connection \[solved\]](https://bbs.archlinux.org/viewtopic.php?id=194042)）

\# 编辑如下文件，如果没有，就新建一个
sudo vim /etc/modules-load.d/netfilter.conf

\# 在文件中添加如下内容
nf\_nat\_pptp
nf\_conntrack\_pptp
nf\_conntrack\_proto_gre

\# 重启

  日志位置：

*   /var/log/syslog，查看系统日志