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

`-XX:PermSize=64M`	 JVM初始分配的非堆内存
`-XX:MaxPermSize=128M` JVM最大允许分配的非堆内存，按需分配

`-XX:+HeapDumpOnOutOfMemoryError`: 让虚拟机在出现内存溢出异常时dump出当前内存堆转储快照以便事后进行分析。
带上这种参数之后运行Jvm，如果出现相应的内存溢出异常，会在目录下形成一个异常时候的内存dump文件(如java_pid7126.hprof文件)，将这个文件使用Memory Analyse Tool工具打开就可以看到当前dump内存空间的分析内容。

`-XX:+HeapDumpOnCtrlBreak`: 


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





