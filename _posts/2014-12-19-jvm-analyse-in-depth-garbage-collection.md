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

#####根搜索算法(GC Rooting Tracing)
1. 通过一系列名为"GC Routes"的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径成为引用链(Reference Chain)，当一个对象到GC Roots 没有任何引用链相连时，则证明对象是不可用的。
2. Java语言里，可以作为GC Roots的对象包括：虚拟机栈中引用的对象、方法区中得类静态属性引用的对象、方法区中常亮引用的对象、本地方法栈中JNI的引用对象
3. 



