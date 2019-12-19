---
title: wordpress主题定制
categories:
  - WordPress
date: 2016-01-22 22:16:17
tags:
  - wordpress
---

1. 行高、字符间距设置

参考[雅黑字体下WordPress 行高与字符间距最佳实践：1.8em与0.06em](http://zhangzhao.me/wordpress-height-under-ya-black-font-and-character-spacing-best-practice-1-8em-and-0-06em.zz)，修改style.css如下

```
    #main {
    clear: both;
    overflow: hidden;
    padding: 20px 0 0 0; <!--段间距，分别是上、右、下、左间距-->
    }
    #content {
    margin-bottom: 36px;
    }
    #content,
    #content input,
    #content textarea {
    color: #333;
    font-size: 16px;
    line-height: 1.8em; <!--行间距，设置为字体的1.8倍-->
    letter-spacing: 0.06em; <!--字符间距，设置为字体的0.06倍-->
    }
    #content p,
    #content ul,
    #content ol,
    #content dd,
    #content pre,
    #content hr {
    margin-bottom: 20px;
    }
    }
```

2. 引用块格式设置
-----------

参考：[WordPress默认主题Twenty Ten的blockquote引用样式修改](http://kangjian.net/blog/892/)，修改引用的样式如下：

```
    blockquote {
    background:#f9f9f9;
    border-left:10px solid #ccc;
    margin:1.5em 10px;
    padding:.5em 10px;
    width: 90%;
    font: 微软雅黑, Georgia;
    color: #888;
    }
```

3. 设置选中文字后的背景色
---------------

参考：[WordPress中设置被选中文字颜色及背景色](https://www.logcg.com/archives/343.html)，修改引用样式如下：

```
    ::-moz-selection {
    background-color: #822e30; <!--使用酒红色-->
    color: #fff;
    }

    ::selection {
    background-color: #822e30;
    color: #fff;
    }
```

附：

*   [html颜色选择器](http://www.w3schools.com/tags/ref_colorpicker.asp)
*   [RGB取色器](http://xiaohudie.net/RGB.html)

4. 修改文章页面的背景颜色
---------------

修改文章页面的背景颜色，代码如下：

    #wrapper {
    background: #f9f9f5; <!--作业部落编辑器的颜色，RGB的值分别是249,249,245-->
    margin-top: 20px;
    padding: 0 20px;
    }
    

注：使用作业部落编辑器的背景颜色，调出qq截图之后，可以看出背景颜色的RGB值，然后生成颜色代码即可；

5\. 修改侧边栏以及副标题文字颜色
------------------

修改侧边栏以及副标题的颜色以及字体

    body,
    input,
    textarea {
    color: #000000; <!--使用黑色字体-->
    font-size: 14px;
    line-height: 180%;
    }
    

6\. 参考文献
--------

*   [雅黑字体下WordPress 行高与字符间距最佳实践：1.8em与0.06em](http://zhangzhao.me/wordpress-height-under-ya-black-font-and-character-spacing-best-practice-1-8em-and-0-06em.zz)
*   [WordPress默认主题Twenty Ten的blockquote引用样式修改](http://kangjian.net/blog/892/)
*   [WordPress中设置被选中文字颜色及背景色](https://www.logcg.com/archives/343.html)
*   [html颜色选择器](http://www.w3schools.com/tags/ref_colorpicker.asp)
*   [RGB取色器](http://xiaohudie.net/RGB.html)
