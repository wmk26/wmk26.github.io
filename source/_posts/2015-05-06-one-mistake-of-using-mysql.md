---
title: 记一个Mysql小错误
categories:
  - Linux
date: 2015-05-06 11:30:49
tags:
  - linux
  - mysql
---

在Ubuntu下使用Mysql遇到一个小错误，错误提示如下：

Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’ (2)

<!-- more -->

刚刚遇到这个错误的时候，第一反应是直接GOOGLE，搜到了一些方案，基本上是说my.conf文件（我的文件位置是/etc/mysql/）中socket路径错了。于是按照其中的方法查找mysql.sock文件，将socket路径配置正确。但是系统中没有发现mysql.sock文件，又按照[另一些人的方法](http://www.cnblogs.com/wwufengg/articles/mysqld-sock-lost.html)尝试重新生成mysql.sock文件，但是生成的过程中出现了错误。按照错误提示去看官方文档，仍旧无法解决问题。所幸今天再次尝试解决该问题的时候，偶遇[一篇好文章](http://www.cnblogs.com/super-lucky/p/superlucky.html)，文章本身提供的方法并没有解决我的问题。但其中用到的方法却给了我很大的启发意义，最终顺利解决了问题。文章说要学会查看错误日志，于是尝试了一下，才发现所犯错误是那么的低级。

\[ERROR\] /usr/sbin/mysqld: unknown variable 'default-character-set=utf8'

原因是之前设置mysql字符集的时候出现了错误，删除my.conf中自己添加的这一句设置，问题迎刃而解。

写这篇博客只是记录以下自己解决问题的思路，以后再遇到类似的问题时可以借鉴。之前遇到问题时很盲目，一味的查找一劳永逸的方法，结果只触及问题的皮毛，而无法了解问题的本质。对于这个问题，我想我首先应该了解mysql.sock这个文件的作用，以及mysql的运行机制和相关的配置文件等，比如说my.conf文件中相关设置，错误日志的输出文件等。当然，遇到问题先去搜索解决方案依然是最高效最便捷的方法，但通常我们遇到的问题都是很独特的，如果搜索的方法无法解决问题，不妨查看以下程序运行日志，或许可以更便捷的解决问题。

参考：

*   [启动mysql错误解决方案，学会查看错误日志：mysql.sock丢失，mysqld_safe启动报错](http://www.cnblogs.com/super-lucky/p/superlucky.html)
*   [Mysql启动失败Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysq](http://chinacheng.iteye.com/blog/1100999)
*   [mysql字符设置](http://www.cnblogs.com/kxdblog/p/4034252.html)
