---
title: MapReduce过程详解
categories:
  - Linux
date: 2016-09-08 18:54:18
tags:
  - hadoop
  - linux
---

关于MapReduce的详细过程一直不是很懂，看了很多资料也是云里雾里的，不过还是找到两篇不错的文章：

*   [MapReduce 过程详解](http://www.cnblogs.com/npumenglei/p/3631244.html)(原理解释的很清晰，强烈建议阅读，本篇文章也是主要参考这篇文章来的)
*   [Hadoop排序工具用法小结](http://www.dreamingfish123.info/?p=1102)(主要是说如何排序，里面的例子也是很清晰的)

自己尝试将上面两篇文章结合起来，并依据自己的实践来进行一些注解； ![mapreduce workflow](http://images.cnitblog.com/i/361346/201404/041657025158483.png) MapReduce总体流程

0\. 综述
------

整个MapReduce过程总共分为七个步骤，分别是Map/Partition/Sort/Combine/Copy/Merge/Reduce。简单来说，Map对文件进行切分，Partition将文件根据一定条件放到不同的桶中，Sort对桶中的数据进行排序，Combine对桶中的数据进行一次合并，以减少数据的传输消耗，Copy是将桶中的数据分发到各个不同的Reduce节点上，Merge是在Reduce节点上对来自不同桶中的数据进行一次合并，Reduce对合并之后的文件进行处理；

1\. Map
-------

拆解，在此阶段可以定义mapper脚本程序，处理文件，得到mapper的输出；

2\. Partition
-------------

分区，分区的原因在于，有时候会有多个Reducer, Partition就是提前对输入进行处理，将同类数据放到一个区中，可以在hadoop脚本中自定义如何做Partition，比如按照输入数据的第1列做partition，那么第一列相同的数据会被分到同一个Partition中去，同一个Partition中第一列的值可能有多个，但是第一列值相同的数据不会出现在多个Partition中。

3\. Sort
--------

排序，对每一个分区的数据进行排序，可以在hadoop脚本中自定义如何做Sort，即指定字段分隔符以及以哪几个字段作为排序的key。如果不指定，则默认以整行为key，value默认为空；

4\. Combine
-----------

Combine可以理解为一个mini Reduce过程，它发生在前面Map的输出结果之后， 目的就是在结果送到Reducer之前先对其进行一次计算，以减少文件的大小，方便后面的传输。但这步也不是必须的。

5\. Copy
--------

Copy阶段，把输出结果传送给Reducer，通过http的方式， 由Reducer节点向各个mapper节点下载属于自己分区（Partition）的数据。

6\. Merge
---------

此时Reducer得到的文件是从不同Mapper那里下载到的，需要对他们进行合并为一个文件（猜想可能是一个Merge Sort的过程，因为最后输出到Reducer的也都是排好序的）；

7\. Reduce
----------

运行Reducer程序对数据进行处理，然后输出，有多少个Reducer就会输出多少个结果文件，一般的命名方式是part-000xx；

总结：
---

Partition, Sort, Combine, Copy, Merge等. 而这些过程往往被统称为"Shuffle" 也就是 “混洗”. 而Shuffle的目的就是对数据进行梳理，排序，以更科学的方式分发给每个Reducer,以便能够更高效地进行计算和处理。 整个MapReduce过程中其实包含两次Sort，一次在Partition之后进行Sort，一次是在Merge的时候进行Sort，将不同的Partition混合到一个Reducer中进行一次Merge Sort；

hadoop中的参数详解
------------

```
    # 典型的参数配置：
    bin/hadoop streaming -input /tmp/comp-test.txt -output /tmp/xx -mapper "python mapper.py" -reducer "python reducer.py" \
    -partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner \
    -jobconf mapred.output.key.comparator.class=org.apache.hadoop.mapred.lib.KeyFieldBasedComparator \
    -jobconf stream.num.map.output.key.fields=3 \
    -jobconf stream.map.output.field.separator=. \
    -jobconf map.output.key.field.separator=. \
    -jobconf mapred.text.key.partitioner.options=-k2,3 \
    -jobconf mapred.text.key.comparator.options="-k3,3 -k4nr" \
    -jobconf mapred.reduce.tasks=5 \
    -file ./mapper.py \
    -file ./reducer.py \
    -jobconf mapred.map.tasks=100 \
    -jobconf mapred.reduce.tasks=100
    
    # 参数详解
    stream.num.map.output.key.fields        # 设置map输出的前几个字段作为key，
    stream.map.output.field.separator       # 设置map输出的字段分隔符，默认是"\t"
    map.output.key.field.separator          # 设置key内的字段分隔符
    num.key.fields.for.partition            # 设置key内前几个字段用来做partition
    mapred.text.key.partitioner.options     # 设置key内某个字段或者某个字段范围用做partition，如上面的例子就是指定key内的第二列和第三列做partition，还可以只设置一列，如设置第二列，则为-k2,2;
    mapred.text.key.comparator.options      # 设置key中需要比较的字段或字节范围，上面的例子解释为：把前四列都作为key，前两列做partition，排序依据优先依据第三列正序(文本序)，第四列逆序(数字序)的组合排序。
    jobconf mapred.map.tasks                # 设置map任务数
    jobconf mapred.reduce.tasks=100         # 设置reduce任务数
```
    
    注0:
    
    KeyFieldBasePartitioner只影响分桶并不影响排序。
    
    注1:
    
    mapred.text.key.partitioner.options和num.key.fields.for.partition不需要一起使用，一起使用则以num.key.fields.for.partition为准。
    
    注2:
    
    此处有一个需要注意的地方，如果这个数字设置的比key中的字段数目多，则只会有一份partition；比如stream.num.map.output.key.fields为1，jobconf mapred.text.key.partitioner.options=-k2,3，则只会产生一份partition；需要特别注意；
    
    注3:
    
    如果不指定Partition字段，则使用整个key作为Partition
