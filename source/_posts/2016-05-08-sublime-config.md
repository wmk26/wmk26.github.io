---
title: sublime使用配置
tags:
  - sublime
categories:
  - Linux
date: 2016-05-08 18:11:05
---

最近在写前端，经推荐尝试使用sublime 3，记录一下自己的配置。

<!-- more -->

1\. 简介
======

Sublime Text 是一套跨平台的文本编辑器，支持基于Python的插件。说是需要购买注册码，但是不买也是可以使用的。。。推荐使用sublime text3，因为比2要快。

2\. 配置
======

安装就不赘述了。下面记录一下自己的配置：

2.1 安装package control
---------------------

package control类似于debian的apt-get，redhat的yum，有了它能极大得简化插件的安装。ctrl+~（Esc下面那个键）同时按住，弹出一个输入框，粘贴下面代码，回车。 sublime text3

    import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
    

sublime text2

    import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
    

注：最近package control的package仓库貌似被墙了，这里提供一个解决方法：修改hosts文件，添加上一句（没遇到过）：

    50.116.34.243 sublime.wbond.net
    

安装好之后，Preferences菜单下看到Package Settings和Package Control两个菜单了，点击Package Control，install package ，输入相应关键字，就能开始安装插件了。

2.2 插件安装
--------

### 2.2.1 Emmet

HTML、CSS必备；

### 2.2.2 Prettify

HTML、CSS、JS、JSON等可以通过Ctrl+Shift+H 一键进行格式化；

### 2.2.3 Bracket Highlighter

括号匹配高亮以及html标签匹配高亮。默认是在匹配的符号下面加横线，可以修改成高亮形式，方法如下： 在preferences-->package settings-->Bracket highlighter-->Bracket settings-Default会打开一个文件，将"bracket_styles"中的style改为hightlight。 注：自己尝试的时候无法编辑，可以将Bracket settings-Default的内容复制到preferences-->package settings-->Bracket highlighter-->Bracket settings-User中，然后进行修改即可；

2.3 其他问题
--------

### 2.3.1 无法通过package control安装package

无法安装package，提示如下：

An error occurred while trying to unzip the package file for HTML-CSS-JS Prettify. Please try installing the package again.

解决：手动安装package

1.  下载相关的package包，如HTML-CSS-JS Prettify的下载地址为：[https://github.com/victorporof/Sublime-HTMLPrettify](https://github.com/victorporof/Sublime-HTMLPrettify)
2.  Preferences -> Browse Package，找到package安装的目录，解压package包到该目录下面即可，然后重启sublime。

### 2.3.2 Ubuntu下Sublime Text3无法输入中文

参考这篇文章即可解决：[Ubuntu下Sublime Text 3解决无法输入中文的方法](http://jingyan.baidu.com/article/f3ad7d0ff8731609c3345b3b.html)

参考文献
====

*   [Sublime Text](https://www.wikiwand.com/zh-cn/Sublime_Text)
*   [如何优雅地使用Sublime Text3](http://www.jianshu.com/p/3cb5c6f2421c#)
*   [我的Sublime Text 3配置](https://segmentfault.com/a/1190000000770104)（主要参考文献）
*   [SubLime BracketHighlighter 配置](http://www.cnblogs.com/cart55free99/p/3502025.html)
*   [sublime中BracketHighlighter 插件使用](http://www.cnblogs.com/liu-liang/archive/2013/06/09/3129471.html)
*   [Ubuntu下Sublime Text 3解决无法输入中文的方法](http://jingyan.baidu.com/article/f3ad7d0ff8731609c3345b3b.html)
