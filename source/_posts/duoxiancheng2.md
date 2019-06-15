---
title: 初识多线程和原理分析
tags: 
	- 多线程 
	- java
---



## 初识多线程和原理分析 ##

#### 在什么情况下使用多线程

​      使用多线程的情况主要是解决程序的堵塞问题,调高CPU的使用率提升程序的性能,说到底多线程是解决的是等待的问题。

#### 多线程的创建方式

​      主要有三种:继承Thread类、实现Runnable接口、使用ExecutorService、Callable、Fulture实现带返回值的多线程。

##### 继承Thread类创建线程

​        Thread是实现Runnable的一个实例,代表一个线程的实例。start()是启动线程的唯一方法。start()是一个native方法,会启动新的线程,执行run()方法。
<!-- more -->
```java
package text;
public class MyThread extends Thread {	
	@Override
	public void run() {
		System.out.println("继承Thread类创建线程");
	}

	public static void main(String[] args) {
		MyThread myThread=new MyThread();
		myThread.start();
	}
}
```

##### 实现Runnable接口创建线程

​       Runnable结果类的单继承的问题。

```java
package text;
public class MyRunnable implements Runnable {
	@Override
	public void run() {
		System.out.println("this is a runnable ");		
	}
	
	public static void main(String[] args) {
		Thread thread=new Thread(new MyThread());
		thread.start();
	}

}
```

##### 实现Callable接口通过FutureTask包装器实现线程的创建

```java
package text;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
public class MyCallable implements Callable<String> {

	@Override
	public String call() throws Exception {
		return "this is a callable";
	}
	
	public static void main(String[] args) throws InterruptedException, ExecutionException {
		ExecutorService executorService=Executors.newCachedThreadPool();
        MyCallable myCallable=new MyCallable();
        Future<String> future=executorService.submit(myCallable);
        //执行future.get()时会堵塞
        System.out.println(future.get());
        executorService.shutdown();
	}
}
```

#### 多线程案例分享

​     通过堵塞队列和多线程实现对请求的异步处理提升处理的性能。

​      **Request** 

```java
package text;
public class Request {
	
	private String name;
	
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

​      **RequestProcessor**

```java
package text;

public interface RequestProcessor {
	void processRequest(Request request);
}
```

​      **PrintProcessor** 

```java
package text;
import java.util.concurrent.LinkedBlockingQueue;

public class PrintProcessor extends Thread implements RequestProcessor {

	LinkedBlockingQueue<Request> requests=new LinkedBlockingQueue<Request>();
	
	private final RequestProcessor nextProcessor;
	
	public PrintProcessor(RequestProcessor nextProcessor){
		this.nextProcessor=nextProcessor;
	}
	
	@Override
	public void run() {
		while (true) {
			try {
				Request request=requests.take();
				System.out.println("print data:"+request.getName());
				nextProcessor.processRequest(request);
			} catch (Exception e) {
				e.printStackTrace();
			}		
		}
	}

	//处理请求
	@Override
	public void processRequest(Request request) {
		requests.add(request);		
	}

}
```

​    **SaveProcessor**

```java
package text;

import java.util.concurrent.LinkedBlockingQueue;

public class SaveProcessor extends Thread implements RequestProcessor {

    LinkedBlockingQueue<Request> requests=new LinkedBlockingQueue<Request>();
	
	private final RequestProcessor nextProcessor;
	
	public SaveProcessor(RequestProcessor nextProcessor){
		this.nextProcessor=nextProcessor;
	}
	
	@Override
	public void run() {
		while (true) {
			try {
				Request request=requests.take();
				System.out.println("save data:"+request.getName());
				nextProcessor.processRequest(request);
			} catch (Exception e) {
				e.printStackTrace();
			}		
		}
	}

	//处理请求
	@Override
	public void processRequest(Request request) {
		requests.add(request);		
	}

}

```

​    **ThreadTest**

```jav
package text;

public class ThreadTest {

	PrintProcessor printProcessor;

	public ThreadTest() {
		SaveProcessor saveProcessor = new SaveProcessor();
		saveProcessor.start();
		printProcessor = new PrintProcessor(saveProcessor);
		printProcessor.start();
	}
	
	private void doTest(Request request){
	    printProcessor.processRequest(request);
	}
	
