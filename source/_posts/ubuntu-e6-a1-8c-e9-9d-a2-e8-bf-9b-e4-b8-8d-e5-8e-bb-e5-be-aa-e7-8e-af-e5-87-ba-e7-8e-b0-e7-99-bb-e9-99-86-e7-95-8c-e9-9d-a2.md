---
title: ubuntu 桌面进不去 循环出现登陆界面
url: 656.html
id: 656
categories:
  - Linux
date: 2015-11-03 09:32:32
tags:
---

不知道为何，电脑重启之后就无法进入ubuntu系统了，密码输入正确但是过一会儿就又回到了登陆界面。循环出现。。。在网上找了一些方法全没有用。于是自己摸索，记录一下过程。 根据[知乎上的一篇回答](http://www.zhihu.com/question/24423384)，运行如下命令。

#进去tty1
Ctrl + Alt + F1 
输入用户名和密码
startx
#一般来说能进入桌面，但是自己的还是不行。于是调出命令行，继续后面的操作

后来看到[这个帖子](http://bbs.51cto.com/thread-1163092-1.html)里面说的原因：

1.  空间不足；
2.  该用户使用的是no-loginbash

由于之前已经尝试了各种方法都不见效，于是想到可能是由于空间不足的缘故。之前登陆的时候一直提示boot分区空间不足，自己也都没在意。这次死马当作活马医，姑且一试，参考[这篇文章](http://blog.csdn.net/wzy_1988/article/details/8848311)，运行如下命令。

#查看系统现有内核
dpkg --get-selections|grep linux-image  

#查看现在使用的内核
uname -a

#清理/boot分区
sudo apt-get purge  内核名称

额，由于自己的情况比较特殊。无法清理，提示如下信息：

子进程 已安装post-removal脚本 返回了错误号1

参考[这篇文章](http://www.cnblogs.com/dolphi/p/3428442.html)解决，输入如下命令；

#进入目录 
/var/lib/dpkg

#删除一个文件夹，还是备份比较保险
sudo mv info info.bak

#重建文件夹
sudo mkdir info

之后继续清理/boot分区；重启之后不经意间就好了，233333. 其他一些原因也是可以参考一下的，[这篇文章](http://blog.sina.com.cn/s/blog_80393a3801018djz.html)值得参考。   参考文献：

*   [Ubuntu桌面等不进去？](http://www.zhihu.com/question/24423384)
*   [Ubuntu在命令行输入了一个startx命令，之后一直是登陆](http://bbs.51cto.com/thread-1163092-1.html)
*   [Ubuntu清理boot分区](http://blog.csdn.net/wzy_1988/article/details/8848311)
*   [解决 子进程已安装的 post-removal脚本返回了错误号 100 的方法](http://www.cnblogs.com/dolphi/p/3428442.html)
*   [ubuntu12.04循环登录，无法进桌面的问题。](http://blog.sina.com.cn/s/blog_80393a3801018djz.html)