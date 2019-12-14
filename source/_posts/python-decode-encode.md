---
title: Python中的编码问题
url: 64.html
id: 64
categories:
  - Python
date: 2014-12-29 16:46:39
tags:
---

用python处理中文文本的时候一直为编码问题所困扰，近期找个时间好好解决一下这个问题，先拟定题目。 1.ASCII(American Standard Code for Information Interchange) 美国信息交换标准代码，这是计算机上最早使用的通用的编码方案。这种编码占用7个Bit，在计算机中占用一个字节，8位，最高位没用，通讯的时候有时用作奇偶校验位。因此ASCII编码的取值范围实际上是：0x00-0x7f，只能表示128个字符。后来发现128个不太够用，做了扩展，叫做ASCII扩展编码，用足八位，取值范围变成：0x00-0xff，能表示256个字符。 2.ANSI（American National Standard Institite ） 美国国家标准协会，每个国家（非拉丁语系国家）自己制定自己文字的编码规则，并得到了ANSI认可，符合ANSI的标准，这种编码就叫ANSI编码。如中国的ANSI对应的是GB2312标准，日本的是JIT标准，香港和台湾的为BIG5标准。（备注：GBK是微软自己的标准，全面兼容GB2312。） ANSI标准规定ASCII的代码（0x00-0x7f）不能使用，即ASCII码在任何符合ANSI标准的编码中都是相同的。中文的编码可以使用两个字节，编码范围可以为0xA1A1-oxFEFE，可以表示23901个汉字。GBK的编码范围为0x8140-0xFEFE，可以表示3万多个汉字。ANSI编码的字节数不确定，可以是两位，如GB2312，GBK，BIG5。 参考资料：[彻底搞懂字符编码(unicode,mbcs,utf-8,utf-16,utf-32,big endian,little endian...)](http://blog.csdn.net/softman11/artetails/icle/d6124345) 百度百科：信息交换用汉字编码字符集