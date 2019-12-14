---
title: pycharm 常用配置
url: 812.html
id: 812
categories:
  - Python
date: 2016-01-12 15:25:32
tags:
---

pycharm是自己目前用到的最好的python IDE（好像自己也没有用过多少python IDE。。。）。此文意在记录一下相关的配置以及遇到的一些问题；

0\. 前言
======

一般来说，写python小程序，还是习惯于用vim，编辑快，在终端直接就可以运行，比较方便；但是一旦要写大程序，还是IDE用着比较顺手；而且使用pycharm的时候，会提示你那里写的不规范，在我看来，这相当有用，python本就是优雅的语言，如果配合好的编码规范代码会更加优雅好看。

1\. pycharm下载安装以及注册
===================

pycharm是商用软件，共有三种形式，一个时Professional版本，需要收费，一个是Community版本，可以免费使用，最后一种形式是针对学生用户的，可以申请免费使用Professional版本，下面介绍，“万恶的”资本主义对学生还是不错的（貌似国外的很多付费产品都有学生版的，这个待验证）。如果有工资，还是推荐使用付费的Professional版本，毕竟“本是同根生”，都是程序员。不过这里还是介绍一下破解的方式；

1.1 学生用户Professional版本
----------------------

进入[这个网址](https://www.jetbrains.com/student/)，然后Apply Now就可以了，前提时你有一个.edu的学生邮箱；申请之后，会有邮件确认，然后激活账户，就可以获得为期一年的Licence Code。一年后更新一下应该就可以了。

1.2 普通用户破解Professional版本
------------------------

方法一：

*   安装完成后，打开pycharm，有一个注册的界面，选择 License server ，填 http://idea.lanyus.com ，然后点击 OK；（注：一开始试了几次，说是无法连接，后来扶了一把梯子（翻墙），终于成功）

方法二（不知如何操作，且附录在此）：

*   安装；
*   调整时间到2038年；
*   申请30天试用；
*   退出pycharm；
*   时间调整回来。

2\. pycharm相关配置
===============

直接参考一下这个系列文章好了：[最全Pycharm教程（1）——定制外观](http://blog.csdn.net/u013088062/article/details/50135135)，自己打算以后慢慢实践，然后添加自己的配置；

3\. pycharm在windows科学计算的相关配置
============================

自己做的很多事情都是和数据处理相关的，所以经常使用numpy和scipy等python第三方库，之前在windows下的pycharm上总是安装不成功。估计时网络的问题吧，这里提供另一种方法；步骤如下 1) 首先安装pip，具体可以参考这篇文章：[怎么在windows下安装pip?](https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/8/README.html)。当然如果你不想这样安装，直接装一个python3.5就可以了，因为python3.4以上都自带pip。 2) 去这个网站：[Unofficial Windows Binaries for Python Extension Packages](http://www.lfd.uci.edu/~gohlke/pythonlibs/)，几乎囊括所有python在windows下的第三方库，关键是都编译好了，简直太给力了。搜索相关版本的库，然后下载； 3) 打开命令行，然后pip install *.whl就可以了；万事大吉；   待续。。。  

参考文献：
=====

*   [Pycharm5注册方式](http://www.cnblogs.com/evlon/p/4934705.html)
*   [最全Pycharm教程（1）——定制外观](http://blog.csdn.net/u013088062/article/details/50135135)
*   [怎么在windows下安装pip?](https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/8/README.html)
*   [Unofficial Windows Binaries for Python Extension Packages](http://www.lfd.uci.edu/~gohlke/pythonlibs/)