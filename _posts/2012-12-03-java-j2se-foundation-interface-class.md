---
layout: post
title: java类方法和接口总结详解
description: "java类方法和接口总结详解"
modified: 2012-12-03
category: articles
tags: [java,j2se]
comments: true
share: true
---

#### 类方法

类方法： 静态的，被虚拟机加载的时候内存中仅存在一份独一无二的拷贝
实例方法： 实例化某个对象的同时，会为对象中得该方法分配一段内存，每个实例一个

####接口
* 实现一个接口必须重写接口中得所有方法
* 接口的特性：

	① 接口可以多重实现，即一个类可以实现多个接口

	② 接口中声明的属性默认为public static final，也只能是public static final，当不写时默认为这个。

	③ 接口中只能定义抽象方法，而且这些方法默认为public，也只能是public

	④ 接口可以继承其他接口，并添加新的属性和抽象方法

	⑤ java接口可以有public、静态和final属性

	⑥ Java接口中的方法默认都是public、abstract类型的，没有方法体，不能被实例化。

	⑦ Java接口中只能包含public、static、final类型的成员变量和public、abstract类型的成员方法。

	⑧ 当类实现了某个Java接口时，它必须实现接口中得所有抽象方法，否则这个类必须声明为抽象类。 **抽象类可以实现接口**。

####接口分类

接口分类：

① 普通接口

② 标识接口：没有任何方法和属性的接口，仅仅表明实现它的类属于一个特定的类型

③ 常量接口：由实现这个接口的类使用这些常量


#### 访问权限控制列表

|   访问权限   |  同一个类内部 | 同一个包内部 | 不同包子类  | 不同包非子类 |
|:-----------:|:-----------:|:----------:|:----------:|:----------:| 
|    public   |      yes    |     yes    |     yes    |     yes    |
|  protected  |      yes    |     yes    |     yes    |     yes    |
|    default  |      yes    |     yes    |     no     |     no     |
|    pricate  |      yes    |     no     |     no     |     no     |






