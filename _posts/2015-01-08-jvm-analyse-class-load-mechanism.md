---
layout: post
title: JVM深度剖析——类加载机制
description: "JVM深度剖析——类加载机制"
modified: 2015-01-08
category: articles
tags: [java,jvm]
comments: true
share: true
---

### 虚拟机类加载机制

####类加载机制
* 虚拟机把描述类的数据从Class文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型。
* Java类型的加载和链接过程都是在程序运行期间完成的，这样会在加载时稍微增加一些性能开销，但是却能为Java应用程序提供更高度的灵活性。Java中的动态扩展语言特性就是这个提供的。

####类加载时机
* 类加载的生命周期：加载(Loading)、验证(、准备、解析、初始化、使用和卸载。