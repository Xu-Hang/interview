# Map集合



## hashmap



### 1.源码解析

```java
DEFAULT_INITIAL_CAPACITY = 16 //初始容量 16
DEFAULT_LOAD_FACTOR = 0.75 //默认负载0.75 原因 泊松分布 hash碰撞最小
  
  //hashmap的节点 数据结构 java-8
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;//拉链法
}

int threshold //扩容阀值
  
//什么时候数组扩容?
当前存储数据超过超过负载因子,默认负载因子 0.75(构造时候可以指定负载因子)
//如何扩容 
扩容时候调用resize方法来进行扩容?(如果超过最大容量,则调阈值为int的最大值,不存在以下步骤,以下为正常的扩容步骤)
1. 扩容：创建一个新的Entry空数组，长度是原数组的2倍。
2.ReHash：遍历原Entry数组，把所有的Entry重新Hash到新数组。
  
//为何需要rehash rehash算法
Hash的公式---> index = hash（Key） & （Length - 1）//采用这种方式和取模的效果一致,但是需要保证为2的n次方,所以每次扩容为两倍
  put的hash算法-->hash =  (h = key.hashCode()) ^ (h >>> 16) //hash算法保证散列
//扩容机制:之前用头插法，java8之后改成尾插了呢？ 1.7产生死锁
1.7的头插法,扩容时候,链表的方向会反向,在多线程的时候回导致A->b->a,采用尾插法的时候,再扩容的时候链表的顺序也能保证一致,所以会出现上述的情况
//为何初始容量是16
index计算方法  15->1111 只要hashcode平均分配,hash算法本身就均匀
//什么情况下转红黑树?
当链表长度大于8,并且数组长度大于64的时候
 
```

#### 2. 概念

> 底层数组 链表 红黑树  可以存储null值的key和value 线程不安全

 ## hashtable

#### 概念

> 底层数组加链表 key和value都不能为null,线程安全
>
> 初始容量为11,扩容就是2n+1

直接对put操作上锁,性能太差

## ConcurrentHashMap

#### 概念

> ConcurrentHashMap 最大的特点便是在于线程安全性,由于这一特性,在并发操作map时候,第一个想到便是这个类

下面介绍如何是如何实现的线程安全的?对比hashtable它的优势又是什么?

```java
if (key == null || value == null) throw new NullPointerException();//不允许为null

//分段锁
1.key-value的检验，不允许为空，否则抛出异常；
2.用while(true)无限重试put动作，直到put成功；
3.如果数组没有初始化过则先进行初始化工作；
4.如果数组中该hashcode对应位置不存在元素，则将该元素以CAS方式放入，对应的casTabAt()方法；
5.如果数组中存在元素，用synchronized块保证同步操作，循环判断链表中是否存在该value，存在则返回oldValue，不存在则插入链表末端，put成功跳出循环；
如果是红黑树，则将元素放入红黑树节点中；
6.put完成后根据链表长度是否大于8且数组长度是否大于64来判断是否将链表转换成红黑树；
7.返回oldValue值；
  
//1.7和1.8的变化
锁方面：由分段锁（Segment继承自ReentrantLock）升级为CAS+synchronized实现；
数据结构层面：将Segment变为了Node，每个Node独立，原来默认的并发度16，变成了每个Node都独立，提高了并发度；
hash冲突：1.7中发生hash冲突采用链表存储，1.8中先使用链表存储，后面满足条件后会转换为红黑树来优化查询；
查询复杂度：1.7中链表查询复杂度为O(N)，1.8中红黑树优化为O(logN))
```



