---
layout: post
title: java经典基础————多态和重载
description: "java经典易出错的基础知识集合详解————多态和重载"
modified: 2012-12-05
category: articles
tags: [java,j2se]
comments: true
share: true
---

####方法继承

* 当sub类和Base类在同一个包时Sub类继承Base类中得public、protected、default级别的变量和方法
* 在不同包时继承public、protected级别的变量和方法

####方法重载

* 方法名相同
* 方法的参数类型、个数顺序至少有一项不同
* 方法的返回类型可以不相同
* 方法的修饰符可以不相同
* main方法也可以被重载

####方法覆盖

* 名称、返回类型、参数签名
* 子类的方法名称、返回类型、参数类型必须与父类一致
* 子类方法不能缩小父类方法的访问权限 (但可以增大)
* 父类的静态方法不能被子类覆盖为非静态方法
* 子类可以定义与父类的静态方法同名的静态方法，以便在子类中隐藏父类的静态方法(满足覆盖约束)，**静态方法会继承的**
* 父类的非静态方法不能被子类覆盖为静态方法
* 父类的私有方法不能被子类覆盖
* 父类的抽象方法可以被子类通过两种途径覆盖(实现和覆盖)
* 父类的非抽象方法可以被覆盖为抽象方法
* **异常的抛出： 子类方法抛出的异常只能是父类的子类，或和父类相同**

####super关键字

super和this关键字可用来覆盖Java的默认作用域，使被屏蔽的变量和方法变为可见。

* 父类的成员变量和方法为private使用super访问编译出错。
* 在子类中访问父类的被屏蔽的方法和属性
* 只能在构造方法或实力方法内使用super关键字，而在静态方法和静态代码块中不能使用super

####try-finally中return的执行顺序

* finally始终执行，如果在finally中有return，始终返回这个值
* 当不抛错和try中return a，finally中没有return语句时，try的return方法会先执行，finally的方法最后执行，然后再返回到主方法
* finally中得return级别最高
* return的值会保存下来，不受后面finally的影响

####abstract方法

* abstract方法不可以是private的
* 接口方法只接受2个修饰符，public和abstract
* 接口中变量默认为final static
* 一个类继承父类，实现接口，如果其中有相同的成员变量，在调用时如果有直接调用会出错。

