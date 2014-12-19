---
layout: post
title: JVM深度剖析——内存区域
description: "JVM深度剖析——内存区域"
modified: 2014-12-08
category: articles
tags: [java,jvm]
comments: true
share: true
---

###jvm 深度剖析

#### jvm内存区域

Java虚拟机运行时数据区

<figure>
     <a href="{{ site.url }}/images/blog2014/jvm_runtime_data_zone.jpg"><img src="{{ site.url }}/images/blog2014/jvm_runtime_data_zone.jpg"></a>
     <figcaption>Java虚拟机运行时数据区图</figcaption>
</figure>

#####程序计数器(Program Counter Register)：
1. 较小的内存空间，是当前线程所执行的字节码的行号指示器
2. **线程私有的**，每个线程都需要一个独立的计数器，每个计数器之间都互不影响，独立存储
3. 程序计数器内存区域是Java虚拟机规范中唯一没有规定任何OutOfMemory情况的区域


#####Java虚拟机栈(Java Virtual Machine Stacks):
1. **线程私有的**，它的生命周期与线程相同。
2. 每个方法被执行的时候都会同时创建一个栈帧用于存储局部变量表、操作栈、动态链接、方法出口等信息。每个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入到出栈的过程。
3. 局部变量表存放编译期间可知的各种基本数据类型(boolean、byte、char、short、int、float、long、double）对象引用(Reference类型，根据不同的Java虚拟机实现，可能是一个指向对象起始地址的引用指针，也可能是一个代表对象句柄或者其他于此对象相关的位置)和returnAddress类型(指向一条字节码指令的地址)
4. 局部变量表所需的存储空间在编译期间完成分配。其中64位长度的long和double会占用2个局部变量空间，其余的数据类型只占用1个。在方法运行期间不会改变局部变量表的大小。
5. Java虚拟机栈区定义了两种异常状况：如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常；如果虚拟机栈可以动态扩展，当扩展是无法申请到足够的内存时会抛出OutOfMemoryError异常。

#####本地方法栈(Native Method Stacks):
1. 为虚拟机使用到的Native方法服务。
2. 同样也会抛出StackOverflow和OutOfMemoryError异常。

#####Java堆(Java Heap)
1. Java堆是Java虚拟机所管理的内存中最大的一块。
2. **所有线程共享一块内存区域**，在虚拟机启动时创建。
3. Java堆的唯一目的就是存放对象实例。Java规范中得描述是所有的对象实例以及数组都要在堆上分配。 （但当前出现了一些栈上分配、标量替换的新方法。）
4. 堆划分， 内存回收的角度：新生代和老年代。 Eden空间、From Survivor空间、To Survivir空间。 从内存分配的角度：线程共享的Java堆中可以划分出多个线程私有的分配缓冲区(Thread Local Allocation Buffer, TLAB)。
5. Java堆可以处于物理上不连续的内存空间中，只要逻辑上是连续的即可。
6. 如果堆中没有内存完成实例分配，并且堆也无法再扩充时，将会抛出OutOfMemoryError异常。

#####方法区(Method Area)
1. 各个线程共享的内存区域。别名叫Non-Heap。
2. 用于存储已被虚拟机加载的类信息、常亮、静态变量、即时编译器编译后的代码数据。
3. Java虚拟机规范对这个区域的限制非常宽松，除了和Java堆一样不需要连续的内存和可以选择固定大小和可扩展外，还可以选择不实现垃圾收集。相对而言，垃圾收集在这个区域是比较少出现的。这个区域内存回收的目标主要是针对常量池的回收和对类型的卸载，一般来说对这个区域的回收成绩比较难以令人满意。
4. 当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常。

#####运行时常量池(Runtime Constant Pool)
1. 方法区的一部分
2. Class文件中除了有类的版本、字段、方法、接口等描述信息，还有一项信息是常量池(Constant Pool Table)，用于存放编译器生成的各种字面量和符号引用。
3. Java虚拟机规范未对运行时常量池做任何细节要求，不同的提供商实现的虚拟机可以按照自己的需求来实现这个内存区域
4. Java语言并不要求常量一定只能在编译期产生，也就是并非预置入Class文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中。如使用String类的intern()方法。
5. 当常量池无法再申请到内存时会抛出OutOfMemoryError异常。

#####直接内存(Direct Memory)
1. 并不是虚拟机运行时数据区的一部分，也不是Java虚拟机规范中定义的内存区域。
2. NIO类，引入了一种基于通道(Channel)与缓冲区(Buffer)的I/O方式，可以使用Native函数直接分配堆外内存，然后通过一个存储在Java堆里面的DirectByteBuffer对象作为这块内存的引用进行操作。这样可以避免在Java堆和Native堆中来回复制数据，提升性能。
3. 本机直接内存的分配不会受到Java堆大小的限制。但会受到本机总内存大小的限制。各个区域的总和大于物理内存限制时，会导致动态扩展时出现OutOfMemoryError异常。


