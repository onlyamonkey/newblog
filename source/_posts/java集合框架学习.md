---
title: java集合框架学习
tags: 
	- 集合框架 
	- java
---

## java集合框架学习 ##
java中结合框架主要分两种一种是继承实现Collection接口，一种是集成实现Map接口
<!-- more -->
<!-- reward:true -->
- 继承实现Conllection
>![](http://pth9simu1.bkt.clouddn.com/md/collection.png)
- 继承实现Map
>![](http://pth9simu1.bkt.clouddn.com/md/map.png) 

***
由图可知Collection主要由List、Queue、Set继承，其中主要的实现类主要有ArrayList、LinkedList、Vector、Stack、HashSet、TreeSet,Map主要实现类则是HashMap
---
1.__ArrayList__
	底层由一维数组组成，不是线程安全的，允许null，其中的元素是有序的，可以重复的，它的容量是可以动态增长的，初始容量为10，当容量满的时候，新的容量=原始容量\*3/2。当容量不够的时候，每次增加元素，都要将原来的元素拷贝到一个新的数组当中。其有三种遍历方式，第一种就是普通for循环，第二种就是增强for循环，第三种就是使用Iterator。随机读取采用的是get（index）的方法，插入时要先判容，删除时要将数组移位。实现了Serializable接口，支持序列化。
2.__LinkedList__ 
  底层是由双向链表组成（模拟栈和队列的模式），不是线程安全的，允许null，其中的元素是有序的，可以重复的，它的容量也是动态增长的，但是没有扩容方法，加入元素是尾部自动扩容。其有三种遍历方式，第一种就是普通for循环，第二种就是增强for循环，第三种就是使用Iterator。实现了Serializable接口，支持序列化。因为是基于链表实现的，所以其的插入删除效率高，查找效率低，查找会遍历整个链表。
3.__Vector__  
	底层是由一维数组组成的（是一个矢量队列），是线程安全的，允许null，其中的元素是有序的，可以重复的，它的容量可以根据需要增大或缩小。其有三种遍历方式，第一种就是普通for循环，第二种就是增强for循环，第三种就是使用Iterator。实现了Serializable接口，支持序列化。Vector是从Java1.0版本开始的，从1.2开始实现了List接口，与ArrayList大同小异，主要差别就是线程安全上的差别。
4.__Stack__ 
	底层是由一维数组组成的（模拟栈的模式），继承了Vector，因此是线程安全的，拥有着Vector的属性和功能，其中的元素是有序的，可以重复的。特性是FILO（先进后出）
5.__HashSet__ 
	是Set子接口的实现类，底层数据结构是Hash表（Hash数组），不是线程安全的，允许null，是无序的，无序指的是存入顺序与取出顺序不一致，不是指随机，不允许重复，因为不允许重复，因此只能有一个null值。添加元素的时候，会先调用元素的hashCode方法得到元素的哈希值，然后通过哈希值经过移位的算法，来算出元素的存储位置，这时候有两种情况.
    1：如果该位置没有元素，则直接将其存上；2：如果该位置有元素，则调用equals方法进行比较，如果返回true，则不允许添加，如果返回false，则可以添加，而这个可以添加，待会讲了HashMap就能知道是怎么添加的了。HashSet底层实际上有一个由HashMap创建的map变量，HashSet的操作函数，实际上是通过HashMap来实现的，将元素存放到HashSet中，实际上是将元素作为key值存放到HashMap中。如果添加一个已经存在的值，底层会直接不允许添加，因此新值是不会覆盖旧值的。其的遍历方式有两种，第一种是增强for循环（foreach），第二种是使用迭代器（Iterator）。实现了Serializable接口，支持序列化。应用的场景一般就是单个元素的去重。
6.__HashMap__ 
  是Map的实现类，底层是Hash表，不是线程安全的，允许null，是无序的，指的是存入顺序与取出顺序不一致，不是随机的。存放的数据以键值对的方式，即key，value，一个key值对应一个value值，key值不能重复，但是value值可以重复。其的遍历方式有两种，一种（keySet()）是将映射里面的key值取出来，存放在set集合里面，然后通过遍历set集合取出key值，再在遍历set里面通过取出的key值来获得value值。第二种（entrySet()）是把集合中所有的映射关系（entry）抽取出来，存放在set集合里面，然后通过遍历Entry，来取得Entry里面的key和value值来达到遍历的作
  
__可以得出各集合特性对比表__ 

|                | 底层实现 | 是否线程安全 | 是否允许null | 是否有序 | 是否可重复 |           扩容            |         遍历          | 增删改效率 | 查询效率 |
| :------------: | :------: | :----------: | :----------: | :------: | :--------: | :-----------------------: | :-------------------: | :--------: | :------: |
| ArrayList  |   数组   |      ×       |      √       |    √     |     √      |           1.5倍           | for 加强for  Iterator |     慢     |    快    |
| LinkedList | 双向链表 |      ×       |      √       |    √     |     √      |         动态增长          | for 加强for  Iterator |     快     |    慢    |
|   Vector   |   数组   |      √       |      √       |    √     |     √      | 默认2倍，可以用容量因子指定 | for 加强for  Iterator |     慢     |    慢    |
|     Stack      |   数组   |      √       |      √       |    √     |     √      | 默认2倍，可以用容量因子指定 | for 加强for  Iterator |     慢     |    慢    |
|    HashSet     | Hash数组 |      ×       |      √       |    ×     |     ×      |            2倍            |     for interator     |     -      |    -     |
|    HashMap     | Hash数组 |      ×       |      √       |    ×     |     ×      |            2倍            | keySet()  entrySet () |     -      |    -     |

--------------------- 
本文内容参考资料
原文：[Java集合框架的学习](ttps://blog.csdn.net/qq_41061437/article/details/81566249 )





