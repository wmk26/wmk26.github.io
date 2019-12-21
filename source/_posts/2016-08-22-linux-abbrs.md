---
title: Linux常用缩写
categories:
  - Linux
date: 2016-08-22 10:00:39
tags:
  - linux
---

经常被各种缩写弄的云里雾里的，今天无意间中知乎上看到一个问答：[Linux中常用的命令都是哪些单词的缩写？](https://www.zhihu.com/question/49073893)，本来想着以后再看的，拖延症又犯了，后来想着也没有什么别的事情，还是立刻去做了的好。于是将整理了一下其中的答案，以备后用；建议查找的时候直接Ctrl+F； update: 20160920

1\. 目录缩写
--------

缩写

全称

说明

/bin

BINaries

二进制可执行命令

/dev

DEVices

特殊设备文件

/etc

ETCetera

系统管理和配置文件

/etc/fstab

FileSystem TABle

文件/etc/fstab包含了静态文件系统信息，定义了存储设备和分区整合到整个系统的方式。mount 命令会读取这个文件，确定设备和分区的挂载选项。

/lib

LIBraries

库文件

/mnt

MouNT

系统提供这个目录是让用户临时挂载其他的文件系统。

/opt

OPTion

第三方软件放置的目录。如果你想自行安装额外的软件(非原本的 distribution 提供的)，那么也能够将你的软件安装到这里来。

/proc

PROCesses

虚拟的目录，是系统内存的映射。可直接访问这个目录来获取系统信息。

/sbin

Super BINaries, Superuser BINaries

系统管理命令，这里存放的是系统管理员使用的管理程序

/srv

SeRVices

是一些网络服务启动后，这些服务所需要取用的数据目录。常见的服务例如 WWW，FTP 等。

/sys

SYStem

虚拟文件系统，主要记录与内核相关的信息，包括目前已加载的内核模块与内核检测到的硬件设备信息，同样不占硬盘容量。

/tmp

TeMPorary

公用的临时文件存储点

tty

teletypewriter

“电传打字机”，在类Unix里，键盘显示器，都是虚拟的teletypewriter

/usr

Unix System/Software/Shared Resources

“Unix 操作系统软件资源” 所放置的目录，不是用户数据。 FHS 建议所有的软件开发者应该将他们的数据合理的放置到这个目录下的子目录，而不是自行新建该软件自己独立的目录。

/var

VARiable

某些大文件的溢出区，比方说各种服务的日志文件

注：具体到各个目录的功能以及作用，可以参考下面两篇文章：

*   [Linux中常用的命令都是哪些单词的缩写？](https://www.zhihu.com/question/49073893)
*   [(转)linux 目录结构及其含义](http://www.cnblogs.com/zhyryxz/archive/2012/05/03/2480242.html)(这篇比较详细)

2\. 命令缩写
--------

缩写

全称

说明

apt

Advanced Packaging Tool

是Debian Linux发行版中的APT软件包管理工具。一般配合apt-get或者apt-update

awk

Aho Weiberger and Kernighan

Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的Family Name的首字符。一种编程语言，用于在linux/unix下对文本和数据进行处理。

bash

Bourne Again SHell

一种shell

bg

BackGround

用于将作业放到后台运行，使前台可以执行其他任务。该命令的运行效果与在指令后面添加符号&的效果是相同的，都是将其放到系统后台执行。

cal

CALendar

用于显示当前日历，或者指定日期的日历。

cat

CATenate

"连接"，连接文件并打印到标准输出设备上，cat经常用来显示文件的内容，类似于下的type命令。

chgrp

CHange GRouP

用来改变文件或目录所属的用户组。

chmod

CHange MODe

用来变更文件或目录的权限。

chown

CHange OWNer

改变某个文件或目录的所有者和所属的组，该命令可以向某个用户授权，使该用户变成指定文件的所有者或者改变文件所属的组。

cd

Change Directory

切换工作目录

cp

CoPy

将一个或多个源文件或者目录复制到指定的目的文件或目录

dd

Data Description

用于复制文件并对原文件的内容进行转换和格式化处理。

df

Disk Free

用于显示磁盘分区上的可使用的磁盘空间。默认显示单位为KB。

du

Disk Usage

查看使用空间的，但是与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看，还是和df命令有一些区别的。

diff

DIFFerence

在最简单的情况下，比较给定的两个文件的不同。

dpkg

Debian PacKaGe

Debian Linux系统用来安装、创建和管理软件包的实用工具。

ed

EDitor

单行纯文本编辑器，它有命令模式（command mode）和输入模式（input mode）两种工作模式。

emacs

Editor MACroS

是由GNU组织的创始人Richard Stallman开发的一个功能强大的全屏文本编辑器，它支持多种编程语言，具有很多优良的特性。(备注：vim大法好！！！)

env

ENVironment

用于显示系统中已存在的环境变量，以及在定义的环境中执行指令。

exec

EXECute

用于调用并执行指令的命令。

fsck

File System Consistency checK, or fuck

用于检查并且试图修复文件系统中的错误。

gawk

Gnu Aho Weiberger and Kernighan

grep

Gnu Regular Expression Print

（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

grub

GRand Unified Bootloader

多重引导程序grub的命令行shell工具。

ifconfig

InterFace CONFIGuration

被用于配置和显示Linux内核中网络接口的网络参数。

init

INITialization

Linux下的进程初始化工具

insmod

INStall Module

用于将给定的模块加载到内核中。

ln

LiNk

用来为文件创件连接，连接类型分为硬连接和符号连接两种，默认的连接类型是硬连接。如果要创建符号连接必须使用"-s"选项。

ls

LiSt

显示目标列表

lsmod

LiSt Module

用于显示已经加载到内核中的模块的状态信息。

man

MANual

Linux下的帮助指令，通过man指令可以查看Linux中的指令帮助、配置文件帮助和编程帮助等信息。一般戏称有问题找男人。。。

mkdir

MaKe DIRectory

创建目录

mkfs

MaKe FileSystem

用于在设备上（通常为硬盘）创建Linux文件系统。

mv

MoVe

对文件或目录重新命名，或者将文件从一个目录移到另一个目录中。

nano

Nano's ANOther editor

是一个字符终端的文本编辑器，有点像DOS下的editor程序。

parted

PARTition EDitor

是由GNU组织开发的一款功能强大的磁盘分区和分区大小调整工具，与fdisk不同，它支持调整分区的大小。

passwd

PASSWorD

用于设置用户的认证信息，包括用户密码、密码过期时间等。

ping

Packet InterNet Grouper

用来测试主机之间网络的连通性。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

popd

POP from Directory

删除目录栈中的记录；

pushd

PUSH to Directory

是将目录加入命令堆叠中。

ps

Processes Status

报告当前系统的进程状态。可以搭配kill指令随时中断、删除不必要的程序。

pwd

Print Working Directory

以绝对路径的方式显示用户当前工作目录

rcconf

Run Command CONFiguration

Debian Linux下的运行等级服务配置工具，用以设置在特定的运行等级下系统服务的启动配置。

rm

ReMove

可以删除一个目录中的一个或多个文件或目录，也可以将某个目录及其下属的所有文件及其子目录均删除掉。对于链接文件，只是删除整个链接文件，而原有文件保持不变。

rmdir

ReMove DIRectory

用来删除空目录。

rmmod

ReMove MODule

用于从当前运行的内核中移除指定的内核模块。

rpm

RPM/Redhat Package Manager

RPM软件包的管理工具。

sed

Stream EDitor

一种流编辑器，它是文本处理中非常中的工具，能够完美的配合正则表达式使用，功能不同凡响。

ssh

Secure SHell

openssh套件中的客户端连接工具，可以给予ssh加密协议实现安全的远程登录服务器。

su

Substitute User

"替代用户"，切换当前用户身份到其他用户身份，变更时须输入所要变更的用户帐号与密码。

sudo

SuperUser DO

用来以其他身份来执行命令，预设的身份为root。

sync

SYNChronize

用于强制被改变的内容立刻写入磁盘，更新超块信息。

vim

vi Improved

是UNIX操作系统和类UNIX操作系统中最通用的全屏幕纯文本编辑器。Linux中的vi编辑器叫vim，它是vi的增强版（vi Improved），与vi编辑器完全兼容，而且实现了很多增强功能。(备注：神一样的编辑器！！！)

yum

Yellow dog Updater, Modified

在Fedora和RedHat以及SUSE中基于rpm的软件包管理器

注：命令的具体解释可以参考这个网站：[Linux命令大全](http://man.linuxde.net/)

3\. 编程相关缩写
----------

缩写

全称

说明

cc

C Compiler

gcc

Gnu Compiler Collection

作为一个软件集被你下载下来编译安装的时候

gcc

Gnu C Compiler

作为一个软件被你调用来编译C程序的时候

g++

Gnu c++ compiler

其实g++只是调用gcc，然后连接c++的库，并且作相应的一些编译设置而已

gcj

Gnu Compiler for Java

gdb

Gnu DeBug

4\. 递归缩写
--------

缩写

全称

说明

GNU

Gnu is Not Unix

PHP

PHP: Hypertext Preprocessor

RPM

RPM Package Manager

WINE

WINE Is Not an Emulator

Wine 是类UNIX系统下运行微软Windows程序的"兼容层"。在Wine中运行的Windows程序，就如同运行原生Linux程序一样，不会有模拟器那样的性能问题。

PNG

PNG's Not GIF

nano

Nano's ANOther editor

5\. 其他缩写
--------

缩写

全称

说明

tar

Tape Archive

“磁带档案卷”

tcl

Tool Command Language

Tcl（发音 tickle）是一种脚本语言。

tty

teletypewriter

“电传打字机”，在类Unix里，键盘显示器，都是虚拟的teletypewriter

tzselect

Time Zone SELECT

rc

Runtime Configuration

比如.vimrc, .bashrc等

6\. 参考资料
--------

*   [Linux中常用的命令都是哪些单词的缩写？](https://www.zhihu.com/question/49073893)
*   [(转)linux 目录结构及其含义](http://www.cnblogs.com/zhyryxz/archive/2012/05/03/2480242.html)
*   [Linux命令大全](http://man.linuxde.net/)
*   [Wine (简体中文)](https://wiki.archlinux.org/index.php/Wine_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
*   [fstab (简体中文)](https://wiki.archlinux.org/index.php/Fstab_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
*   [AWK 简明教程](http://coolshell.cn/articles/9070.html)
*   [缩写习惯](http://i.linuxtoy.org/docs/guide/ch02s02.html)
*   [Tcl](https://zh.wikipedia.org/wiki/Tcl?oldformat=true)
*   [Unix Command Acronyms](http://www.abbreviations.com/acronyms/UNIX)
*   [What is the meaning of .VIMRC Acronym in Computing?](http://www.acronym24.com/vimrc-vim-runtime-configuration-file/)
