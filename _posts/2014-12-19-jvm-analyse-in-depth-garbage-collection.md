---
layout: post
title: JVM深度剖析——垃圾收集器与内存分配策略
description: "JVM深度剖析——垃圾收集器与内存分配策略"
modified: 2014-12-19
category: articles
tags: [java,jvm]
comments: true
share: true
---

###Jvm深度剖析

####垃圾收集器与内存分配策略


#####几种基本垃圾收集算法：

######引用计数法(Reference Counting)
1. 给对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加1；当引用失效时，计数器值就减1；任何时刻引用计数器为0就代表对象不可能再被使用的。
2. 引用计数器未在Java中使用的主要原因是它很难解决对象之间的相互循环引用的问题。

######根搜索算法(GC Rooting Tracing)
1. 通过一系列名为"GC Routes"的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径成为引用链(Reference Chain)，当一个对象到GC Roots 没有任何引用链相连时，则证明对象是不可用的。
2. Java语言里，可以作为GC Roots的对象包括：虚拟机栈中引用的对象、方法区中得类静态属性引用的对象、方法区中常亮引用的对象、本地方法栈中JNI的引用对象

######引用
JDK引用概念将引用分为强引用(Strong Reference)、软引用(Soft Reference)、弱引用(Weak Reference)、虚引用(Phantom Reference)四种，这四种引用强度依次逐渐减弱。

1. 强引用：在程序代码中普遍存在的引用 Object obj = new Object()。只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象。
2. 软引用：描述一些还有用，但是非必须得对象。对于软引用关联着的对象，在系统将要发生内存溢出异常时，将会把这些对象列进回收范围之中并进行第二次回收。如果回收还是没有足够的内存，才会抛出内存溢出异常。
3. 弱引用：被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。
4. 虚引用：也称为幽灵引用或者幻影引用。一个对象是否有虚引用的存在，完全不会对其生存时间构成影响。也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用关联的唯一目的就是希望能在这个对象被收集器回收时受到一个系统通知。

###### finalize()方法
在根搜索算法中不可达的对象，处于缓刑状态，要真正宣告一个对象死亡，至少要经历两次标记过程：第一次标记和筛选条件是对象是否有必要执行finalize()方法。当对象没有覆盖finalize()方法或者finalize()方法已经被虚拟机调用过，虚拟机将这两种情况视为“没有必要执行”。如果对象被判定为有必要执行finalize()方法，那么这个对象将会被放置在一个名为F-Queue的队列中，并稍后由一条虚拟机自动建立的，低优先级的Finalizer线程去执行。

finalize的运行代价较高，不确定性非常大，无法保证各个对象的调用顺序。finalize能做的工作，使用try-finally或其他方法可以做得更好、更及时，尽量避免使用这个方法。

###### 回收方法区

永久代的垃圾收集主要回收两个部分：废弃常量和无用的类。
废弃常量： 当系统中没有任何一个对象引用此常量，就会被系统请出常量池
无用的类： 1.该类所有的实例都已被回收，也就是Java堆中不存在该类的任何实例。2.加载该类的ClassLoader已经被回收。3.该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。

是否进行类回收，HotSpot虚拟机提供了-Xnoclassgc参数进行控制。
-Xnoclassgc  禁用class垃圾收集
-verbose:class    -XX:+TraceClassLoading、-XX:+TraceClassUnLoading查看类的加载和卸载信息。

在大量使用反射、动态代理、CGLib等bytecode框架的场景，以及动态生成JSP和OSGI这类频繁自动以ClassLoader的场景都需要虚拟机具备类卸载的功能，以保证永久代不会溢出。


#####垃圾收集算法

######标记-清除算法
首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象。

缺点：一是效率问题，标记和清除过程效率不高。二是空间问题，标记和清除之后会产生大量不连续的内存碎片，空间碎片太多可能会导致当程序在以后的运行过程中需要分配较大对象时无法找到足够的连续内存空间而不得不提前触发另一次的垃圾收集动作。

######复制算法
将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间依次清理掉。

缺点：内存缩小为原来的一半，代价太高。

现代商业虚拟机使用复制算法来回收新生代，对于新生代中98%的对象是朝生夕死的，不需要按照1：1的比例来划分空间，二是将内存划分为一块较大的Eden空间和两块较小的Survivor空间。每次使用Eden空间和其中一块Survivor空间。当回收时，将Eden和Survivor空间存活着的对象一次性拷贝到另外一块Survivor空间上，最后清理掉Eden和Survivor空间。

