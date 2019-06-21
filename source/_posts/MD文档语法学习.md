# MD文档语法学习

#### 前言：

​	作为一个程序员，很多地方都可能遇到markdown文件，特别是博客系统，大部分博客都可以用markdown文件来生成，其原理就是通过md语法的各种标签关键字转换为html展示出来，下面就是学习MD语法的过程。
<!-- more -->
#####1.标题的用法

标题大概可以分三种

1. 用“#”来标记

   + 单边式（在文字左侧）

     >![p](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/1.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/1-1.png)	

     由此看来几个“#”代表几级标题，最多6级.

 + 双边式（在文字两侧）

     >![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/2.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/2-2.png)

     无论单双“#”与文字都应该有空格

2. 用“=”和“-”来标记

   > ![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/3.png)![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/3-3.png)

不过“=”和“-”只能来标记一二级标题

##### 2.列表的用法

1. “+”，“-”，“*” 无序

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

##### 4.分割线的使用

简单来说连续的大于等于3个的“*”，“-”，“_”都可以用作分割线
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/10.png)

由上图看出符号无需全部连续，中间可以有空格，三种情况选一种就可以，个人推荐“-”，因为不同编辑器可能对“*”和“—”效果不一样例如
在Typora
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/11.png)

可以看出三种写法都一样的效果
但是在hexomd 中,只对“-”有效，所以推荐使用“-”

>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/12.png)

##### 5.链接的使用
 *  行内式
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/13.png)

很容易看出行内式语法就是“[”+链接的内容+“]”+"("+链接地址+“）”。

除此之外链接还可以带title就是在地址后面加上空格 然后引号引起来内容即可，如下图
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/14.png)

* 参数式
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/15.png)

类似于js语法前面定义变量，后面使用变量。

##### 6.图片的引用
与连接写法类似分为行内式和参数式
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/16.png)

##### 7.代码框的使用
代码框个人认为还是MD语法中比较重要的，经常用来展示代码片段。代码框分为单行和多行。
* 单行
 >![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/17.png)

单行的代码框是用单个的“`”（英文下的波浪线在键盘的左上角ESC的下方）将代码块包裹起来。
* 多行
>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/18.png)
 
 多行的代码框是用连续三个“```” 将代码块首尾包裹起来，还可以加上注释。
 ##### 8.表格
 md样式的表格一般用的不多吧，大多数都是用外部软件做好表格截个图引用进来，不过学习麻，我们还是要看一下

>![](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/md-images/19.png)

表格写法大同小异，也没有必要全部对齐，但是强迫症患者还是对的吧哈哈
 
 
