---
title: wordpress修改记录
categories:
  - WordPress
tags:
  - wordpress
date: 2015-01-02 20:00:48
---

1.  文章编辑器：一开始使用的是wordpress默认的编辑器，功能太少，换用kindeditor-for-wordpress，试用了一段时间，功能虽多，体验太差。现在使用的是TinyMCE Advanced，目前感觉还不错。
2.  添加qq表情：使用kindeditor-for-wordpress 的一个好处是，里面内嵌了qq的默认表情，换用TinyMCE Advanced后，根据[这篇文章](http://www.annhe.net/article-1478.html)添加了qq表情。（备注：kindeditor-for-wordpress中的表情更多，下载该插件到本地，表情文件位于/kindeditor-for-wordpress/plugins/emoticons/images，以后有时间尝试把qq表情嵌入到TinyMCE Advanced编辑器中）
3.  首页显示文章摘要：参考的是[这篇博文](http://www.zrblog.net/13.html)，使用的插件是wp-utf8-excerpt，主要特点：1. 支持多字节语言（如中文），不会产生乱码；2.摘要可保留文章中的格式标签，如字体、颜色、链接、图片等（需保留的标签可在后台设置）；3. 首页每篇文章显示300字，存档页面每篇文章显示150字（字数可设置）
4.  实现按照修改日期对文章进行排序：参考文章：[实现WordPress文章按修改时间排序](http://i7298.com/archives/55)遇到了一些问题，待解决;
5.  无意间搜到另一个代码高亮工具，大赞！！！Crayon Syntax Highlighter，强烈推荐！！！下载量与SyntaxHighlighter Evolved相差无几，但是评论量接近SyntaxHighlighter Evolved的三倍，且是全五星（SyntaxHighlighter Evolved四星半）。代码编辑远胜于SyntaxHighlighter Evolved。之前一直看别人的推荐使用SyntaxHighlighter Evolved，而且用搜索引擎搜索也都是这货，于是被坑的遍体鳞伤，身心俱疲。看来，别人的推荐还真不一定靠谱，以后得自己尝试了之后才能做评价，且要货比三家，不能只关注大众的观点！！！亦不能迷信权威和大多数，血与泪的教训！希望自己谨记。其中的配置可以参考[这篇文章](http://www.ipeld.net/archives/3765.html)；设置成SyntaxHighlighter Evolved的形式。
6.  在底部添加pv和uv，参考[这篇文章](http://ibruce.info/2015/04/04/busuanzi/)；
7.  使用WordPress返回顶部插件wp auto top，参考[这篇文章](http://jingyan.baidu.com/article/5d6edee228118899eadeec00.html)；
8.  [本站建站技术合集](http://codefine.co/%E6%80%BB%E7%BB%93%E4%B8%80%E4%B8%8B%E6%9C%AC%E7%AB%99%EF%BC%88wordpress%EF%BC%89%E7%94%A8%E5%88%B0%E7%9A%84%E6%8F%92%E4%BB%B6%E3%80%81%E4%B8%BB%E9%A2%98%E4%BB%A5%E5%8F%8A%E8%87%AA%E5%B7%B1%E5%81%9A/)，不错的合集，可供业余时间折腾；
9.  参考这篇文章：[wordpress主题字体更换](https://www.anotherhome.net/1010)，更换字体；
10.  添加Jetpack，使用其中的Markdown支持，参考文章：[使用Markdown写作WordPress](http://www.itoldme.net/archives/427)；
11.  使用多说评论，直接搜索安装名为的duoshuo插件；
12.  使用文章目录插件，Table of Contents Plus，参考这篇文章配置：[免费好用的文章内容目录wordpress插件](http://www.52souji.net/free-plugin-about-insert-table-of-contents-in-post-for-wordpress/)
13.  使用插件external link，设置全站外部链接在新窗口打开，参考这篇文章：[使用 external links 插件控制wordpress外部链接属性](http://www.yzznl.cn/archives/230.html)
14.  添加社交化分享工具[JiaThis](http://www.jiathis.com/)，可以自定义；
15.  使用插件**谷歌字体与Gravatar头像加速**优化网站加载速度；参考这篇文章：[新手如何系统地优化WordPress博客？](http://www.haoqimao.net/?/question/30)，里面提到可以在网站备案之后启用国内的CDN加速，自己暂时没有备份的打算，以后再考虑；
16.  使用插件**WP Fastest Cache**优化网站加载速度；
17.  使用插件**a3 Lazy Load**后发现图片无法加载，暂时停用；
18.  20160911,添加文档归档页面，参考：[WordPress 归档页面模板\[WP原生函数实现2014版\]](http://zww.me/wordpress-archive-page-template-wp-primary-function-2014-edition.z-turn)