HotSpot虚拟机默认Eden和Survivor的大小比例是8：1。

如果另外一块Survivor空间没有足够的空间存放上一次新生代收集下来的存活对象，这些对象将直接通过分配担保机制进入老年代。

######标记-整理算法
标记过程和标记-清除算法一样，但后续步骤不是直接对可回收对象进行清理，二是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。

######分代收集算法(Generational Collection)
当前商业虚拟机都使用分代收集算法。一般是把Java堆分为新生代和老年代，根据各个代的特点采用最适当的收集算法。

#####垃圾收集器

<figure>
     <a href="{{ site.url }}/images/blog2014/jvm_gc_generation.jpg"><img src="{{ site.url }}/images/blog2014/jvm_gc_generation.jpg"></a>
     <figcaption>Java GC收集器</figcaption>
</figure>

######Serial收集器
1. 单线程收集器。在垃圾收集时，必须暂停其他所有工作线程(Stop The World)。
2. **现代虚拟机运行在Client模式下得默认新生代收集器**。桌面应用场景中分配虚拟机内存一般只有几十到一两百兆的新生代，停顿时间可以控制在几十毫秒之内。

######ParNew收集器
1. Serial收集器的多线程版本。除了使用多条线程以外，其余行为包括Serial收集器可用得所有控制参数、收集算法、Stop The World、对象分配规则、收回策略等都与Serial收集器完全一样。
2. **许多运行在Server模式下得虚拟机中首选的新生代收集器**
3. 目前只有它能与CMS收集器配合工作。
4. ParNew收集器在单CPU环境下绝对不会有比Serial更好的效果。

######Parallel Scavenger收集器
1. 新生代收集器，使用复制算法的收集器，又是并行的多线程收集器。
2. Parallel Scavenger收集器的目的是达到一个可控制的吞吐量。吞吐量=运行用户代码时间/(运行用户代码时间+垃圾收集时间)
3. 提供两个参数： -XX:MaxGCPauseMills 设置最大垃圾收集停顿时间   -XX:GCTimeRatio 设置吞吐量大小
4. GC自适应调节策略(GC Ergonomics) : -XX:+UseAdaptiveSizePolicy 指定不需要手工指定新生代的大小(-Xmm)、Eden和Survival(-XX:SurvivorRatio)、晋升老年代对象年龄(-XX:PretenureSizeThreshold)等细节参数。虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间或最大的吞吐量。
5. 自适应调节是Parallel Scavenger收集器与ParNew收集器的一个重要区别。

######Serial Old收集器
1. Serial收集器的老年代版本。同样是一个单线程收集器，使用"标记-整理"算法。
2. 在Client模式下的虚拟机使用。
3. 在Server模式下：在JDK1.5之前的版本中与Parallel Scavenger收集器搭配使用，另一个就是作为CMS收集器的后备预案，在并发收集发生Concurrent Mode Failure的时候使用。

######Parallel Old收集器
1. Parallel Scavenger收集器的老年代版本，使用多线程和"标记-整理"算法。
2. 在吞吐量优先的情况下，在注重吞吐量及CPU资源敏感的场合，都可以优先考虑Parallel Scavenger加Parallel Old收集器。

######CMS收集器
1. CMS(Concurrent Mark Sweep)收集器是一种以获取最短回收停顿时间为目标的收集器。
2. 基于"标记-清除"算法实现
3. 步骤：初始标记(CMS initial mark)、并发标记(CMS concurrent mark)、重新标记(CMS remark)、并发清除(CMS concurrent sweep)。
4. 初始标记、重新标记这两个步骤任然需要"Stop the World"。初始标记仅仅标记一下GC Roots能直接关联到得对象，速度很快。并发标记阶段就是进行GC Roots Tracing的过程。重新标记阶段则是为了修正并发标记期间，用户因程序继续运行而导致标记产生变动的那一部分对象的标记记录。由于整个过程中最长的并发标记和并发清除过程中，收集线程都可以与用户线程一起工作，所以总体上来说，CMS收集器的内存回收过程是与用户线程一起并发地执行的。
5. 缺点： 1.对CPU资源非常敏感，会因为占用一部分线程资源导致应用程序变慢。2. 无法处理浮动垃圾，可能出现Concurrent Mode Failure失败而导致另一次Full GC 产生。3. 标记清除算法会形成大量碎片，空闲碎片过多时，将会给大对象分配带来麻烦，造成往往老年代还有很大的空间剩余，但是由于无法找到足够大的连续空间来分配对象，不得不提前触发一次Full GC。

