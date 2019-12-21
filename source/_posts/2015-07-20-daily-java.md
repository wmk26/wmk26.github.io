---
title: 每日Java(20150716-0720)
categories:
  - Java
date: 2015-07-20 15:35:49
tags:
  - java
---

Java中的hash存储机制（以HashMap为主）

<!-- more -->

1\. HashMap实现原理分析
=================

1.1 概述：
-------

看了HashMap源码的说明(HashMap.java)，说明的非常好，尝试先翻译一下：

Hash table是基于Map接口的实现，这个实现提供了map的所有操作，同时允许null values和null key。（HashMap类和Hashtable基本可以等同，除了HashMap是非同步的（unsynchronized）且允许null键值）。HashMap不保证map中的顺序一直不变；

如果假设hash函数可以均匀地把元素分配在桶中，那么这个实现可以提供常量时间的基本操作（get方法和put方法）；在集合层面上的迭代要求时间和HashMap实例的容量（capacity）以及大小（key-value映射对的数目）成比例；因此，如果要求高的迭代性能，最好不要把初始容量设置过高（或者把load factor设置的过低）；

影响HashMap实例性能的有两个参数：初始容量和负载因子（load factor）；初始容量是指创建hash表时分配的容量，load factor是一个衡量指标，当hash表的使用率大于这个指标时，hash表将会自动扩容（rehashed，容量将会翻倍）；load factor默认为0.75，在时间和空间使用率上取得了一个很好的平衡；

注意这个实现是非同步的（not synchronized），如果多个线程同时访问一个hash map， 同时至少一个线程修改了map的结构（添加或删除一个mapping，仅仅改变一个已经存在的key的value，这不算结构修改），必须外在地执行同步化操作。如果没有这种对象存在，map需要被装箱（wrapped）,如：

Map m = Collections.synchronizedMap(new HashMap(...));

1.2 HashMap的数据结构
----------------

在Java中，基本的存储数据的结构有两种，一个是数组，一个事模拟指针（引用）。所有的数据结构都可以用这两个基本结构来构造的，HashMap就是一个“链表的数组”的数据结构。如下图所示：

