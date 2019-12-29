---
title: vim快捷键
categories:
  - Linux
date: 2015-03-25 10:30:35
tags:
  - linux,vim
---

最近更新（2015.12.23） 代码排版：

*   对当前行用"=="，多行则是"n=="；
*   gg=G，对全部代码进行排版，等同于IDE中的code format；

<!-- more -->

注释：

*   normal模式下，control + v，移动光标，选中要注释的行，使用“I”进行插入注释，然后按两下ESC即可；
*   normal模式下，control + v，移动光标，选中要取消注释的行（选中两个“//”），使用“d”进行删除；

翻页：

*   整页翻页：CTRL + f/b（f: forword, b: backword）
*   半页翻页：CTRL + d/u（d: down, u: up）

光标位置：

*   zz：光标所处的行居于屏幕中央；
*   zt：光标所处的行居于屏幕最上一行(t=top);
*   zb：光标所处的行居于屏幕最下一行(b=bottom);

vim分屏：

*   shell中：vim -On file1 file2（如vim -O2 file1 file2，大O表示左右分割，小o表示上下分割）
*   vim中： :sp file1（上下分割） 或者 :vsp file1（左右分割）或者Ctrl + W s（上下分割）或者Ctrl + W v（左右分割）
*   切换窗口：Ctrl + W 或者 Ctrl + ww

vim高亮之后去除高亮显示

*   :noh

vim复制，删除，粘贴：

*   y$，复制光标所在的那个字符到该行行尾的所有数据；
*   y0，复制光标所在的那个字符到该行行首的所有数据；
*   d$，删除光标所在的那个字符到该行行尾的所有数据；
*   d0，删除光标所在的那个字符到该行行首的所有数据；
*   使用p命令粘贴的时候，如果复制的不包含换行符，则直接插入行中；否则插入新行；

参考资料：

1.  [vim多行缩进及多行注释](http://blog.sina.com.cn/s/blog_5ac88b350100b4u9.html)
2.  [vim编辑器---批量注释与反注释](http://blog.csdn.net/xiajun07061225/article/details/8488210)
3.  [vim中翻页的命令,这个要记录](http://www.lpfrx.com/archives/365/)
4.  [Vim的分屏功能](http://coolshell.cn/articles/1679.html)
5.  [请问vim中的高亮显示查询内容后如何取消啊？](http://bbs.chinaunix.net/thread-837524-1-1.html)
6.  [vim复制,粘贴,删除,撤销,替换,光标移动等用法](http://blog.51yip.com/linux/1140.html)
