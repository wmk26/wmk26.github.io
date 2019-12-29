---
title: linux下编译使用命令行编译运行java程序（多个文件或者整个工程）
categories:
  - Java
date: 2016-03-12 17:18:32
tags:
  - java
  - linux
---

近期做毕业设计，在台式机上使用Intellij IDEA跑Java程序，总是卡的不行不行的。所以就想着放到学院的高性能集群上去跑一下，那么问题来了，长期依赖于IDE的调试，差不多忘记如何再命令行上编译程序了。无赖之下，只好重新学习如何再命令行下编译Java程序；经过摸索，终于弄明白一些基本的东西了，记录如下： 注：以下文章大部分来自于这篇英文文章，[How to Compile and Run Java Code from a Command Line](http://www.sergiy.ca/how-to-compile-and-launch-java-code-from-command-line/)，如果能读懂英文，还是建议阅读原文。本文会大部分翻译，然后加上自己的理解以及收集到的相关知识；

<!-- more -->

1\. 任务
------

假设有一个工程，就叫做helloworld好了，helloworld这个文件夹下包含三个文件夹，描述如下：

*   /bin，空文件夹，用于放编译好的.class文件（这个可以任意命名，一般命名为bin）；
*   /lib，包含第三方库文件（一般是一些jar包）；
*   /src，包含java源文件；

我们的任务是从根目录上编译和运行整个工程； 注：以下使用的是Linux系统，如果使用windows系统，唯一的区别在于要把路径分隔符的":"换成";"

2\. 编译Java代码
------------

这一步就是使用javac命令把Java源代码编译成JVM字节码（.class文件）。 假设我们要编译的文件为：Application.java，该文件的相关情况描述如下：

*   文件在名为com.example的package中；
*   依赖jar包为：lib文件夹下的lib1.jar和lib2.jar；
*   编译好的字节码放在bin目录下；

运行如下命令即可编译：

javac -d bin -sourcepath src -cp lib/lib1.jar:lib/lib2.jar src/com/example/Application.java

编译结果会生成bin/com/example/Application.class文件；如果Application.java还依赖其他类文件，那些类文件会被自动编译； 注：如果依赖其他类文件，则需要加上，-sourcepath src，让javac知道到哪里去找这些类文件； javac常用参数说明：

*   -d，指定编译后class文件的存放位置，如果不指定，则编译好的class文件和源文件放在同一个目录下；
*   -classpath，可以简写成-cp，指定编译所需的class文件的位置，比如jar包、zip文件或其他包含class文件的目录，会覆盖系统的CLASSPATH变量；
*   -sourcepath，指定编译所需的源文件的位置；
*   -encoding，指定源文件使用的字符编码（暂时没有用到过）；

3\. 运行Java代码
------------

这一步是使用java命令运行编译好的.class文件； 假设我们当前在工程的根目录中，运行如下命令即可：

java -cp bin:lib/lib1.jar:lib/lib2.jar com.example.Application

注：这里提供的是类的名称而不是类的整个文件名； 一些自己用到过的参数说明：

*   -Xms，设置虚拟机可用内存堆的初始大小，缺省单位为字节，该大小为1024的整数倍并且要大于1MB，可用k(K)或m(M)为单位来设置较大的内存数。初始堆大小为2MB。 例如：-Xms6400K，-Xms256M
*   -Xmx，设置虚拟机内存堆的最大可用大小，缺省单位为字节，该值必须为1024整数倍，并且要大于2MB。可用k(K)或m(M)为单位来设置较大的内存数。缺省堆最大值为64MB。 例如：-Xmx81920K，-Xmx80M

4\. 关于classpath
---------------

让我们假设在编译Application.java的过程中，编译器卡在了com.example.Util类上，那么如何在文件系统里找到这些类呢？根据Java的命名规则，Util类必定是放在/com/example文件夹下的，但是如何开始去搜寻这个路径呢？这时候，classpath就派上用场了，classpath可以用如下三种方式设置：

*   如果没有 --classpath参数（或者简写成 -cp），就会使用CLASSPATH环境变量；
*   如果CLASSPATH环境变量没有找到，就会默认把当前目录当成classpath；
*   如果 --classpath被显式设定，那么它将会覆盖所有其他的值；

如果classpath设置不当，可能会造成一些无法预料的错误，举例如下： 比如，我们没有使用任何第三方的库，只是使用自己的com.example.Util类，同时尝试从src目录下编译Application.java文件，命令如下：

javac com/example/Application.java

这没问题，但是当我们需要依赖第三方库的时候，此时把库添加到classpath中，命令如下：

javac -cp lib/lib1.jar com/example/Application.java

这将会造成一个错误：

package com.example.Util does not exist

原因在于当我们设置 -cp lib/lib1.jar的时候我们覆盖了classpath的默认值（当前目录）。现在编译器只会去查找lib1.jar里面的所有文件。此时我们需要显式地将当前目录添加到classpath中；命令如下：

javac -cp .;lib/lib1.jar com/example/Application.java

5\. 建议
------

如果要编译多个java文件，或者编译整个工程，建议写一个shell脚本（在windows底下就协议贯彻bat批处理程序即可），

6\. 参考文献：
---------

*   [How to Compile and Run Java Code from a Command Line](http://www.sergiy.ca/how-to-compile-and-launch-java-code-from-command-line/)
*   [如何用javac 和java 编译运行整个Java工程](http://blog.csdn.net/huagong_adu/article/details/6929817)
*   [ubuntu下使用java、javac命令行编译工程](http://blog.csdn.net/wen294299195/article/details/39964971)
*   [Java命令行运行参数说明大全(偷来的)](http://xinklabi.iteye.com/blog/837435)