######G1收集器
1. 基于标记-整理算法实现的收集器。
2. 可以做到基本不牺牲吞吐率的前提下完成低停顿的回收工作。

#####垃圾收集参数

可以参考这篇文章 [http://blog.sina.com.cn/s/blog_4080505a0101i6cr.html](http://blog.sina.com.cn/s/blog_4080505a0101i6cr.html)

#####MinorGC和Full GC的区别
新生代GC(Minor GC)指发生在新生代的垃圾收集动作。MinorGC非常频繁，且回收速度一般较快。
老年代GC(Major GC/Full GC)指发生在老年代的垃圾收集动作。出现了Major GC，经常会伴随着至少一次的Minor GC(但非绝对的)。MajorGC的速度一般会比MinorGC慢10倍以上。


#####Serial/Serial Old收集器下得内存分配和回收机制
1. **对象优先在新生代Eden区中分配**。当Eden区没有足够的空间进行分配时，虚拟机将发起一次MinorGC。
2. -XX:+printGCDetails 这个参数收集日志参数，告诉虚拟机在发生垃圾收集行为时打印内存回收日志，并且在进程退出的时候输出当前内存各区域的分配情况。内存回收日志一般是打印到文件后通过日志工具进行分析。
3. 需要大量连续内存空间的对象，就是所谓的**大对象直接进入老年代**。
4. 大对象对虚拟机的内存分配来说是一个坏消息(比遇到一个大对象更加坏得消息是遇到一群朝生夕灭的短命大对象)，写程序时应尽量避免，经常出现大对象容易导致内存还有不少空间时就提前触发垃圾收集以获取足够的连续空间来安置他们。
5. 虚拟机提供一个-XX:PretenureSizeThrehold参数，另大于这个值得对象直接在老年代中分配。目的是避免大对象在Eden区和Survivor区之间发生大量的内存拷贝。
6. **长期存活的对象将进入老年代**。虚拟机给每个对象定义了一个对象年龄Age计数器。如果对象在Eden出生并经过第一次MinorGC后仍然存活，并且能被Survivor容纳的话，将被移动到Survivor空间中，并将对象年龄设为1 。对象在Survivor区中每熬过一此MinorGC，年龄就增加1岁，当它的年龄增加到一定程度(默认15岁)时，就会被晋升到老年代中。
7. -XX:MaxTenuringThreshold来设置对象晋升老年代的年龄阈值。
8. **动态年龄判定**，虚拟机并不是总要求对象的年龄必须达到MaxTenuringThreshold才能晋升老年代，如果在Survivor中空间中相同年龄所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象就可以直接进入老年代。
9. **空间分配担保**（在MinorGC时如果Survivor空间无法容纳内存回收后的新生代对象，则需要老年代进行这样的担保），在发生MinorGC时，虚拟机会检测之前每次晋升到老年代的平均大小是否大于老年代的剩余空间大小，如果大于，则改为一次FullGC。如果小于，则查看HandlePromotionFailure设置是否允许担保失败，如果允许则进行MinorGC，如果不允许则进行一次FullGC。



####GC 日志和参数定义


如果希望查看程序的GC日志，需要在启动参数中增加参数`-verbose:gc` ，这个开关用于显示GC内容。可以显示最忙和最空闲收集行为发生的时间、收集前后的内存大小、收集需要的时间等。


`-XX:+printGCdetails` : 输出详细GC日志，可以详细了解GC中的变化。

`-XX:+PrintGCTimeStamps` : 输出GC的时间戳（以基准时间的形式）,可以了解这些垃圾收集发生的时间，自JVM启动以后以秒计量。

`-XX:+PrintGCDateStamps` : 输出GC的时间戳（以日期的形式，如 2013-05-04T21:53:59.234+0800）

`-XX:+PrintHeapAtGC` : 在进行GC的前后打印出堆的信息, 了解堆的更详细的信息。

`-XX:=PrintTenuringDistribution` : 开关了解获得使用期的对象权。

`-Xloggc:$CATALINA_BASE/logs/gc.log` : gc日志产生的路径

`-XX:+PrintGCApplicationStoppedTime` : 输出GC造成应用暂停的时间

也可以使用一些离线工具来进行GC日志的具体分析。比如sun的[gchisto](https://java.net/projects/gchisto)，[gcviewer](https://github.com/chewiebug/GCViewer)，这些都是开源的工具，用户可以直接通过版本控制工具下载其源码，进行离线分析。


####Sun 垃圾收集实现

实现代码

	/**
	*参数： -verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintHeapAtGC
	*/
	private static final int _1MB = 1024 * 1024;

	public static void main(String[] args) {

		byte[] allocation1, allocation2, allocation3, allocation4, allocation5;
		allocation1 = new byte[2 * _1MB];
		allocation2 = new byte[2 * _1MB];
		allocation3 = new byte[2 * _1MB];
		System.out.println("allocation1 = " + allocation1 + "\n allocation2 = " +allocation2 + "\n allocation3 = " + allocation3);
		allocation4 = new byte[4 * _1MB]; // 此时出现一次MinorGC
		//allocation1 = null;
		allocation5 = new byte[2 * _1MB];
		//byte[] allocation6 = new byte[6 * _1MB];
	}


返回结果：

	allocation1 = [B@150697e2
	 allocation2 = [B@2a36bb87
	 allocation3 = [B@4fd281f1
	{Heap before GC invocations=1 (full 0):
	 PSYoungGen      total 9216K, used 7143K [0x00000007ff600000, 0x0000000800000000, 0x0000000800000000)
	  eden space 8192K, 87% used [0x00000007ff600000,0x00000007ffcf9eb0,0x00000007ffe00000)
	  from space 1024K, 0% used [0x00000007fff00000,0x00000007fff00000,0x0000000800000000)
	  to   space 1024K, 0% used [0x00000007ffe00000,0x00000007ffe00000,0x00000007fff00000)
	 ParOldGen       total 10240K, used 4096K [0x00000007fec00000, 0x00000007ff600000, 0x00000007ff600000)
	  object space 10240K, 40% used [0x00000007fec00000,0x00000007ff000010,0x00000007ff600000)
	 PSPermGen       total 21504K, used 2590K [0x00000007f9a00000, 0x00000007faf00000, 0x00000007fec00000)
	  object space 21504K, 12% used [0x00000007f9a00000,0x00000007f9c87a70,0x00000007faf00000)
	2015-01-05T15:18:41.818-0800: [GC-- [PSYoungGen: 7143K->7143K(9216K)] 11239K->15335K(19456K), 0.0048480 secs] [Times: user=0.01 sys=0.00, real=0.01 secs] 
	Heap after GC invocations=1 (full 0):
	 PSYoungGen      total 9216K, used 7143K [0x00000007ff600000, 0x0000000800000000, 0x0000000800000000)
	  eden space 8192K, 87% used [0x00000007ff600000,0x00000007ffcf9eb0,0x00000007ffe00000)
	  from space 1024K, 0% used [0x00000007fff00000,0x00000007fff00000,0x0000000800000000)
	  to   space 1024K, 40% used [0x00000007ffe00000,0x00000007ffe68020,0x00000007fff00000)
	 ParOldGen       total 10240K, used 8192K [0x00000007fec00000, 0x00000007ff600000, 0x00000007ff600000)
	  object space 10240K, 80% used [0x00000007fec00000,0x00000007ff400030,0x00000007ff600000)
	 PSPermGen       total 21504K, used 2590K [0x00000007f9a00000, 0x00000007faf00000, 0x00000007fec00000)
	  object space 21504K, 12% used [0x00000007f9a00000,0x00000007f9c87a70,0x00000007faf00000)
	}
	{Heap before GC invocations=2 (full 1):
	 PSYoungGen      total 9216K, used 7143K [0x00000007ff600000, 0x0000000800000000, 0x0000000800000000)
	  eden space 8192K, 87% used [0x00000007ff600000,0x00000007ffcf9eb0,0x00000007ffe00000)
	  from space 1024K, 0% used [0x00000007fff00000,0x00000007fff00000,0x0000000800000000)
	  to   space 1024K, 40% used [0x00000007ffe00000,0x00000007ffe68020,0x00000007fff00000)
	 ParOldGen       total 10240K, used 8192K [0x00000007fec00000, 0x00000007ff600000, 0x00000007ff600000)
	  object space 10240K, 80% used [0x00000007fec00000,0x00000007ff400030,0x00000007ff600000)
	 PSPermGen       total 21504K, used 2590K [0x00000007f9a00000, 0x00000007faf00000, 0x00000007fec00000)
	  object space 21504K, 12% used [0x00000007f9a00000,0x00000007f9c87a70,0x00000007faf00000)
	2015-01-05T15:18:41.823-0800: [Full GC [PSYoungGen: 7143K->2343K(9216K)] [ParOldGen: 8192K->8192K(10240K)] 15335K->10535K(19456K) [PSPermGen: 2590K->2589K(21504K)], 0.0105960 secs] [Times: user=0.02 sys=0.00, real=0.01 secs] 
	Heap after GC invocations=2 (full 1):
	 PSYoungGen      total 9216K, used 2343K [0x00000007ff600000, 0x0000000800000000, 0x0000000800000000)
	  eden space 8192K, 28% used [0x00000007ff600000,0x00000007ff849d00,0x00000007ffe00000)
	  from space 1024K, 0% used [0x00000007fff00000,0x00000007fff00000,0x0000000800000000)
	  to   space 1024K, 0% used [0x00000007ffe00000,0x00000007ffe00000,0x00000007fff00000)
	 ParOldGen       total 10240K, used 8192K [0x00000007fec00000, 0x00000007ff600000, 0x00000007ff600000)
	  object space 10240K, 80% used [0x00000007fec00000,0x00000007ff400030,0x00000007ff600000)
	 PSPermGen       total 21504K, used 2589K [0x00000007f9a00000, 0x00000007faf00000, 0x00000007fec00000)
	  object space 21504K, 12% used [0x00000007f9a00000,0x00000007f9c876a0,0x00000007faf00000)
	}
	Heap
	 PSYoungGen      total 9216K, used 4801K [0x00000007ff600000, 0x0000000800000000, 0x0000000800000000)
	  eden space 8192K, 58% used [0x00000007ff600000,0x00000007ffab04b0,0x00000007ffe00000)
	  from space 1024K, 0% used [0x00000007fff00000,0x00000007fff00000,0x0000000800000000)
	  to   space 1024K, 0% used [0x00000007ffe00000,0x00000007ffe00000,0x00000007fff00000)
	 ParOldGen       total 10240K, used 8192K [0x00000007fec00000, 0x00000007ff600000, 0x00000007ff600000)
	  object space 10240K, 80% used [0x00000007fec00000,0x00000007ff400030,0x00000007ff600000)
	 PSPermGen       total 21504K, used 2599K [0x00000007f9a00000, 0x00000007faf00000, 0x00000007fec00000)
	  object space 21504K, 12% used [0x00000007f9a00000,0x00000007f9c89e30,0x00000007faf00000)

可以根据这个了解到部分Sun Java虚拟机GC实现细节：

堆被划分成三个不同的区域：新生代 ( PSYoungGen )、老年代 ( ParOldGen )、永久代（ PsPermGen）。 

新生代 ( Young ) 又被划分为三个区域：
Eden、From Survivor、To Survivor。
这样划分的目的是为了使 JVM 能够更好的管理堆内存中的对象，包括内存的分配以及回收。 其中的比例默认为8:1:1， 也可以使用参数修改。

新生代和老年代的比例一般为1:2， 也可以自己通过参数定义。
JVM 每次只会使用 Eden 和其中的一块 Survivor 区域来为对象服务，所以无论什么时候，总是有一块 Survivor 区域是空闲着的。

SUN JVM GC 使用是分代收集算法，即将内存分为几个区域，将不同生命周期的对象放在不同区域里。新的对象会先生成在Young area，也就是PSYoungGen中。在几次ＧＣ以后，如过没有收集到，就会逐渐升级到PSOldGen 及Tenured area(也就是PSPermGen)中。

#####PSYoungGen ParOldGen PsPermGen区别
在GC收集的时候，频繁收集生命周期短的区域(Young area)，因为这个区域内的对象生命周期比较短，GC 效率也会比较高。而比较少的收集生命周期比较长的区域(Old area or Tenured area)，以及基本不收集的永久区(Perm area)。




