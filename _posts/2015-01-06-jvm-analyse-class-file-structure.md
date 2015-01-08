---
layout: post
title: JVM深度剖析——类文件结构
description: "JVM深度剖析——类文件结构"
modified: 2014-12-22
category: articles
tags: [java,jvm]
comments: true
share: true
---

###类文件结构

####平台无关性
1. 平台无关性最终实现在操作系统的应用层上：Sun公司和其他虚拟机提供商发布了许多可以运行在不同操作系统上得虚拟机，这些虚拟机都可以从载入和执行同一种平台无关的字节码，从而实现程序的“一次编写，到处执行”。
2. 实现语言无关性的基础仍然是虚拟机和字节码存储格式，使用Java编译器可以把Java代码编译为存储字节码的Class文件，使用Jruby等其他语言同样可以把程序代码编写成Class文件，虚拟机并不关心Class的来源是什么语言，只要它符合Class应有的结构就可以在Java虚拟机中运行。


####Class类文件结构
class文件格式：

<figure>
     <a href="{{ site.url }}/images/blog2015/jvm_class_file_class_type.png"><img src="{{ site.url }}/images/blog2015/jvm_class_file_class_type.png"></a>
     <figcaption>JVM class 文件格式</figcaption>
</figure>

* Class文件是一组以8位字节为基础单位的二进制流，各个项目紧密严格按照顺序紧凑的排列在Class文件中，中间没有添加任何分隔符。
* Class文件格式只有两种数据类型：**无符号数和表**。无符号数属于基本数据类型，以u1、u2、u4、u8来代表1，2，4，8个字节的无符号数。无符号数可以用来描述数字、索引引用、数量值，或者按照UTF-8编码构成的字符串值。表是由多个无符号数或者其他表作为数据项构成的符合数据类型。所有表都习惯的以"_info"结尾。整个Class文件本质上就是一张表。
* “**1 魔数**” : **每个Class的头4个字节称为魔数**，它用于确定这个文件是否为一个能被虚拟机接受的Class文件。
* **Class文件的魔数： 0xCAFEBABE**
* ”**2 文件版本号**“ : 魔数后面的4个字节存储的是**Class文件的版本号**，第5和第6个字节是次版本号，第7和第8个字节是主版本号。
* “**3 常量池**” : 主次版本号之后的是**常量池入口**。常量池中的常量是不固定的，所以常量池的入口需要放置一项u2类型的数据，代表常量池容量计数值(constant_pool_count)
* 常量池中主要放置两大类常量：**字面量(Literal)和符号引用(Symbolic Reference)**，字面量代表文本字符串、被声明为final的常量值等。符号引用则属于编译原理方面的概念，主要包括：类和接口的全限定名、字段的名称和描述符、方法的名称和描述符。
* 常量池中得每一项常量都是一个表，共有11种结构各不相同的数据结构，这11种表开始的第一位都是一个u1类型的标志位代表当前常量属于哪一种常量类型。这11种类型均各自有各自的结构。
   
<figure>
     <a href="{{ site.url }}/images/blog2015/jvm_class_file_proj_type.png"><img src="{{ site.url }}/images/blog2015/jvm_class_file_proj_type.png"></a>
     <figcaption>JVM class 项目类型</figcaption>
</figure>
<figure>
     <a href="{{ site.url }}/images/blog2015/jvm_class_file_proj_type_detail.png"><img src="{{ site.url }}/images/blog2015/jvm_class_file_proj_type_detail.png"></a>
     <figcaption>JVM class 项目类型格式</figcaption>
</figure>
* ”**4 访问标识**“ :  常量池后的2各字节代表访问标识(access_flags)，这个标识用于识别一些类或接口的访问层次。access_flags一共有32个标志位可以使用，当前只定义了其中的8个，没有使用到的标志位一致为0
<figure>
     <a href="{{ site.url }}/images/blog2015/jvm_class_file_access_type.jpg"><img src="{{ site.url }}/images/blog2015/jvm_class_file_access_type.jpg"></a>
     <figcaption>JVM class访问标志</figcaption>
