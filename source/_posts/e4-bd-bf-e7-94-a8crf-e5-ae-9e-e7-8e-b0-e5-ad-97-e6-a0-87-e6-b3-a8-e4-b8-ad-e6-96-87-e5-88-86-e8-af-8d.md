---
title: 使用CRF++实现字标注中文分词
url: 74.html
id: 74
categories:
  - NLP&amp;ML
date: 2014-12-31 10:20:39
tags:
---

近期需要学习CRF，在[我爱自然语言处理](http://www.52nlp.cn/)上看到一篇很详细的介绍[用CRF++做中文分词](http://www.52nlp.cn/%E4%B8%AD%E6%96%87%E5%88%86%E8%AF%8D%E5%85%A5%E9%97%A8%E4%B9%8B%E5%AD%97%E6%A0%87%E6%B3%A8%E6%B3%954)的文章，本文主要是对这篇文章加入自己注解，以加深理解。

  

目录：

[1\. 安装](#ch1)

[2\. 分词模型介绍](#ch2)

   [2.1 训练集（train.data）](#ch2-1)

   [ 2.2 测试集（test.data）](#ch2-2)

   [2.3 特征模板文件（template）](#ch2-3)

   [ 2.4 执行脚本（exec.sh）](#ch2-4)

[3\. 分词实现](#ch3)

    [3.1 准备语料](#ch3-1)

    [3.2 训练语料处理](#ch3-2)

    [3.3 模型训练](#ch3-3)

    [3.4 测试语料处理及测试](#ch3-4)

    [3.5 分词效果评估](#ch3-5)

[4\. 参考资料](#ch4)

1\. 安装
------

安装可以参考这篇博文，[用条件随机场CRF进行字标注中文分词（Python实现）](http://blog.csdn.net/u010189459/article/details/38546115)，此不赘述。

2\. 分词模型介绍
----------

本文利用CRF++ example里的样例进行实验，其中的seg目录对应的是一个日文分词的样例，可以直接套用其中的特征模板。 seg目录下有4个文件：

*   exec.sh（执行脚本）
*   template（特征模板）
*   test.data（测试集）
*   train.data（训练集)

注：可以直接使用给定的样例做实验，直接执行./exec.sh即可。

###   

### 2.1 训练集（train.data）

示例如下：  

1.  **    毎 k B**
2.  **    日 k I**
3.  **    新 k I**
4.  **    聞 k I**
5.  **    社 k I**
6.  **    特 k B**
7.  **    別 k I**
8.  **    顧 k B**
9.  **    問 k I**
10.  **    4 n B **

第一列为日文文字；

第二列为词性标记；

第三列为字标注中的2-tag(B,I)标记（将词首标记设计为B，而将词的其他位置标记设计为I，如“海南岛”可以标记为“海/B 南/I 岛/I”）

注：训练集中句子与句子之间通过空行来分隔。

### 2.2 测试集（test.data）

示例如下：

1.  **    よ h I**
2.  **    っ h I**
3.  **    て h I**
4.  **    私 k B**
5.  **    た h B**
6.  **    ち h I**
7.  **    の h B**
8.  **    世 k B**
9.  **    代 k I**
10.  **    が h B**

第一列为日文文字；

第二列为词性标记；

第三列为字标记中的tag标记，在测试集中主要是占位作用。

备注：**训练集和测试集的格式要求**：

CRF++对于训练集和测试集文件格式的要求是比较灵活的，

首先，需要多列，每一行的列数相同；

其次，第一列代表的是需要标注的“字或词”，最后一列是输出位”标记tag”，如果有额外的特征，例如词性什么的，可以加到中间列里；

因此，训练集和测试集文件最少需要两列。

### 2.3 特征模板文件（template）

    # Unigram

    U00:%x\[-2,0\]

    U01:%x\[-1,0\]

    U02:%x\[0,0\]

    U03:%x\[1,0\]

    U04:%x\[2,0\]

    U05:%x\[-2,0\]/%x\[-1,0\]/%x\[0,0\]

    U06:%x\[-1,0\]/%x\[0,0\]/%x\[1,0\]

    U07:%x\[0,0\]/%x\[1,0\]/%x\[2,0\]

    U08:%x\[-1,0\]/%x\[0,0\]

    U09:%x\[0,0\]/%x\[1,0\]

    # Bigram

    B

关于CRF++中特征模板的说明和举例，请参考官方文档上的“Preparing feature templates”这一节，而以下部分的说明拿上述日文分词数据举例。

在特征模板文件中，每一行(如U00:%x\[-2,0\]）代表一个特征，而宏“%x\[行位置,列位置\]”则代表了相对于当前指向的token的行偏移和列的绝对位置，

以上述训练集为例，如果当前扫描到“新 k I”这一行，

1.  **    毎 k B**
2.  **    日 k I**
3.  **    新 k I < == 扫描到这一行，代表当前位置**
4.  **    聞 k I**
5.  **    社 k I**
6.  **    特 k B**
7.  **    別 k I**
8.  **    顧 k B**
9.  **    問 k I**
10.  **    ４ n B**

依据特征模板文件抽取的特征如下：

    # Unigram

    U00:%x\[-2,0\] ==> 毎

    U01:%x\[-1,0\] ==> 日

    U02:%x\[0,0\] ==> 新

    U03:%x\[1,0\] ==> 聞

    U04:%x\[2,0\] ==> 社

    U05:%x\[-2,0\]/%x\[-1,0\]/%x\[0,0\] ==> 每/日/新

    U06:%x\[-1,0\]/%x\[0,0\]/%x\[1,0\] ==> 日/新/聞

    U07:%x\[0,0\]/%x\[1,0\]/%x\[2,0\] ==> 新/聞/社

    U08:%x\[-1,0\]/%x\[0,0\] ==> 日/新

    U09:%x\[0,0\]/%x\[1,0\] ==> 新/聞

    # Bigram

    B

CRF++里将特征分成两种类型，一种是Unigram的，“U”起头，另外一种是Bigram的，“B”起头。对于Unigram的特征，假如一个特征模板是“U01:%x\[-1,0\]”, CRF++会自动的生成一组特征函数(func1… funcN) 集合:

func1 = if (output = B and feature="U01:日") return 1 else return 0

func2 = if (output = I and feature="U01:日") return 1 else return 0

....

funcXX = if (output = B and feature="U01:問") return 1 else return 0

funcXY = if (output = I and feature="U01:問") return 1 else return 0

生成的特征函数的数目 = (L*N)，其中L是输出的类型的个数，这里是B，I这两个tag，N是通过模板扩展出来的所有单个字符串（特征）的个数，这里指的是在扫描所有训练集的过程中找到的日文字（特征）。（**注：**不是太明白这里N的意义，当前位置会不会影响N的大小，比如说这里当前行是第三行，根据特征模板”U01:%x\[-1,0\]”，生成的特征函数就没有包含”U01:毎“的情况，这里N=8，如果当前行是第二行，N=9？是按行扫描，每一行都会根据特征模板生成L\*N个特征函数？还是与当前行无关，只是根据特征模板生成L\*N个特征函数？） 

而Bigram特征主要是当前的token和前面一个位置token的自动组合生成的bigram特征集合。最后需要注意的是U01和U02这些标志位，与特征token组合到一起主要是区分“U01:問”和“U02:問”这类特征，虽然抽取的日文”字”特征是一样的，但是在CRF++中这是有区别的特征。

### 2.4 执行脚本（exec.sh）

    #!/bin/sh

    ../../crf_learn -f 3 -c 4.0 template train.data model

    ../../crf_test -m model test.data

    ../../crf_learn -a MIRA -f 3 template train.data model

    ../../crf_test -m model test.data

    rm  
-f model

注：参数含义如下：

-a CRF-L2 or CRF-L1

    规范化算法选择。默认是CRF-L2。一般来说L2算法效果要比L1算法稍微好一点，虽然L1算法中非零特征的数值要比L2中大幅度的小。

    备注：CRF++支持single-best MIRA训练，当-a MIRA设置后可以使用MIRA训练。（不知什么是MIRA训练）

-c float

    这个参数设置CRF的hyper-parameter。c的数值越大，CRF拟合训练数据的程度越高。这个参数可以调整过度拟合和不拟合之间的平衡度。这个参数可以通过交叉验证等方法寻找较优的参数。

-f NUM

    这个参数设置特征的cut-off threshold。CRF++使用训练数据中至少NUM次出现的特征。默认值为1。当使用CRF++到大规模数据时，只出现一次的特征可能会有几百万，这个选项就会在这样的情况下起到作用。

-p NUM

    如果电脑有多个CPU，那么那么可以通过多线程提升训练速度。NUM是线程数量。

3\. 分词实现
--------

### 3.1 准备语料

采用[backoff2005](http://sighan.cs.uchicago.edu/bakeoff2005/)里的数据，语料选取的是其中北京大学《人民日报》的语料，采用4-tag(B(Begin，词首),E(End，词尾),M(Middle，词中),S(Single,单字词))标记集，处理utf-8编码文本。

### 3.2 训练语料处理

训练语料：./icwb2-data/training/pku_training.utf8，其中是已经人工分好词的中文句子；

样例：

1.  **    迈向 充满 希望 的 新 世纪 —— 一九九八年 新年 讲话 （ 附 图片 １ 张 ）**
2.  **    中共中央 总书记 、 国家 主席 江 泽民**
3.  **    （ 一九九七年 十二月 三十一日 ）**
4.  **    １２月 ３１日 ， 中共中央 总书记 、 国家 主席 江 泽民 发表 １９９８年 新年 讲话 《 迈向 充满 希望 的 新 世纪 》 。 （ 新华社 记者 兰 红光 摄 ）**
5.  **    同胞 们 、 朋友 们 、 女士 们 、 先生 们 ：  **

处理：将训练语料转换为CRF++训练用的语料格式（2列，4-tag），处理脚本如下：

make\_crf\_train_data.py

\[python\]<br /> #!/usr/bin/env python<br /> # -*- coding: utf-8 -*-<br /> # Author: 52nlpcn@gmail.com<br /> # Copyright 2014 @ YuZhen Technology<br /> #<br /> # 4 tags for character tagging: B(Begin), E(End), M(Middle), S(Single)</p> <p>import codecs #用于编码转换的模块，返回的长度是字数<br /> import sys</p> <p>def character\_tagging(input\_file, output\_file):<br /> input\_data = codecs.open(input\_file, 'r', 'utf-8')<br /> output\_data = codecs.open(output\_file, 'w', 'utf-8')<br /> for line in input\_data.readlines():<br /> word\_list = line.strip().split()<br /> for word in word\_list: #这里的word中单个汉字长度为1<br /> if len(word) == 1: #单个汉字<br /> output\_data.write(word + "tSn")<br /> else: #word中包含多个汉字，对其中的每一个汉字进行BME标注<br /> output\_data.write(word\[0\] + "tBn")<br /> for w in word\[1:len(word)-1\]:<br /> output\_data.write(w + "tMn")<br /> output\_data.write(word\[len(word)-1\] + "tEn")<br /> output\_data.write("n") #使用空行断句<br /> input\_data.close()<br /> output\_data.close()</p> <p>if \_\_name__ == '\_\_main\_\_':<br /> if len(sys.argv) != 3:<br /> print "pls use: python make\_crf\_train\_data.py input output"<br /> sys.exit()<br /> input\_file = sys.argv\[1\]<br /> output\_file = sys.argv\[2\]<br /> character\_tagging(input\_file, output\_file)<br /> \[/python\]

make\_crf\_train\_data\_my.py（自己写的处理脚本，没有用codecs模块，多了一些冗余代码）

\[python\] #!/usr/bin/env python # coding=utf-8 import sys reload(sys) sys.setdefaultencoding('utf-8') def tagging(input, output): input\_file = file(input, 'r+') output\_file = file(output, 'w+') for line in input\_file.readlines(): word\_list = line.strip().split() for word in word\_list: print word if len(word) == 3: output\_file.write(word + "tSn") else: output\_file.write(word\[0:3\] + "tBn") i = 6 while(i &amp;lt; len(word)): output\_file.write(word\[i-3:i\] + "tMn") i += 3 output\_file.write(word\[len(word)-3 : len(word)\] + "tEn") output\_file.write("n") input\_file.close() output\_file.close() if \_\_name\_\_ == '\_\_main\_\_': if len(sys.argv) != 3: print "Usage: python " + sys.argv\[0\] + " input output" sys.exit(-1) tagging(sys.argv\[1\], sys.argv\[2\]) \[/python\]

使用：python char-tagging.py ./icwb2-data/training/pku\_training.utf8 pku\_training_tag.utf8

其中，pku\_training\_tag.utf8为生成的训练文件。

样例如下：

1.  **    迈 B**
2.  **    向 E**
3.  **    充 B**
4.  **    满 E**
5.  **    希 B**
6.  **    望 E**
7.  **    的 S**
8.  **    新 S**
9.  **    世 B**
10.  **    纪 E**
11.  **    — B**
12.  **    — E**
13.  **    一 B**
14.  **    九 M**
15.  **    九 M**
16.  **    八 M**
17.  **    年 E**
18.  **    新 B**
19.  **    年 E**
20.  **    讲 B**
21.  **    话 E**
22.  **    （ S**
23.  **    附 S**
24.  **    图 B**
25.  **    片 E**
26.  **    １ S**
27.  **    张 S**
28.  **    ） S**

### 3.3 模型训练

使用训练工具crf_learn来训练模型，执行如下命令：

crf\_learn -f 3 -c 4.0 template pku\_training\_tag.utf8 crf\_model

注：参数参考上述介绍，crf_model为训练好的模型。训练的时间可能会比较长，小时级别，长短视机器配置而不同。

### 3.4 测试语料处理及测试

测试语料：icwb2-data/testing/pku_test.utf8

样例：

1.  **共同创造美好的新世纪——二○○一年新年贺词**
2.  **（二○○○年十二月三十一日）（附图片1张）**
3.  **女士们，先生们，同志们，朋友们：**
4.  **2001年新年钟声即将敲响。人类社会前进的航船就要驶入21世纪的新航程。中国人民进入了向现代化建设第三步战略目标迈进的新征程。**
5.  **在这个激动人心的时刻，我很高兴通过中国国际广播电台、中央人民广播电台和中央电视台，向全国各族人民，向香港特别行政区同胞、澳门特别行政区同胞和台湾同胞、海外侨胞，向世界各国的朋友们，致以新世纪第一个新年的祝贺！**

处理：脚本如下

crf_segmenter.py

\[python\] #!/usr/bin/env python # -*- coding: utf-8 -*- # Author: 52nlpcn@gmail.com # Copyright 2014 @ YuZhen Technology # # CRF Segmenter based character tagging: # 4 tags for character tagging: B(Begin), E(End), M(Middle), S(Single) import codecs import sys import CRFPP def crf\_segmenter(input\_file, output\_file, tagger): input\_data = codecs.open(input\_file, 'r', 'utf-8') output\_data = codecs.open(output\_file, 'w', 'utf-8') for line in input\_data.readlines(): tagger.clear() for word in line.strip(): #此处word为单个字 word = word.strip() if word:             #将测试语料转化成CRF++要求的格式，3列，后面两列为占位符，无实义。 tagger.add((word + "totB").encode('utf-8')) tagger.parse() size = tagger.size() #一行中字的数量 xsize = tagger.xsize() #为1，不清楚什么意思 for i in range(0, size): #按标注的词位信息将结果转化成分词结果 for j in range(0, xsize): char = tagger.x(i, j).decode('utf-8') tag = tagger.y2(i) if tag == 'B': output\_data.write(' ' + char) elif tag == 'M': output\_data.write(char) elif tag == 'E': output\_data.write(char + ' ') else: # tag == 'S' output\_data.write(' ' + char + ' ') output\_data.write('n') input\_data.close() output\_data.close() if \_\_name__ == '\_\_main\_\_': if len(sys.argv) != 4: print "pls use: python crf\_segmenter.py model input output" sys.exit() crf\_model = sys.argv\[1\] input\_file = sys.argv\[2\] output\_file = sys.argv\[3\] tagger = CRFPP.Tagger("-m " + crf\_model) crf\_segmenter(input\_file, output\_file, tagger) \[/python\]

使用：

python  
crf\_segmenter.py crf\_model ./icwb2-data/testing/pku_test.utf8  
pku\_test\_seg.utf8

样例：

1.  **共同 创造 美好 的 新 世纪 —— 二○○ 一 年 新年 贺词**
2.  **（ 二○○○年 十二月 三十一日 ） （ 附 图片 1 张 ）**
3.  **女士 们 ， 先生 们 ， 同志 们 ， 朋友 们 ：**
4.  **2001 年 新年 钟声 即将 敲响 。 人类 社会 前进 的 航船 就要 驶入 21 世纪 的 新 航程 。 中国 人民 进入 了 向 现代化 建设 第三 步 战略 目标 迈进 的 新 征程 。**
5.  **在 这个 激动人心 的 时刻 ， 我 很 高兴 通过 中国 国际 广播 电台 、 中央 人民 广播 电台 和 中央 电视台 ， 向 全国 各族 人民 ， 向 香港 特别 行政区 同胞 、 澳门 特别 行政区 同胞 和 台湾 同胞 、 海外 侨胞 ， 向 世界 各国 的 朋友 们 ， 致以 新世纪 第一 个 新年 的 祝贺 ！**

### 3.5 分词效果评估

使用backoff2005的测试脚本来评测分词效果。

./icwb2-data/scripts/score  
./icwb2-data/gold/pku\_training\_words.utf8  
./icwb2-data/gold/pku\_test\_gold.utf8 pku\_test\_seg.utf8 > pku_result.score

注： pku\_test\_seg.utf8为使用crf模型得到的分词结果，即第四步的结果。

结果如下：

=== SUMMARY:

=== TOTAL INSERTIONS: 1512

=== TOTAL DELETIONS: 3195

=== TOTAL SUBSTITUTIONS: 5011

=== TOTAL NCHANGE: 9718

=== TOTAL TRUE WORD COUNT: 104372

=== TOTAL TEST WORD COUNT: 102689

=== TOTAL TRUE WORDS RECALL: 0.921

=== TOTAL TEST WORDS PRECISION: 0.936

=== F MEASURE: 0.929

=== OOV Rate: 0.058

=== OOV Recall Rate: 0.553

=== IV Recall Rate: 0.944

可以看出，基于字标注（4-tag）的CRF分词的准确率为93.6%，召回率为92.1%。

4\. 参考资料
--------

[中文分词入门之字标注法4](http://www.52nlp.cn/%E4%B8%AD%E6%96%87%E5%88%86%E8%AF%8D%E5%85%A5%E9%97%A8%E4%B9%8B%E5%AD%97%E6%A0%87%E6%B3%A8%E6%B3%954)

[中文分词入门之字标注法1](http://www.52nlp.cn/%E4%B8%AD%E6%96%87%E5%88%86%E8%AF%8D%E5%85%A5%E9%97%A8%E4%B9%8B%E5%AD%97%E6%A0%87%E6%B3%A8%E6%B3%951)

[用条件随机场CRF进行字标注中文分词（Python实现）](http://blog.csdn.net/u010189459/article/details/38546115)

[CRF++使用小结](http://www.cnblogs.com/pangxiaodong/archive/2011/11/21/2256264.html)