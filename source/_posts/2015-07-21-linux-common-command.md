---
title: linux常用命令
url: 199.html
id: 199
categories:
  - Linux
date: 2015-07-21 10:58:23
tags:
---

用linux命令的时候很容易忘，以至于每次用的时候都得重新搜索，此处记录自己经常使用的一些命令，方便自己查找。

<!-- more -->

### **1\. sort命令**

*   -u：unique，去除重复行
*   -t：指定字段分隔符
*   -k：指定按第几列来排序
*   -r：反向排序

使用示例：sort -t ":" -k 3 /etc/passwd（如果以tab键为分割，使用-t$'\\t'） 在使用的使用遇到如下问题：

$ sort -n -t $'\\t' output.txt >output_sort.txt
sort: 字符串比较出现错误: Invalid or incomplete multibyte or wide character
sort: 请设置LC_ALL='C' 以避免出现问题。
sort: 要比较的字符串为"0\\t-1\\t鎺ㄨ繘澶х┖娓湴鍖鸿鍒掑缓璁\\276 鍔犲揩鎵撻€犲煄甯傛柊鐨勫闀挎瀬\\t\[鎺ㄨ繘/v, 澶\\247/a, 绌烘腐/n, 鍦板尯/n, 瑙勫垝/n, 寤鸿/vn,  /nr, 鍔犲揩/v, 鎵撻€\\240/v, 鍩庡競/n, 鏂\\260/a, 鐨\\204/uj, 澧為暱鏋\\201/n\]" 和"0\\t1\\t鍘熸爣棰橈細绀句細鏁戝姪浣撳埗鏈哄埗鏈夊緟瀹屽杽\\t\[鍘\\237/b, 鏍囬/n, 锛\\232/w, 绀句細/n, 鏁戝姪/v, 浣撳埗/n, 鏈哄埗/n, 鏈夊緟/v, 瀹屽杽/v\]"。

解决方案：在终端输入命令：export LC_ALL=C，然后再排序即可； 多列排序，命令如下： sort -t ' ' -k1,1 -k2n,2 data 说明：

*   其中 -t ' ' 指定使用空格分列
*   -k1,1 指定以第一列为关键字排序
*   -k2n,2 指定以第二列为关键字做数据排序

### **2\. make 2>&1 | tee build.log**

*   1,2表示的是文件描述符。（0 stdin，1 stdout，2 stderr）
*   2>&1，表示标准错误重定向到标准输出；
*   tee的作用同时输出到控制台和文件；

### **3\. find**

find命令的常用形式可以简化为：

find \[path...\] \[expression\]

*   path：find命令所查找的目录路径。例如用.来表示当前目录，用/来表示系统根目录
*   expression：expression可以分为——“-options \[-print -exec -ok ...\]”
*   -options，指定find命令的常用选项
*   -print，find命令将匹配的文件输出到标准输出
*   -exec，find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' { } \\;，注意{ }和\\；之间的空格

find命令常用选项：

*   -name，按照文件名查找文件。如：find . -name "*.c" 在当前目录及其子目录（用“.”表示）中查找任何扩展名为“c”的文件；
*   -perm，按照文件权限来查找文件，如：find . -perm 755 –print 在当前目录下查找文件权限位为755的文件；
*   -prune，使find命令不在当前指定的目录中查找；
*   -user：按照文件属主来查找文件。

详细参考[这篇文章](http://www.cnblogs.com/skynet/archive/2010/12/25/1916873.html)；

### **4\. cd**

*   最近才发现cd还有一个比较实用的命令，cd - ：表示回到上次所在目录；

### **5\. scp**

scp命令用于两台机器之间复制文件；

#复制文件，指定用户名，需要输入密码
scp local\_file remote\_username@remote\_ip:remote\_file
#复制文件夹
scp -r local\_folder remote\_username@remote\_ip:remote\_folder

#实现无密码传输
#本地机器运行如下命令，一路回车即可
ssh-keygen -t rsa
#在~目录下生成.ssh目录，该目录下有三个文件；
id_rsa         #私钥
id_rsa.pub     #公钥
known_hosts    #已知主机
#用scp将公钥复制到另一台机器上面，同时修改文件名为authorized_keys
scp ~/.ssh/id\_rsa.pub remote\_username@remote\_ip:~/.ssh/authorized\_keys

### **6\. 进程后台运行**

\# 总共有三种方法，举例说明
\# 1\. nohup
nohup ping www.ibm.com 

\# 2\. setsid
setsid ping www.ibm.com

\# 3\. &
ping www.ibm.com &

\# 前台运行程序改成后台运行
\# 先暂停任务
ctrl + z
\# 查看任务详情
jobs
\# 任务后台运行
bg %jobnumber 将任务放到后台中去处理

具体参考：[Linux 技巧：让进程在后台可靠运行的几种方法](https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/#ibm-pcon)，介绍的非常详细； 另，还可以参考，[linux命令后台运行](http://www.cnblogs.com/lwm-1988/archive/2011/08/20/2147299.html)

### **7. 删除已输入的命令**

经常会有输入错了一个命令，然后狂按backspace的经历。后来才发现又快捷键来做

ctl + u 删除光标以前的字符
ctl + k 删除光标以后的字符
ctl + a 移动光标至的字符头
ctl + e 移动光标至的字符尾
ctl + l 清屏

具体参考：[linux 删除已输入的命令](http://www.xiaojian.org/article/305.html) 参考文章：

1.  [7.2.2 排序命令：sort](http://book.51cto.com/art/200808/85110.htm)
2.  [Unix Sort with Tab Delimiter](http://stackoverflow.com/questions/1037365/unix-sort-with-tab-delimiter)
3.  [linux sort 多字段排序](http://blog.chinaunix.net/uid-25504049-id-3075909.html)
4.  [Veritas™ Dynamic Multi-Pathing 版本说明](http://static-sort.symanteccloud.com/public/documents/sfha/6.0.1/linux/productguides/zh/cn/pdf/dmp_notes_601_lin.pdf)
5.  [make 2>&1 | tee build.log 求解释](http://bbs.chinaunix.net/thread-2331765-1-1.html)
6.  [【日常小记】linux中强大且常用命令：find、grep](http://www.cnblogs.com/skynet/archive/2010/12/25/1916873.html)
7.  [linux cd命令详解及cd命令的几个小技巧](http://os.51cto.com/art/200907/140868.htm)
8.  [18\. scp 跨机远程拷贝](http://linuxtools-rst.readthedocs.org/zh_CN/latest/tool/scp.html)
9.  [SCP命令设置无密码](http://david-je.iteye.com/blog/1960302)