</figure>
* “**5 类索引和父类索引**” : 类索引(this_class)和父类索引(super_class)都是一个u2类型的数据集合，而接口索引(interfaces)是一组u2类型的数据的集合。Class文件由这3项来确定这个类的继承关系。 类索引、父类索引和接口索引都顺序的排列在访问标识之后。类索引和父类索引用两个u2类型的索引值表示。对于接口索引集合，入口第一项使用u2类型的数据位接口计数器，表示索引表的容量。
* ”**6 字段表集合**“ : 用于描述接口或类中声明的变量。字段包括类级变量或实例级变量，但不包括在方法内部声明的变量。
整个 字段表示 = 作用域 + 静态or非静态 + 可变性 + 并发可见性 + 是否可序列化 + 数据类型 + 字段名称 + 其他属性	

字段表数据结构：

		Field_info{
			u2 access_flags;
			u2 name_index;
			u2 descriptor_index;
			u2 attributes_count;
			attribute_info attributes;
		}

其中access_flags [ACC_PUBLIC, ACC_PRIVATE, ACC_PROTECTED, ACC_STATIC, ACC_FINAL, ACC_VOLATILE, ACC_TRANSIENT, ACC_SYNTHETIC, ACC_ENUM]。

name_index 字段的简单名称，没有类型和参数修饰的方法或字段名称。
descriptor_index 字段和方法的描述符，用于描述字段的数据类型、方法的参数列表(数量、类型及顺序)和返回值。 
attributes_count 和 attributes 用于描述一些其他属性。

字段表中不会列出从超类或者父类接口中继承的字段，但有可能列出原来java代码中不存在的字段。

在Java语言中字段是无法重载的，两个字段的数据类型，修饰符不管是否相同，都必须使用同样的名称。但对于字节码来说，如果两个字段的描述符不一致，那字段重名就是合法的。

* ”**7 方法表集合**“ :  Class文件存储格式中对方法的描述与对字段的描述几乎采用了完全一致的方式。

方发表结构：

		method_info{
			u2 access_flags;
			u2 name_index;
			u2 descriptor_index;
			u2 attributes_count;
			attribute_info attributes;
		}

access_flags [ACC_PUBLIC, ACC_PRIVATE, ACC_PROTECTED, ACC_STATIC, ACC_FINAL, ACC_SYNCHRONIZED, ACC_BRIDGE, ACC_VARARGS, ACC_NATIVE, ACC_ABSTRACT, ACC_STRICT, ACC_SYNTHETIC]

方法的代码经过编译器编译成字节码之后，存放在方法属性表集合中一个命名为"Code"的属性里面。

如果父类方法在子类中没有重写，方法表集合中就不会出现来自父类的方法信息。但有可能出现由编译器自动添加的方法，如<init>方法。

**重载的判断** : 要重载Overload 一个方法，除了要与原方法具有相同的简单名称之外，还要求必须有一个与原方法不同的特征签名，特征签名就是一个方法中各个参数在常量池中得字段符号引用的集合。由于返回值不包含在特征签名之中，因此Java语言里面无法仅仅依靠返回值的不同来对一个已有的方法进行重载的。 但是在Class文件中，如果两个方法有相同的名称和特征签名，但返回值不同，依然可以合法的共同存在。

* “**8 属性表集合**” : 属性表不要求严格的顺序，并且只要不与已有的属性名重复，任何人实现的编译器都可以向属性表中写入自己定义的属性信息，Java虚拟机运行时会忽略掉它不认识的属性。
<figure>
     <a href="{{ site.url }}/images/blog2015/jvm_class_file_attribute_table.jpg"><img src="{{ site.url }}/images/blog2015/jvm_class_file_attribute_table.jpg"></a>
     <figcaption>虚拟机规范预定义的属性</figcaption>
</figure>