	public static void main(String[] args) {
		Request request=new Request();
		request.setName("Dog");
		new ThreadTest().doTest(request);
	}

}
```

#### 并发编程的基础

​     线程是操作系统调度的最小单元,并且能够让多线程同时执行,提高程序的性能,在多核处理器更加有优势。

#####  线程的状态


​    线程的生命周期总共有六种:(New、Runnabe、Blocked、Waiting、Time_Waiting、Terminated)

​    Blocked状态有3中情况:

- 等待堵塞:执行Wait方法,JVM把线程放入等待队列。
- 同步堵塞:同步锁
- 其它堵塞:sleep、join方法等
  ![001](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/images/001.png)
  ​    旧的方法stop、suspend、resume等不提倡使用,因为结束一个线程时并不会保证线程的资源正常的释放,可能导致程序出现一些不确定的状态。

​     使用interrupt方法只是改变中断状态而已,不会中断正在运行的线程,其实是给受堵塞线程发出一个中断信号,使受阻线程得以退出堵塞的线程。线程通过检查资深是否被中断来进行相应，可以通过 isInterrupted()来判断是否被中断。

​    实例演示线程终止的逻辑,这种通过标识位或者中断操作的方式能够使线程在终止时有机会去清理资源,而不是武断地将线程停止

``` JAVA
public class InterruptDemo {
     private static int i;
     public static void main(String[] args) throws InterruptedException {
        Thread thread=new Thread(()->{
        while(!Thread.currentThread().isInterrupted()){
          i++;
         }
         System.out.println("Num:"+i);
       },"interruptDemo");
     thread.start();
     TimeUnit.SECONDS.sleep(1);
     thread.interrupt();
   }
}
```

​    定义一个volidate修饰的成员变量,来可以控制线程的终止。volidate可以实现共享变量之间的可见性。

```java
package text;

public class ValidateTest extends Thread {
    
	public volatile static boolean stop=false;
	
	@Override
	public void run() {
		int j=0;
		while(!stop){
			j++;
		}
	}