![](file:///C:/Users/Administrator/AppData/Local/YNote/data/qq1C04AC66BBFA7A186F0EBDB3B35D84F2/c74ac270b618462d9e2fa980e534d573/hashmap.jpg)[![hashmap](http://www.wangmingkuo.com/wp-content/uploads/2015/07/hashmap-300x238.jpg)](http://www.wangmingkuo.com/wp-content/uploads/2015/07/hashmap.jpg)

由上图可以看出，HashMap底层就是一个数组结构，数组的每一项又是一个链表。当新建一个HashMap的时候，就会初始化一个数组，源码如下：

/\*\*
 \* The table, resized as necessary. Length MUST Always be a power of two.
 */
transient Entry<K,V>\[\] table = (Entry<K,V>\[\]) EMPTY_TABLE;

 static class Entry<K,V> implements Map.Entry<K,V> {
     final K key;
     V value;
     Entry<K,V> next;
     int hash;
     ......

从源码中可以看出，Entry就是数组中的元素，每个Map.Entry就是一个key-value对，Entry持有一个指向下一个元素的引用，这就构成了链表。

1.3 HashMap的存储实现
----------------

### 1.3.1 存储

public V put(K key, V value) {
    //如果table为空，初始化；
    if (table == EMPTY_TABLE) {
        inflateTable(threshold);
    }
    //如果key为null，调用putForNullKey方法进行处理，将value放置在数组的第一个位置；
    if (key == null)
        return putForNullKey(value);
    //根据key计算hash值
    int hash = hash(key);
    //搜索指定hash值在对应table中的索引；
    int i = indexFor(hash, table.length);
    //如果i索引的Entry不为null，通过循环不断遍历e元素的下一个元素；
    for (Entry<K,V> e = table\[i\]; e != null; e = e.next) {
        Object k;
        //找到指定key与需要放入的key是否相同（hash值相同且通过equals比较返回true）；
        //旧值将会被新值替换；
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
    //如果i索引处的Entry为null，表明此处还没有Entry；
    //modCount记录HashMap中修改结构的次数；
    modCount++;
    //将key value添加到i索引处；
    addEntry(hash, key, value, i);
    return null;
}

上面方法提供了一个根据 hashCode() 返回值来计算 Hash 码的方法：hash()，这个方法是一个纯粹的数学计算，其方法如下：

final int hash(Object k) {
    int h = hashSeed;
    if (0 != h && k instanceof String) {
        return sun.misc.Hashing.stringHash32((String) k);
    }

    h ^= k.hashCode();

    // This function ensures that hashCodes that differ only by
    // constant multiples at each bit position have a bounded
    // number of collisions (approximately 8 at default load factor).
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}

对于任意给定的对象，只要它的 hashCode() 返回值相同，那么程序调用 hash 方法所计算得到的 Hash 码值总是相同的。接下来程序会调用 indexFor(int h, int length) 方法来计算该对象应该保存在 table 数组的哪个索引处。indexFor(int h, int length) 方法的代码如下：

static int indexFor(int h, int length) 
{ 
    return h & (length-1); 
}

h&(length-1)运算等价于对length取模，即h % length，举例如下所示：

h&(length-1)                h                  length-1

       4 & (16-1)         00100      &     01111      =      00100 (4)

       5 & (16-1)         00101       &     01111      =      00101 (5)

      18 & (16-1)        10010       &     01111      =      00010(2)

      23 & (16-1)       10111         &     01111      =      00111(7)

      -----------------------------------------------------------------------------------------------------------------------

根据上面 put 方法的源代码可以看出，当程序试图将一个 key-value 对放入 HashMap 中时，程序首先根据该 key 的 hashCode() 返回值决定该 Entry 的存储位置：如果两个 Entry 的 key 的 hashCode() 返回值相同，那它们的存储位置相同。如果这两个 Entry 的 key 通过 equals 比较返回 true，新添加 Entry 的 value 将覆盖集合中原有 Entry 的 value，但 key 不会覆盖。如果这两个 Entry 的 key 通过 equals 比较返回 false，新添加的 Entry 将与集合中原有 Entry 形成 Entry 链，而且新添加的 Entry 位于 Entry 链的头部。

当向 HashMap 中添加 key-value 对，由其 key 的 hashCode() 返回值决定该 key-value 对（就是 Entry 对象）的存储位置。当两个 Entry 对象的 key 的 hashCode() 返回值相同时，将由 key 通过 eqauls() 比较值决定是采用覆盖行为（返回 true），还是产生 Entry 链（返回 false）。

上面程序中还调用了 addEntry(hash, key, value, i); 代码，其中 addEntry 是 HashMap 提供的一个包访问权限的方法，该方法仅用于添加一个 key-value 对。下面是该方法的代码：

void addEntry(int hash, K key, V value, int bucketIndex) 
{ 
    // 获取指定 bucketIndex 索引处的 Entry 
    Entry<K,V> e = table\[bucketIndex\]; 	  //1 
    // 将新创建的 Entry 放入 bucketIndex 索引处，并让新的 Entry 指向原来的 Entry 
    table\[bucketIndex\] = new Entry<K,V>(hash, key, value, e); 
    // 如果 Map 中的 key-value 对的数量超过了极限
    if (size++ >= threshold) 
        // 把 table 对象的长度扩充到 2 倍。
        resize(2 * table.length); 	  
}

上面方法的代码很简单，但其中包含了一个非常优雅的设计：系统总是将新添加的 Entry 对象放入 table 数组的 bucketIndex 索引处——如果 bucketIndex 索引处已经有了一个 Entry 对象，那新添加的 Entry 对象指向原有的 Entry 对象（产生一个 Entry 链），如果 bucketIndex 索引处没有 Entry 对象，也就是上面程序1处代码的 e 变量是 null，也就是新放入的 Entry 对象指向 null，也就是没有产生 Entry 链。

### 1.3.2 读取

当 HashMap 的每个 bucket 里存储的 Entry 只是单个 Entry ——也就是没有通过指针产生 Entry 链时，此时的 HashMap 具有最好的性能：当程序通过 key 取出对应 value 时，系统只要先计算出该 key 的 hashCode() 返回值，在根据该 hashCode 返回值找出该 key 在 table 数组中的索引，然后取出该索引处的 Entry，最后返回该 key 对应的 value 即可。看 HashMap 类的 get(K key) 方法代码：

public V get(Object key) 
 { 
    // 如果 key 是 null，调用 getForNullKey 取出对应的 value 
    if (key == null) 
        return getForNullKey(); 
    // 根据该 key 的 hashCode 值计算它的 hash 码
    int hash = hash(key.hashCode()); 
    // 直接取出 table 数组中指定索引处的值，
    for (Entry<K,V> e = table\[indexFor(hash, table.length)\]; 
        e != null; 
        // 搜索该 Entry 链的下一个 Entr 
        e = e.next) 		 // ①
	{ 
	    Object k; 
	    // 如果该 Entry 的 key 与被搜索 key 相同
	    if (e.hash == hash && ((k = e.key) == key 
		|| key.equals(k))) 
		return e.value; 
	} 
	return null; 
 }

从上面代码中可以看出，如果 HashMap 的每个 bucket 里只有一个 Entry 时，HashMap 可以根据索引、快速地取出该 bucket 里的 Entry；在发生“Hash 冲突”的情况下，单个 bucket 里存储的不是一个 Entry，而是一个 Entry 链，系统只能必须按顺序遍历每个 Entry，直到找到想搜索的 Entry 为止——如果恰好要搜索的 Entry 位于该 Entry 链的最末端（该 Entry 是最早放入该 bucket 中），那系统必须循环到最后才能找到该元素。

归纳起来简单地说，HashMap 在底层将 key-value 当成一个整体进行处理，这个整体就是一个 Entry 对象。HashMap 底层采用一个 Entry\[\] 数组来保存所有的 key-value 对，当需要存储一个 Entry 对象时，会根据 Hash 算法来决定其存储位置；当需要取出一个 Entry 时，也会根据 Hash 算法找到其存储位置，直接取出该 Entry。由此可见：HashMap 之所以能快速存、取它所包含的 Entry，完全类似于现实生活中母亲从小教我们的：不同的东西要放在不同的位置，需要时才能快速找到它。

当创建 HashMap 时，有一个默认的负载因子（load factor），其默认值为 0.75，这是时间和空间成本上一种折衷：增大负载因子可以减少 Hash 表（就是那个 Entry 数组）所占用的内存空间，但会增加查询数据的时间开销，而查询是最频繁的的操作（HashMap 的 get() 与 put() 方法都要用到查询）；减小负载因子会提高数据查询的性能，但会增加 Hash 表所占用的内存空间。

掌握了上面知识之后，我们可以在创建 HashMap 时根据实际需要适当地调整 load factor 的值；如果程序比较关心空间开销、内存比较紧张，可以适当地增加负载因子；如果程序比较关心时间开销，内存比较宽裕则可以适当的减少负载因子。通常情况下，程序员无需改变负载因子的值。

1.4 HashSet的实现
--------------

对于 HashSet 而言，它是基于 HashMap 实现的，HashSet 底层采用 HashMap 来保存所有元素，因此 HashSet 的实现比较简单，查看 HashSet 的源代码，可以看到如下代码：

public class HashSet<E> 
	 extends AbstractSet<E> 
	 implements Set<E>, Cloneable, java.io.Serializable 
 { 
	 // 使用 HashMap 的 key 保存 HashSet 中所有元素
	 private transient HashMap<E,Object> map; 
	 // 定义一个虚拟的 Object 对象作为 HashMap 的 value 
	 private static final Object PRESENT = new Object(); 
	 ... 
	 // 初始化 HashSet，底层会初始化一个 HashMap 
	 public HashSet() 
	 { 
		 map = new HashMap<E,Object>(); 
	 } 
	 // 以指定的 initialCapacity、loadFactor 创建 HashSet 
	 // 其实就是以相应的参数创建 HashMap 
	 public HashSet(int initialCapacity, float loadFactor) 
	 { 
		 map = new HashMap<E,Object>(initialCapacity, loadFactor); 
	 } 
	 public HashSet(int initialCapacity) 
	 { 
		 map = new HashMap<E,Object>(initialCapacity); 
	 } 
	 HashSet(int initialCapacity, float loadFactor, boolean dummy) 
	 { 
		 map = new LinkedHashMap<E,Object>(initialCapacity 
			 , loadFactor); 
	 } 
	 // 调用 map 的 keySet 来返回所有的 key 
	 public Iterator<E> iterator() 
	 { 
		 return map.keySet().iterator(); 
	 } 
	 // 调用 HashMap 的 size() 方法返回 Entry 的数量，就得到该 Set 里元素的个数
	 public int size() 
	 { 
		 return map.size(); 
	 } 
	 // 调用 HashMap 的 isEmpty() 判断该 HashSet 是否为空，
	 // 当 HashMap 为空时，对应的 HashSet 也为空
	 public boolean isEmpty() 
	 { 
		 return map.isEmpty(); 
	 } 
	 // 调用 HashMap 的 containsKey 判断是否包含指定 key 
	 //HashSet 的所有元素就是通过 HashMap 的 key 来保存的
	 public boolean contains(Object o) 
	 { 
		 return map.containsKey(o); 
	 } 
	 // 将指定元素放入 HashSet 中，也就是将该元素作为 key 放入 HashMap 
	 public boolean add(E e) 
	 { 
		 return map.put(e, PRESENT) == null; 
	 } 
	 // 调用 HashMap 的 remove 方法删除指定 Entry，也就删除了 HashSet 中对应的元素
	 public boolean remove(Object o) 
	 { 
		 return map.remove(o)==PRESENT; 
	 } 
	 // 调用 Map 的 clear 方法清空所有 Entry，也就清空了 HashSet 中所有元素
	 public void clear() 
	 { 
		 map.clear(); 
	 } 
	 ... 
 }

由上面源程序可以看出，HashSet 的实现其实非常简单，它只是封装了一个 HashMap 对象来存储所有的集合元素，所有放入 HashSet 中的集合元素实际上由 HashMap 的 key 来保存，而 HashMap 的 value 则存储了一个 PRESENT，它是一个静态的 Object 对象。HashSet 的绝大部分方法都是通过调用 HashMap 的方法来实现的，因此 HashSet 和 HashMap 两个集合在实现本质上是相同的。

2. HashSet和HashMap的区别
=====================

\*HashMap\*

\*HashSet\*

HashMap实现了Map接口

HashSet实现了Set接口

HashMap储存键值对

HashSet仅仅存储对象

使用put()方法将元素放入map中

使用add()方法将元素放入set中

HashMap中使用键对象来计算hashcode值

HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false

HashMap比较快，因为是使用唯一的键来获取对象

HashSet较HashMap来说比较慢

3\. HashMap和HashTable的区别
========================

HashMap和Hashtable都实现了Map接口，但决定用哪一个之前先要弄清楚它们之间的分别。主要的区别有：线程安全性，同步(synchronization)，以及速度。

*   HashMap几乎可以等价于Hashtable，除了HashMap是非synchronized的，并可以接受null(HashMap可以接受为null的键值(key)和值(value)，而Hashtable则不行)。
*   HashMap是非synchronized，而Hashtable是synchronized，这意味着Hashtable是线程安全的，多个线程可以共享一个Hashtable；而如果没有正确的同步的话，多个线程是不能共享HashMap的。Java 5提供了ConcurrentHashMap，它是HashTable的替代，比HashTable的扩展性更好。
*   另一个区别是HashMap的迭代器(Iterator)是fail-fast迭代器，而Hashtable的enumerator迭代器不是fail-fast的。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。
*   由于Hashtable是线程安全的也是synchronized，所以在单线程环境下它比HashMap要慢。如果你不需要同步，只需要单一线程，那么使用HashMap性能要好过Hashtable。
*   HashMap不能保证随着时间的推移Map中的元素次序是不变的。

注：

*   sychronized意味着在一次仅有一个线程能够更改Hashtable。就是说任何线程要更新Hashtable时要首先获得同步锁，其它线程要等到同步锁被释放之后才能再次获得同步锁更新Hashtable。
*    Fail-safe和iterator迭代器相关。如果某个集合对象创建了Iterator或者ListIterator，然后其它的线程试图“结构上”更改集合对象，将会抛出ConcurrentModificationException异常。但其它线程可以通过set()方法更改集合对象是允许的，因为这并没有从“结构上”更改集合。但是假如已经从结构上进行了更改，再调用set()方法，将会抛出IllegalArgumentException异常。
*    结构上的更改指的是删除或者插入一个元素，这样会影响到map的结构。

参考文献：
=====

1.  [通过分析 JDK 源代码研究 Hash 存储机制](http://www.ibm.com/developerworks/cn/java/j-lo-hash/?ca=drs-tp4608)
2.  [深入Java集合学习系列：HashMap的实现原理](http://www.cnblogs.com/xwdreamer/archive/2012/06/03/2532832.html)
3.  [深入Java集合学习系列：HashMap的实现原理](http://zhangshixi.iteye.com/blog/672697)
4.  [Java 7之集合类型第4篇 - HashMap](http://blog.csdn.net/mazhimazh/article/details/17876641)
5.  [HashMap源码分析（基于JDK1.6）](http://www.cnblogs.com/hzmark/archive/2012/12/24/hashmap.html)
6.  [Java 集合系列10之 HashMap详细介绍(源码解析)和使用示例](http://www.cnblogs.com/skywang12345/p/3310835.html)
7.  [Java HashMap的工作原理](http://www.importnew.com/10620.html)
8.  [HashMap的工作原理](http://www.importnew.com/7099.html)（针对面试的一些问题给出解答）
9.  [HashMap和HashSet的区别](http://www.importnew.com/6931.html)
10.  [HashMap和Hashtable的区别](http://www.importnew.com/7010.html)
