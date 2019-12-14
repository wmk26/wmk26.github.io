---
title: python中判断字符是否是汉字、字母以及数字
url: 907.html
id: 907
categories:
  - Python
date: 2016-03-05 20:07:00
tags:
---

在python处理数据的过程中，有时候需要判断给定的字符是否是汉字、字母以及数字；参考网上的程序，给出如下的示例：

### python2.x

#!/usr/bin/env python
\# -*- coding:utf-8-*-

\# 判断一个unicode是否是汉字
def is_chinese(uchar):         
    if u'\\u4e00' <= uchar<=u'\\u9fff':
        return True
    else:
        return False

\# 判断一个unicode是否是数字
def is_number(uchar):
    if u'\\u0030' <= and uchar<=u'\\u0039':
        return True
    else:
        return False

\# 判断一个unicode是否是英文字母
def is_alphabet(uchar):         
    if (u'\\u0041' <= uchar<=u'\\u005a') or (u'\\u0061' <= uchar<=u'\\u007a'):
        return True
    else:
        return False

\# 判断是否非汉字，数字和英文字符
def is_other(uchar):
    if not (is\_chinese(uchar) or is\_number(uchar) or is_alphabet(uchar)):
        return True
    else:
        return False

if \_\_name\_\_=="\_\_main\_\_":
    ustring=u'中国 人名ａ高频Ａ'
    # 判断是否有其他字符；
    for item in ustring:
        if (is_other(item)):
            break

### python 3.x

#!/usr/bin/env python
\# -*- coding:utf-8-*-

\# 判断一个unicode是否是汉字
def is_chinese(uchar):         
    if '\\u4e00' <= uchar<='\\u9fff':
        return True
    else:
        return False

\# 判断一个unicode是否是数字
def is_number(uchar):
    if '\\u0030' <= and uchar<='\\u0039':
        return True
    else:
        return False

\# 判断一个unicode是否是英文字母
def is_alphabet(uchar):         
    if ('\\u0041' <= uchar<='\\u005a') or ('\\u0061' <= uchar<='\\u007a'):
        return True
    else:
        return False

\# 判断是否非汉字，数字和英文字符
def is_other(uchar):
    if not (is\_chinese(uchar) or is\_number(uchar) or is_alphabet(uchar)):
        return True
    else:
        return False

if \_\_name\_\_=="\_\_main\_\_":
    ustring=u'中国 人名ａ高频Ａ'
    # 判断是否有其他字符；
    for item in ustring:
        if (is_other(item)):
            break

### 备注

1.  python2.x和python3.x代码的主要不同之处在于unicode判断的地方；因为python3.x中默认都是使用unicode编码方式处理的；
2.  unicode 分配给汉字（中日韩越统一表意文字）的范围为 4E00-9FFF （目前 unicode 6.3 的标准已定义到 9FCC ）
3.  \\u4e00-\\u9fff 不包含中文符号，如有需要可参考[维基](http://goo.gl/hyUcT7)

### 参考文献：

*   [python判断unicode是否是汉字，数字，英文，或者其他字符](http://outofmemory.cn/code-snippet/35784/python-test-unicode-chinese-char)（主要参考文献，对其中做了一些细微的修改）
*   [Python 判断汉字](http://luopuya.github.io/2014/03/29/Python%20%E5%88%A4%E6%96%AD%E6%B1%89%E5%AD%97%E5%AD%97%E7%AC%A6)