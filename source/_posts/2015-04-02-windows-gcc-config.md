---
title: Windows下GCC配置
categories:
  - Linux
date: 2015-04-02 09:33:07
tags:
  - linux
---

基本认识：

- gcc最初是一个c语言编译器，现在则是一个包含众多语言的编译器，包括C++、Fortran、Pascal、Objective-C、Java、Ada，以及Go等；（GNU C Compiler -> GNU Compiler Collection）
- windows下比较流行的gcc移植版本主要有三个，MinGW，Cygwin和Djgpp；
    - MinGW：让GCC的Windows移植版能使用Win32API来编程；
    - Cygwin：让Unix-like下的程序代码在Windows下直接被编译；
    - Djgpp：则是想让DOS下也能用上GCC；

因为[MinGW](http://www.mingw.org/)被墙，所以选择了[Cygwin](https://sourceware.org/cygwin/)。。。

安装步骤：参考[http://www.33lc.com/article/7276.html](http://www.33lc.com/article/7276.html)

Cygwin配置：参考[http://oldratlee.com/post/2012-12-22/stunning-cygwin](http://oldratlee.com/post/2012-12-22/stunning-cygwin)

Cygwin配置Vim编辑器：运行setup-x86.exe程序，在选择软件包的那个步骤中搜索vim，安装即可。

Vim 的配置可以参考：[http://www.cnblogs.com/ma6174/archive/2011/12/10/2283393.html](http://www.cnblogs.com/ma6174/archive/2011/12/10/2283393.html)

使用g++编译C++时有可能会提示缺少相关的DLL文件，可以到cygwin/bin下面找到相关的DLL文件，复制到C:/Windows/System32文件夹下即可。

参考资料：

1. [gcc for Windows 开发环境介绍](http://blog.csdn.net/Mobidogs/article/details/1819084)
2. [Cygwin官网](https://sourceware.org/cygwin/)
3. [cygwin安装教程图解：一步一步教你安装设置cygwin](http://www.33lc.com/article/7276.html)
