---
title: OpenJDK编译
categories:
  - Java
date: 2015-09-04 11:06:12
tags:
  - java
---

近日想深入了解一下Java虚拟机，于是阅读[《深入理解Java虚拟机》](https://book.douban.com/subject/24722612/)这本书；在编译openjdk的时候遇到一些问题。记录如下，以供参考；

<!-- more -->

### 0\. 编译环境（Linux底下可以用sudo lshw -short命令查看）：

1.  Ubuntu 14.04LTS i386
2.  4GB内存；
3.  Intel Core i3 cpu；

 

### 1\. 获取OpenJDK源码

有如下两种方式：

*   下载源码包
    *   从[Source Bundle Release](https://jdk7.java.net/source.html)页面下载打包好的源码，在本地解压即可；（注：我用这种方法试了一下，总是报各种错误，遂弃而不用）
*   从Repository中直接获取源码（推荐，我就是用这种方法编译成功的，这种方法可以获取最新的源码，会修复之前版本的一些bug，但是就是下载下来的时间比较长，总共有400多M，因文件较多，故所需时间比较长，自己大概用了1个多小时的时间）
    *   \# 代码如下：
        hg clone http://hg.openjdk.java.net/jdk7u/jdk7u-dev
        cd jdk7u-dev
        chmod 755 get_source.sh
        ./get_source.sh
        

 

### 2. 构建编译环境

\# Ubuntu下使用如下命令一次安装完成：
sudo apt-get install build-essential gawk m4 openjdk6-jdk libasound2-dev libcups2-dev libxrender-dev xorg-dev xutils-dev x11proto-print-dev binutils libmotif3 libmotif-dev ant

 

### 3. 编译前的一些设置

\# 语言选项，这个必须设置，否则编译好后会出现一个HashTable的NPE错误；
export LANG=C
\# jdk的安装路径，必须设置，注：jvm后面的目录根据个人电脑而异；
export ALT_BOOTDIR=/usr/lib/jvm/java-1.6.0-openjdk-i386/
\# 允许自动下载
export ALLOW_DOWNLOADS=true
\# 比较本次build出来的映像与先前版本的差异。这对我们来说没有意义，必须设置成false
export SKIP\_COMPARE\_IMAGES=true
\# 使用预编译头文件，不加这个编译会更慢一些
export USE\_PRECOMPILED\_HEADER=true
\# 编译结果存放的路径，可以随意设置
export ALT_OUTPUTDIR=/home/wmk/JVM/jdkbuild
\# 如下两个环境变量必须去掉，不然会有诡异的事情发生（:-O）
unset JAVA_HOME
unset CLASSPATH

\# 之后可以执行
\# 下面这行命令不明其意，或许可以不执行；
sh ./jdk/make/jdk\_generic\_profile.sh
\# 检查之前的设置是否全部正确
make sanity

还有一些需要改动的地方：

vim hotspot/make/linux/makefiles/gcc.make
#将其中的 WARNINGS\_ARE\_ERRORS = -Werror 注释掉
vim hotspot/make/linux/Makefile
#将其中的test_gamma全部干掉；不然会出现错误；

 

### 4\. 编译

make 2>&1 | tee build.log
\# 注：命令的意思是将标准错误重定向到标准输出，同时输出到控制台和文件；自己大概编译了一个多小时；

编译成功后大概会有类似这样的输出：

\-\- Build times ----------
Target all\_product\_build
Start 2012-05-06 23:07:41
End 2012-05-06 23:12:30
00:00:04 corba
00:00:14 hotspot
00:00:02 jaxp
00:00:06 jaxws
00:04:19 jdk
00:00:04 langtools
00:04:49 TOTAL
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-

 

### 5\. 其他一些可能的错误

#然后，就到sound出错了
cd jdk/make/javax/sound/jsoundalsa
vim Makefile
#找到CPPFLAGS ，在其结尾，添加 -lasound
#如果再次make还是失败在这里，那么 ，先还原上一个修改， 并拷贝出错的最后一条gcc命令，添加-lasound后执行，应该无错误地pass

#OK，你会遇到最后的错误， corba的什么什么类找不到？！注：/opt/openjdk应该需要根据自己的情况实际进行修改一下；
cp -r /opt/openjdk/build/linux-amd64/corba/classes/* /opt/openjdk/build/linux-amd64/classes/

 

### 6\. 参考文献

*   [成功编译OpenJDK 7u2 ! 哦也！](http://wendal.net/420.html)
*   [ubuntu 下查看电脑配置](http://my.oschina.net/uniquejava/blog/99169)
*   [《深入理解Java虚拟机》](https://book.douban.com/subject/24722612/)
*   [Cannot compile openjdk7 source code on CentOS6.5](http://stackoverflow.com/questions/26804355/cannot-compile-openjdk7-source-code-on-centos6-5)
