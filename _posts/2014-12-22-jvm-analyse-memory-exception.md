---
layout: post
title: JVM深度剖析——内存溢出分析方法
description: "JVM深度剖析——内存溢出分析方法"
modified: 2014-12-22
category: articles
tags: [java,jvm]
comments: true
share: true
---

### Java深度剖析——内存溢出分析方法

####工具

安装Memory Analyse Tools(MAT) 工具， 可以直接在eclipse中安装其相应的插件，安装方法可以参考另一篇[eclipse插件汇总](http://pengjunjie.com/articles/java-eclipse-pluign-summary/)

不会用的可以参考一下这个帖子[使用 Eclipse Memory Analyzer 进行堆转储文件分析](http://www.ibm.com/developerworks/cn/opensource/os-cn-ecl-ma/index.html)

####一些Java内存参数设置

`-vmargs`:  说明后面是VM的参数，所以后面的其实都是JVM的参数了

`-Xms20m`:  Java初始分配的堆内存，此处设置为20M
`-Xmx20m`:  Java最大允许分配的堆内存，此处设置为20M，同时这样设置表示堆内存不许扩展

`-XX:PermSize=64M`:	 JVM初始分配的非堆内存
`-XX:MaxPermSize=128M`: JVM最大允许分配的非堆内存，按需分配

`-XX:+HeapDumpOnOutOfMemoryError`: 让虚拟机在出现内存溢出异常时dump出当前内存堆转储快照以便事后进行分析。
带上这种参数之后运行Jvm，如果出现相应的内存溢出异常，会在目录下形成一个异常时候的内存dump文件(如java_pid7126.hprof文件)，将这个文件使用Memory Analyse Tool工具打开就可以看到当前dump内存空间的分析内容。

`-XX:+HeapDumpOnCtrlBreak`: 

`-Xss`: 设置虚拟机栈内存容量


####获得转储文件的一些方法：
1. 使用JVM启动时的参数设置，如需要在内存溢出时才获取Dump可以使用`-XX:+HeapDumpOnOutOfMemoryError` 或者是希望在某个特定时间获取可以使用 `-XX:+HeapDumpOnCtrlBreak`。
2. 使用一些Java工具获取 ，如 JMap，JConsole 都可以帮助我们得到一个堆转储文件。

####MAT工具的一些使用心得：

#####文件目录

使用MAT工具打开获取的java_pid7126.hprof文件后，会自动的形成如下的文件目录：

	java_pid7126.a2s.index    
	java_pid7126.domIn.index
	java_pid7126.domOut.index
	java_pid7126.hprof              //转储堆文件
	java_pid7126.idx.index
	java_pid7126.inbound.index
	java_pid7126.index
	java_pid7126.o2c.index
	java_pid7126.o2hprof.index
	java_pid7126.o2ret.index
	java_pid7126.outbound.index
	java_pid7126.threads
	java_pid7126_Leak_Suspects.zip	 //打包的报告，解压后会形成一个有HTML方式的报告发送给其他人，方便读取

#####分析方法

######分析三步曲：
1. 对问题发生时刻的系统内存状态获取一个整体印象。
2. 找到最有可能导致内存泄露的元凶，通常也就是消耗内存最多的对象
3. 进一步去查看这个内存消耗大户的具体情况，看看是否有什么异常的行为。

#####具体的分析：

######查看报告一：内存消耗的整体状况
在OverView上的Report中可以选择 Leak Suspects ，来查看整体对象消耗。下方有一个警告可以看到当前系统自动帮忙分析的怀疑对象。在这个怀疑对象中便可以发现大多数的问题。

######查看报告二：分析问题的所在
分析对象为什么没有被回收从而导致一直占用内存。采用根搜索算法来分析对象的事情情况。
点击报告一种怀疑对象的Detail，可以看到相应的具体分析报告。

`Shortest Paths To the Accumulation Point` 分析GC根元素到内存消耗聚集点的最短路径。
`Accumulated Objects` 查看具体的内存对象信息。


####几种经常出现的内存溢出方式

#####Java堆溢出

异常信息： "java.lang.OutOfMemoryError"。会跟着进一步提示Java heap space.

	java.lang.OutOfMemoryError: Java heap space
	Dumping heap to java_pid1293.hprof ...
	Heap dump file created [27559990 bytes in 0.233 secs]
	Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at java.util.Arrays.copyOf(Arrays.java:2245)
	at java.util.Arrays.copyOf(Arrays.java:2219)
	at java.util.ArrayList.grow(ArrayList.java:242)
	at java.util.ArrayList.ensureExplicitCapacity(ArrayList.java:216)
	at java.util.ArrayList.ensureCapacityInternal(ArrayList.java:208)
	at java.util.ArrayList.add(ArrayList.java:440)
	at com.valentine.jvm.analyzer.exception.HeapOOM.main(HeapOOM.java:16)

Java堆溢出内存问题分析步骤总结：
1. dump出来堆转储快照
2. 使用MAT工具对dump出来的堆转储快照进行分析，重点是确认内存中得对象是否必要的，这样可以分清楚到底是出现了内存泄露(Memory Leak)还是内存溢出(Memory Overflow)
3. 如果是内存泄露，进一步通过MAT工具分析泄露对象到GC Roots的引用链。找到泄露对象是通过怎样的路径与GC Roots相关联并导致垃圾收集器无法自动回收的。
4. 如果不存在泄露，那么就是内存中得对象却是都还必须活着，就应当检查虚拟机的堆参数(-Xmx与-Xms)，与机器物理内存对比看是否可以调大，从代码上检查是否存在某些对象生命周期过长、持有状态时间过长的情况，尝试减少程序运行期间的内存。

#####虚拟机和本地方法栈溢出
在HotSpot虚拟机中并不区分虚拟机栈和本地方法栈。到对于HostSpot来说-Xoss参数(设置本地方法栈大小)是存在的，但实际是无效的，栈容量只由-Xss参数决定。

异常情况：
* 如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常
* 如果虚拟机在扩展栈时无法申请到足够的内存空间时，将抛出OutOfMemoryError异常

异常信息：

	stack length:1891Exception in thread "main" 
	java.lang.StackOverflowError
		at com.valentine.jvm.analyzer.exception.JavaVmStackSOF.stackLeak(JavaVmStackSOF.java:8)
		at com.valentine.jvm.analyzer.exception.JavaVmStackSOF.stackLeak(JavaVmStackSOF.java:9)
		at com.valentine.jvm.analyzer.exception.JavaVmStackSOF.stackLeak(JavaVmStackSOF.java:9)
		……
		at com.valentine.jvm.analyzer.exception.JavaVmStackSOF.stackLeak(JavaVmStackSOF.java:9)		

在单线程下，无论是由于栈帧太小还是虚拟机栈容量太小，当内存无法分配的时候，虚拟机抛出的都是StackOverflowError异常。
在多线程下，通过不断的建立线程的方式可以产生内存溢出OutOfMemoryError异常。

在多线程情况下，给每个线程分配的内存越大，越容易产生内存溢出异常。由于操作系统分配给每个进程的内存是有限的，32位的Windows限制为2GB。虚拟机提供了参数来控制Java堆和方法区的这两部分内存的最大值。剩余的2GB减去Xmx，再减去MaxPermSize，忽略掉很小的程序计数器内存。如果虚拟机进程本身耗费的内存不计算，剩下的内存就是有虚拟机栈和本地方法栈瓜分了。此时如果每个线程分配到的虚拟机栈容量越大，可以建立的线程数量自然就越少，建立线程时就越容易把剩下的内存耗尽。
**如果建立过多线程导致的内存溢出，在不能减少线程数或者更换64位虚拟机的情况下，就只能通过减少最大堆或者减少栈容量来获取更多的线程。**

出现StackOverflowError的时候有错误堆栈可以读，即时加入+HeapDumpOnOutOfMemoryError也不会dump异常堆内存。


#####运行时常量池溢出
如果要项运行时常量池中添加内容，最简单的方法就是使用String.intern()这个Native方法。

异常信息：

	java.lang.OutOfMemoryError: PermGen space
	Dumping heap to java_pid1582.hprof ...
	Heap dump file created [625411 bytes in 0.018 secs]
	Exception in thread "Reference Handler" Error occurred during initialization of VM
	java.lang.OutOfMemoryError: PermGen space
		<<no stack trace available>>	
	
	Exception: java.lang.OutOfMemoryError thrown from the UncaughtExceptionHandler in thread "Reference Handler"

从异常中可以看出OutOfMemoryError后报的是PermGen space，说明是方法区溢出。

#####方法区溢出
方法区用于存放Class的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。当大量的类产生时填满方法区，会造成方法去溢出。

方法区溢出也是一种常见内存溢出异常，一个类如果要被垃圾收集器回收，判断条件非常苛刻。

#####本机直接内存溢出

DirectMemory容量可通过-XX:MaxDirectMemorySize指定，如果不指定，则默认与Java堆的最大值(-Xmx)一样。


