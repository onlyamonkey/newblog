---
title: MD文档语法学习
tags: 
	- markdown 
---

# MD文档语法学习

#### 前言：

​	作为一个程序员，很多地方都可能遇到markdown文件，特别是博客系统，大部分博客都可以用markdown文件来生成，其原理就是通过md语法的各种标签关键字转换为html展示出来，下面就是学习MD语法的过程。
<!-- more -->
##### 1.标题的用法

标题大概可以分三种

1. 用“#”来标记

   1. 单边式（在文字左侧）

     >![p](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/1.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/1-1.png)	

     由此看来几个“#”代表几级标题，最多6级.

   2. 双边式（在文字两侧）

     >![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/2.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/2-2.png)

     无论单双“#”与文字都应该有空格

2. 用“=”和“-”来标记

   > ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/3.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/3-3.png)

不过“=”和“-”只能来标记一二级标题

##### 2.列表的用法

1. “+”，“-”，“+” 无序

   > ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/4.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/4-4.png)

   三种方式没有区别任选一种即可

2. 数值+“.”有序

   > ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/5.png)

只需要数字加英文的点然后空格跟内容就好了，值得注意的是，看下图



> ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/6.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/7.png)

可以看出有的编辑器会把所有的序列从1开始，有的就是从第一个数字一次累加，本人用的Typora是后者

##### 3.区块引用">"

常用于一解释某些内容，或者引用某人的话。

> ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/8.png)

只需要在文字前面加入“>”，就可以，另外引用也可以嵌套使用。

> ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/9.png)

多层嵌套就是把多个“>”放一起就好了，另外引用可以与其他的一起联合使用可以达到意想不到的效果哦！



