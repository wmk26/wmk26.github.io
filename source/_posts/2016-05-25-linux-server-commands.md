---
title: Linux服务器相关命令实践总结
categories:
  - Linux
date: 2016-05-25 17:00:10
tags:
  - linux
---

0\. 通用
======

注：以ngin为例

0.0 文件夹功能
---------

安装位置

文件类型

/usr/bin

普通执行程序文件

/usr/sbin

服务器执行程序文件和管理程序文件

/etc

应用程序配置文件

/var/log

日志文件

/usr/share/doc

应用程序文档文件

/usr/share/man

应用程序手册页文件

具体可以参考这篇文章： [Linux中应用程序目录结构分析](http://www.cnblogs.com/mizhongqin/archive/2013/06/05/tomcat_dir.html)

0.1 查找文件安装路径
------------

    whereis nginx
    

0.2 查找运行文件所在路径（即可执行程序）
----------------------

    which nginx
    

以上具体可以参考这篇文章：[Linux如何查找文件安装路径](http://www.cnblogs.com/qq78292959/archive/2012/03/04/2379763.html)

1\. 用户相关
========

1.1 新建用户
--------

主要有如下两个命令，adduser更适合初级使用者，useradd比较适合有些高阶经验的使用者，往往一行命令加参数就能解决很多问题，所以创建起来十分方便。：

*   useradd，如果后面不添加任何参数选项，例如：#sudo useradd test创建出来的用户将是默认“三无”用户：一无Home Directory，二无密码，三无系统Shell。
*   adduser，创建用户的过程更像是一种人机对话，系统会提示你输入各种信息，然后会根据这些信息帮你创建新用户。

具体可以参考这篇文章：[Ubuntu中useradd和adduser的区别](http://os.51cto.com/art/201104/256231.htm)

1.2 添加sudo权限
------------

修改/etc/sudoers文件，修改文件如下：

    # User privilege specification
    root ALL=(ALL) ALL
    db ALL=(ALL) ALL
    # 保存退出，db用户就拥有了root权限。
    

具体可以参考这篇文章：[Ubuntu创建新用户并增加管理员权限](https://www.douban.com/note/362271457/)

1.3 删除用户
--------

    sudo userdel -r user
    # 彻底删除用户，加上-r的选项，在删除用户的同时一起把这个用户的宿主目录和邮件目录删除。
    

具体可以参考这篇文章：[ubuntu如何彻底删除用户（帐户）！！！](http://forum.ubuntu.org.cn/viewtopic.php?t=201243)

2\. nginx相关
===========

可以通过whereis nginx和which nginx查找nginx的安装目录和可执行程序目录 nginx启动/停止/重启

    #注：目前知道有三种方式，假设nginx的可执行程序位置在/usr/sbin/nginx中
    
    # 方法一：
    
    sudo service nginx start/stop/restart
    
    # 方法二：
    
    sudo /etc/init.d/nginx start/stop/restart
    
    # 方法三：
    
    sudo /usr/sbin/nginx -s start/stop/reload
    

具体可以参考如下两篇文章：

*   [CentOS+nginx+uwsgi+Django 环境部署](http://www.jianshu.com/p/7494560da3e6)
*   [基于nginx和uWSGI在Ubuntu上部署Django](http://www.jianshu.com/p/e6ff4a28ab5a)

3\. Centos区别于Ubuntu的地方
======================

3.1 SELinux
-----------

    # 将SELinux设置为宽容模式
    sudo setenforce 0
    
    # 查看状态
    sudo getenforce
    
    # 查看SELinux状态，如时enable说明开启
    sudo /usr/sbin/sestatus
    
    

3.2 防火墙
-------

centos会默认启动firewalld服务，导致无法从别的机器访问we应用。

    # centos 7以上推荐用如下两条命令，可以参考这个链接：http://stackoverflow.com/questions/24756240/how-can-i-use-iptables-on-centos-7：
    
    # 临时关闭防火墙
    sudo systemctl stop firewalld
    sudo systemctl mask firewalld
    
    # 或者开放需要的端口
    sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
    sudo firewall-cmd --reload
    
    # cento 7之前使用的是iptables，相关设置如下：
    
    # 临时关闭防火墙
    sudo service iptables stop
    
    # 或者开放一些需要的端口，比如 80
    sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
    

具体可以参考如下两篇文章：

*   [Django 部署(Nginx)](http://www.ziqiangxuetang.com/django/django-nginx-deploy.html)
*   [nginx + uwsgi + centos部署 django攻略](http://www.jianshu.com/p/80393ae41a5f)

4\. 端口相关
========

    # 查看端口占用
    netstat -lpnt
    
    # 解除端口占用，如8000端口
    sudo fuser -k 8000/tcp
    
    # 查看远程端口是否可以连通，以百度为例，如果出现Connected表示连通了，说明百度的80端口开放的
    telnet baidu.com 80
    

具体可以参考如下文献：

*   [查看远端的端口是否通畅3个简单实用案例！](http://oldboy.blog.51cto.com/2561410/942530)

5\. 进程相关
========

    # 显示所有的进程
    ps -aux
    
    # 杀死进程
    kill -9 PID
    

具体可以参考如下两篇文章：

*   [4\. ps 进程查看器](http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/ps.html)
*   [Linux下ps查找进程用kill终止命令](http://os.51cto.com/art/200905/125605.htm)
