---
title: vps配置记录&&博客搬家
categories:
  - Linux
date: 2016-01-07 13:48:12
tags:
  - linux
---

之前一直使用高大上的linode，但是经常会无法访问，一直让自己很不爽。听说linode的日本机房挺快的，不过目前好像没有资源了。前几天，看见知乎上一个问答：[有哪些便宜稳定，速度也不错的Linux VPS 推荐？](https://www.zhihu.com/question/20800554/answer/71397836)，里面有人推荐vultr，见猎心喜，于是准备尝试一下；现在vultr做活动，新用户注册，送50$，让你免费体验2个月（需要双币信用卡），简直爽歪歪。于是去薅“万恶的”资本主义的羊毛。之后花了大概一天的时间，搭建了梯子，做了相关优化，同时把linode上面的个人网站转移过来。期间遇到不少问题，记录之；

1\. vultr基本设置：
--------------

*   vultr部署之后会默认初始化一个root密码，使用不方便，修改之：

sudo passwd root # 修改root密码

*    .bashrc .vimrc（vim需要自己安装）配置： 直接scp命令从自己的ubuntu上复制过去（找时间上传到github上再贴地址）；

2\. shadowsocks配置：
------------------

参考以下两篇文章：

*   [shadowsocks（影梭）不完全指南](http://www.auooo.com/2015/06/26/shadowsocks%EF%BC%88%E5%BD%B1%E6%A2%AD%EF%BC%89%E4%B8%8D%E5%AE%8C%E5%85%A8%E6%8C%87%E5%8D%97/)
*   [在 Ubuntu 服务器上部署 shadowsocks](http://felixqu.com/2014/06/23/setup-shadowsocks-on-ubuntu/)

如果不想看上面的文章，直接安装下面的步骤来也可以：

1）基本工具安装：sudo apt-get install python-gevent python-pip
2）shadowsocks安装：sudo pip install shadowsocks
3）配置：新建一个配置文件，比如在/etc下新建一个shadowsocks.json文件，文件内容如下：
{
"server":"你的服务器地址",
"port_passwd":{
"服务器端口号":"密码",
...（可以添加多个）
}
"local_address": "127.0.0.1",
"local_port":1080,
"timeout":300,
"method":"aes-256-cfb",
"fast_open": true
}
备注：这篇文章（http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/）推荐使用rc4-md5加密的方式，尝试了一下，不行。google了一下，说这种加速在路由器上会很明显，一般的速度还是靠网速。。。
4）启动（后台启动）：nohup ssserver –c /etc/shadowsocks.json > log &
5）配置开机自动启动：打开/etc/rc.local，添加一行：/usr/local/bin/ssserver –c /etc/shadowsocks.json
6）升级shadowsock命令：sudo pip install –upgrade shadowsocks

3\. shadowsocks加速：
------------------

主要参考这篇文章：[科学上网之 Shadowsocks 安装及优化加速](http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/) 备注：其中涉及到使用锐速加速，需要更改内核，参考这篇文章：[Ubuntu 14.04 更换内核以安装锐速](https://prinzeugen.net/ubuntu-14-04-change-kernel-to-install-serverspeeder/)

4\. 个人博客相关设置
------------

*   更改dns服务商，改狗爹的nameserver为国内的dnspod，主要参考如下文章： [如何让Godaddy域名使用DNSPod做DNS解析](http://bigc.at/godaddy-domain-dnspod-dns.orz)

备注：需要有如下需要注意的地方：

*   记录类型A表示ipv4的类型；
*   主机记录@表示主域名，也就是不带www的域名，对应本网站就是wangmingkuo.com，也就是添加了这一条记录，访问wangmingkuo.com会解析到对应的记录值；
*   如果设置二级域名，则主机记录填写二级域名如blog，记录类型为CNAME，记录值就填要解析的ip地址或者其他网站地址；
*   为了让www.wangmingkuo.com也解析到wangmingkuo.com，可以设置二级域名为www，记录类型为CNAME，记录值为wangmingkuo.com即可；

5\. wordpress搬家（linode->vultr）
------------------------------

*   在vultr上搭建LNMP环境：[使用一键安装LNMP](http://lnmp.org/install.html)（注：建议记录一下个人配置，如mysql账户密码等）
*   一般来说，一个虚拟主机就是一个网站，因此需要开启虚拟主机，参考：[LNMP添加、删除虚拟主机及伪静态使用教程就可以了](http://lnmp.org/faq/lnmp-vhost-add-howto.html)，下面是自己的一些配置，仅供参考：

添加了两个域名，www.wangmingkuo.com wangmingkuo.com 
使用默认的网站目录
Allow Rewrite rule? y
rewrite: wordpress
Allow access_log? y
access log name: default
create database and MySQL user with same name: y
mysql root password: 你的mysql密码
Enter database name: 如果是vps之间搬家，最好保持数据库名称一致，如果第一次安装，就不必在意了（wmk_blog）
Please enter password for mysql user wmk\_blog: wmk\_blog

*   以下说明如何从linode搬家到vultr；如果第一次在vps安装，则可以跳过；主要参考：[WordPress搬家全攻略](http://now4live.com/wordpress%E6%90%AC%E5%AE%B6%E5%85%A8%E6%94%BB%E7%95%A5/)

a. 导出wordpress数据库（注意-u和-p后面不要有空格，下同）：
mysqldump -u数据库用户名 -p数据库密码 要导出的Wordpress数据库名称>backup.sql

b. 打包wordpress文件
tar -czf wmk.tar.gz /home/wwwroot/www.wangmingkuo/

c. 文件数据拷贝到新的vps上，直接用scp命令，参考如下：
scp backup.sql wmk.tar.gz root@新vps的ip地址:新vps上的地址
示例：
scp backup.sql wmk.tar.gz root@1.1.1.1:/home/wwwroot/

d. 把wmk.tar.gz加压缩到www.wangmingkuo.com文件夹下：
tar -xzvf wmk.tar.gz

备注可能会遇到的问题：
问题：以上操作之后，如果要改变/home/wwwroot/www.wangmingkuo.com文件夹的内容，会提示"Operation not permitted"，解决：chattr -i www.wangmingkuo.com，
原因：www.wangmingkuo.com这个文件夹被锁定了，参数i的意思是设定文件不能被删除/改名/设定链接关系，同时不能写入或新增内容，-号的意思是去除这个限制；

e. 导入数据库
mysql -u数据库用户名 -p数据库密码 数据库名称<backup.sql
备注：注意，需要保证搬家前和搬家后的数据库名称相同；另如果导入之后有问题，那么可能是数据库的格式不太对，可以尝试进入LNMP安装的myPHPadmin种手动导入。

f. 设置域名转向新的vps

g. 内存优化，暂时还没有尝试，参考下面这篇文章：
小内存VPS优化 – 如何搭建个人网站（http://keenwon.com/436.html）

备注：可能会遇到的问题

问题1 ：Wordpress的插件，主题无法更新
原因：LNMP一键包环境中对于PHP权限的设置限制
解决：
1\. 编辑/usr/local/php/etc/php.ini文件
2\. 找到disable_functions这一行中，删除"scandir,"这一段脚本，然后保存这个文件退出
3\. 运行 /etc/init.d/php-fpm restart 命令，重启PHP-FPM

参考文献：
-----

*   [有哪些便宜稳定，速度也不错的Linux VPS 推荐？](https://www.zhihu.com/question/20800554/answer/71397836)
*   [shadowsocks（影梭）不完全指南](http://www.auooo.com/2015/06/26/shadowsocks%EF%BC%88%E5%BD%B1%E6%A2%AD%EF%BC%89%E4%B8%8D%E5%AE%8C%E5%85%A8%E6%8C%87%E5%8D%97/)
*   [在 Ubuntu 服务器上部署 shadowsocks](http://felixqu.com/2014/06/23/setup-shadowsocks-on-ubuntu/)
*   [科学上网之 Shadowsocks 安装及优化加速](http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/)
*   [Ubuntu 14.04 更换内核以安装锐速](https://prinzeugen.net/ubuntu-14-04-change-kernel-to-install-serverspeeder/)
*   [如何让Godaddy域名使用DNSPod做DNS解析](http://bigc.at/godaddy-domain-dnspod-dns.orz)
*   [使用一键安装LNMP](http://lnmp.org/install.html)
*   [LNMP添加、删除虚拟主机及伪静态使用教程就可以了](http://lnmp.org/faq/lnmp-vhost-add-howto.html)
*   [WordPress搬家全攻略](http://now4live.com/wordpress%E6%90%AC%E5%AE%B6%E5%85%A8%E6%94%BB%E7%95%A5/)
*   [（总结）Linux的chattr与lsattr命令详解](http://www.ha97.com/5172.html)
*   [解决LNMP环境无法显示所有WordPress主题及无法编辑主题页面](http://www.itbulu.com/lnmp-scandir.html)