	public static void main(String[] args) {
		ValidateTest demo=new ValidateTest();
        demo.start();
        stop=true;
	}

}
```

#####  线程的复位

- 通过interrupt设置了一个标识告诉线程可以终止了,线程中还提供静态方法 Thread.interrupted()对设置的中断标识的线程复位。例子:外面线程通过 thread.interrupt设置中断标识,在线程里面通过Thread.interrupted进行复位。
- 通过抛出InterruptedException异常时复位标识已经设置成false了。

####  线程的安全问题 

​     多线程在编程时会出现线程安全问题,安全问题可以归纳成三点主要是:原子性、可见性、有序性。

#####  CPU的高速缓存

​    线程是 CPU 调度的最小单元，线程涉及的目的最终仍然是更充分的利用计算机处理的效能,但是绝大部分的运算任务不能只依靠处理器“计算”就能完成,处理器还需要与内存交互,比如读取运算数据、存储运算结果,这个 I/O 操作是很难消除的。而由于计算机的存储设备与处理器的运算速度差距非常大,所以现代计算机系统都会增加一层读写速度尽可能接近处理器运算速度的高速缓存来作为内存和处理器之间的缓冲：将运算需要使用的数据复制到缓存中,让运算能快速进行，当运算结束后再从缓存同步到内存之中。
![002](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/images/002.png)
​        高速缓存从下到上越接近CPU速度越快,同时容量越小。现在大部分的处理器都有二级和三级缓存,从下到上依次为 L3 cache, L2 cache, L1 cache. 缓存又可以分为指令缓存和数据缓存,指令缓存用来缓存程序的代码,数据缓存用来缓存程序的数据。

​         L1 Cache,一级缓存,本地 core 的缓存,分成 32K 的数据缓存 L1d 和 32k 指令缓存 L1i,访问 L1 需要 3cycles,耗时大约 1ns。

​        L2 Cache,二级缓存,本地 core 的缓存,被设计为 L1 缓存与共享的 L3 缓存之间的缓冲,大小为 256K，访问 L2 需要 12cycles，耗时大约 3ns;

​        L3 Cache,三级缓存，在同插槽的所有 core 共享 L3 缓存，分为多个 2M 的段,访问 L3 需要 38cycles，耗时大约 12ns；

##### 缓存一致性问题

​       CPU-0 读取主存的数据，缓存到 CPU-0 的高速缓存中，CPU-1 也做了同样的事情，而 CPU-1 把 count 的值修改成了 2，并且同步到 CPU-1 的高速缓存，但是这个修改以后的值并没有写入到主存中，CPU-0 访问该字节，由于缓存没有更新，所以仍然是之前的值，就会导致数据不一致的问题。引发这个问题的原因是因为多核心 CPU 情况下存在指令并行执行，而各个CPU 核心之间的数据不共享从而导致缓存一致性问题，为了解决这个问题,CPU 生产厂商提供了相应的解决方案。

##### 总线锁

​        当一个 CPU 对其缓存中的数据进行操作的时候，往总线中发送一个 Lock 信号。其他处理器的请求将会被阻塞，那么该处理器可以独占共享内存。总线锁相当于把 CPU 和内存之间的通信锁住了,所以这种方式会导致 CPU 的性能下降，所以 P6 系列以后的处理器，出现了另外一种方式，就是缓存锁。

#####  缓存锁

​       如果缓存在处理器缓存行中的内存区域在 LOCK 操作期间被锁定，当它执行锁操作回写内存时，处理不在总线上声明 LOCK 信号，而是修改内部的缓存地址，然后通过缓存一致性机制来保证操作的原子性，因为缓存一致性机制会阻止同时修改被两个以上处理器缓存的内存区域的数据，当其他处理器回写已经被锁定的缓存行的数据时会导致该缓存行无效。

​      所以如果声明了 CPU 的锁机制，会生成一个 LOCK 指令，会产生两个作用:

- Lock 前缀指令会引起引起处理器缓存回写到内存，在 P6 以后的处理器中，
  LOCK 信号一般不锁总线，而是锁缓存
- 一个处理器的缓存回写到内存会导致其他处理器的缓存无效

##### 缓存一致性协议 

​    处理器上有一套完整的协议,来保证Cache的一致性,经典是是MESI协议,它的方法时在CPUh缓存中保留一个标记位,这个标记位有四种状态:

   M(Modified):修改缓存,当前CPU缓存已经被修改,表示已经和内存中的数据不一致了。

   I(Invalid):失效缓存,说明CPU缓存不能使用了。

   E(Exclusive):处理器没有缓存该数据独占缓存，当前 cpu 的缓存和内存中数据保持一直,而且其他处理器没有缓存该数据。

   S(Shared):缓存中共享缓存,数据和内存中数据一致，并且该数据存在多个 cpu缓存中。

**每个Core的Cache控制器不仅知道自己的读写操作,也监听其它Cache的读写操作,嗅探（snooping）"协议。**

CPU的读取会遵循几个原则:

- 1.如果缓存的状态是I,那么就从内存中读取,否则直接从缓存中读取。
- 如果缓存处于M或者E的CPU嗅探到其他CPU有读的操作,就把自己的缓存写入到内存,并把自己的状态设置成S。
- 只有缓存状态是M或者E的时候,CPU才可以修改缓存中的数据,修改后,缓存 状态变为MC。


##### 内存模型

​          内存模型定义了共享内存系统中多线程程序读写操作行为的规范，来屏蔽各种硬件和操作系统的内存访问差异，来实现 Java 程序在各个平台下都能达到一致的内存访问效果。Java 内存模型的主要目标是定义程序中各个变量的访问规则，也就是在虚拟机中将变量存储到内存以及从内存中取出变量（这里的变量,指的是共享变量,也就是实例对象、静态字段、数组对象等存储在堆内存中的变量。而对于局部变量这类的，属于线程私有，不会被共享）这类的底层细节。通过这些规则来规范对内存的读写操作，从而保证指令执行的正确性。它与处理器有关、与缓存有关、与并发有关、与编译器也有关。他解决了 CPU多级缓存、处理器优化、指令重排等导致的内存访问问题，保证了并发场景下的可见性、原子性和有序性，。内存模型解决并发问题主要采用两种方式：限制处理器优化和使用内存屏障。

​          Java 内存模型定义了线程和内存的交互方式，在 JMM 抽象模型中,分为主内存、工作内存。主内存是所有线程共享的，工作内存是每个线程独有的。线程对变量的所有操作（读取、赋值）都必须在工作内存中进行，不能直接读写主内存中的变量。并且不同的线程之间无法访问对方工作内存中的变量,线程间的变量值的传递都需要通过主内存来完成,他们三者的交互关系如下

![003](https://raw.githubusercontent.com/onlyamonkey/newblog/master/source/_posts/images/003.png)



​          所以,总的来说，JMM 是一种规范，目的是解决由于多线程通过共享内存进行通信时，存在的本地内存数据不一致、编译器会对代码指令重排序、处理器会对代码乱序执行等带来的问题。目的是保证并发编程场景中的原子性、可见性和有序性。






