---
title: python中if __name__ == "__main__"的理解
url: 821.html
id: 821
categories:
  - Python
date: 2016-01-17 18:06:24
tags:
---

在看很多python代码的时候，都会有这么一句python代码：if **name** == "**main**"，一直不解其意，今日看了一些文章，有点明白了，故记录之；

1\. 开宗明义
--------

注：假设这行语句是在a.py文件中

*   if \_\_name\_\_ == "\_\_main\_\_"用来判断是否直接运行a.py，经常用于测试a.py这个模块；
*   如果在命令行中直接运行python a.py，则程序会执行if语句之后的内容，即此时\_\_name\_\_ == "\_\_main\_\_"；
*   如果在b.py中把a.py作为一个模块导入（import a），则a.\_\_name\_\_就不是\_\_main\_\_了（为a），此时a.py中的if语句之后的内容就不会执行了；此时如果执行python b.py则会检查b.py中的这行语句；

python官方文档有一段代码说的比较明白，如下所示：

if \_\_name\_\_ == "\_\_main\_\_":
    # execute only if run as a script
    main()

2\. 原理
------

每一个模块（一个python文件）都有一个内置属性“**name**”， **name** 的值取决于您如何应用模块。如果 import 一个模块，那么模块**name** 的值通常为模块文件名，如果直接运行模块，**name** 的值将是一个特别缺省"**main**"。 一般可以用来说明模块用法；

3\. \_\_main\_\_.py的作用
----------------------

有以下三种情况： 1). 假设有一个文件夹my_program包含**main**.py文件，如果直接执行python my_program，则会执行**main**.py文件； 2). 假设有一个zip压缩文件，my_program.zip包含**main**.py文件，如果直接执行python my_program.zip，则会执行**main**.py文件； 3). 假设有一个模块文件，my\_program\_module包含**main**.py文件，如果直接执行python -m my\_program\_module，则会执行**main**.py文件；

4\. 参考文献
--------

*   [python中if \_\_name\_\_ == '\_\_main\_\_': 的解析](http://www.cnblogs.com/xuxm2007/archive/2010/08/04/1792463.html)（主要参考文献）
*   [29.4. \_\_main\_\_ — Top-level script environment](https://docs.python.org/3/library/__main__.html)（官方文档）
*   [What is \_\_main\_\_.py?](http://stackoverflow.com/questions/4042905/what-is-main-py)