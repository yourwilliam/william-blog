---
layout: post
title: java经典基础知识集合详解
description: "java经典易出错的基础知识集合详解"
modified: 2012-12-10
category: articles
tags: [java,j2se]
comments: true
share: true
---

最近把之前在公司内部考试的复习资料进行整理一下，也顺便回顾一下之前的知识。

###java 经典基础知识

####对象的声明和初始化

* static函数只能访问static变量或static函数。
* 对象饮用唯一的缺省值是null
	
	char : '\u0000'
	boolean : false
	Reference : null
* 刚构造的数组中的元素总是设为默认值
	
	声明一个数组仅产生了一个数组引用，但未分配数组的存储空间，一个数组的大小在分配数组内存时才确定的。
	`System.arrayCopy()` 拷贝数组比循环要快很多。
* final变量
	
	声明为final的**成员变量**必须在构造对象的过程中完成初始化：1.定义处 2.初始化快中 3.构造函数中
	声明为final的**静态变量**不必在构造函数中初始化，但必须在静态上下文中初始化一次(定义时)静态区段即可。
	声明为final的**局部变量**必须(且只能)在使用前初始化一次，不使用的话可以不用初始化。
* final用法
	
	final类： 不能再派生新的子类
	final函数：不能被子类覆写
	final参数：只能引用，不能修改
* 常量： 不是所有的final数据都是常量，只有再编译期间能确定数字的才能算是常量
	
	final static也可以声明常量
* 构造函数： 在构造函数中避免调用子类可以覆写的函数
* 有继承关系的类之间的初始化顺序是先静态后非静态，先父后子
	
	类的初始化：
		
	① 父类的类初始化 
	
	② 类变量分配存储空间
	
	③ 类变量显示赋值
	
	④ 执行static块
	1步如果类在初始化中需要使用其他未加载的类，则对未加载的类也会执行上述过程。
 	2步在给类变量分配存储空间时，由于新分配的存储空间都被虚拟机初始化过了，所以自动取得了默认值，虚拟机会通过某些方法，如调用memset()函数将一大块内存全部置为默认值，这在java代码中不可见，编译出来的字节码也没有相关的内容，如果没有显示赋值则声明变量的地方不能设置断点便是这个原因

 	对象的初始化：

 	① 给对象分配存储空间
 	
	② 父类的对象初始化过程
 	
	③ 给对象变量显示赋值
 	
	④ 执行构造函数

* instance of 运算符

	getClass() 用于获取对象的真是类型，两个有继承关系的类分别调用getClass获得的类是不同的。
	instance of用来判断一个对象是否是某种类型的，即IS-A关系，子类对象是父类对象的实例。
	null对象不是任何类的实例

* 对象是通过引用传递的，引用变量是通过值传递的。
* 覆写equals()函数须遵循一下约定：

	① 不同类型的对象的equals函数总返回false

	② 对于任意的非空引用值x, x.equals(null)返回false

	③ 自反性原则： 和自己比较返回true

	④ 对称性原则： `x.equals(y)=true ~ y.equals(x)=true`

	⑤ 传递性： 

	⑥ 一致性原则：多次调用结果相同。
* 覆写hashCode()注意事项

	在每个覆写了equals函数的类中，必须要改写hashcode函数

	① 如果一个对象的equals函数比较所用到的对象的信息没有修改的话，那么该对象多次调用hashCode()，必须返回同一个整数  (多次调用值唯一)

	② 如果两个对象根据equals函数是相等的，那么调用这两个对象中任意一个对象的hashcode函数必产生相同的整数结果

	③ 如果equals步相等，则不需要要求必须产生相同的结果
* 克隆

	Object.clone()函数仅实现浅层拷贝————只把基本数据类型和引用类型值复制给新对象。

	① 各自拥有数值相同的基本数据类型的值

	② 共享引用类型对象成员
	正③覆写clone()函数

	① 要支持克隆必须实现Cloneable接口

	② 实现clone函数时要记得调用super.clone()函数，实现浅层克隆

	③ 如果缺省的浅克隆步符合要求，再实现深层克隆

* String对象一旦被创建，它就不能再被改变

	不要使用new String("...")来创建对象，这样会创建两次造成浪费。
	从byte[]数组创建字符串时，应使用new String(byte[])。构建bytep[]数组的toString实际上是Object的toString
	大量字符串相加，应该使用StringBuilder,StringBuffer

* Switch 

	switch语句必须配套default分支(良好的编程风格)
	各分支之间不能遗漏break
	switch 语句只能用byte,char,short,int做参数。或者Integer,Chacter,Short,Byte类型  （java 1.7版本好像可以支持用String作为switch参数了）
	case的表达式必须是常量，不能是变量。

* 异常处理

	语法上允许：
		`try... catch`
		`try... finally`
		`try... catch... finally`
	对于资源的释放建议放在finally中进行处理，只有一种情况下finally子句不会被执行，那就是在try或者catch块中执行了System.exit()函数。
	在子类中覆写函数时只能抛出父类中声明过的异常或异常的子类

* 循环的性能

	避免在循环体中创建对象
	循环的嵌套层次———— 小的写在外面
	尽量避免在循环体中使用try catch块，最好在循环体外使用try catch提高性能。
	建议使用局部变量保存循环测试。

* 接口中声明的属性默认为public static final的，实现该接口的类，可通过类名直接调用接口中的变量。
* 接口中函数定义默认为public abstract的
* 内部类的特征：

	①可使用包含他的类的静态和实例成员变量，即使他们在外围时private的

	②若被声明为static，就不能再访问其外部类得非静态成员

	③若想在Inner Class中声明任何static成员，则该Inner Class必须声明为static

	对于局部变量，只有final的才能被内部类使用。

* 一个接口可以继承多个借口
* static函数不能被覆写，所以static函数没有多态性
* 非静态内部类不能持有外部类对象的引用，可以访问外部类的所有成员。而静态内部类则不持有其外部类对象的引用，只能访问其外部类的静态成员。
* 线程

	① 用一个类实现java.lang.Runnable接口，构造这个类的实例，然后通过这个实例构造一个java.lang.Thread类的实例，覆写run函数。

	② 尽管run函数是线程代码执行的起点，但要启动一个线程只能调用start函数。

	③ 在代码直接调用run()并不会新生线程，只是在原来的线程中执行run函数中的代码

	④ 实现runnable接口不等于生成了线程

	⑤ 只有线程运行结束了，线程对象的引用资源才能被回收。

* 三种声明互斥代码的方法：

	① 同步块(对象锁)： synchronized(对象){互斥代码}

	② 成员函数锁(实例锁)：成员函数加上synchronized修饰

	③ 静态函数锁(类锁)：静态函数上加

	synchronized块应该尽可能的小

* Collections是java.util下的类，包含各种有关集合操作的静态函数实现对各种集合的搜索、排序、线程安全等操作，但Collections不允许被继承。

	Collections 是 java.util下的接口，是各种集合结构的父接口。
	HashMap非线程安全，HashTable是线程安全的。 HashMap效率较高，允许null作为key和value
	Vectory是线程安全的，ArrayList不是
	当需要增长时，vectory增加一倍，ArrayList增长0.5倍
	Map中不允许重复key，多次存入时，最后一次的有效。
	ArrayList、Vectory、LinkedList

* Set

	Set拒绝持有重复的元素，另外不能通过任何索引的函数来操作Set对象
	HashSet、TreeSet、LinkedSet

* JVM在加载类的时候，如果没有给出classpatch则使用当前路径，如果给出了则使用classpatch。

	如果classpath中没有说明当前路径，即没有包含(.)，则JVM不会在当前路径搜索。
	在classpath中如果有重名类，JVM只加载第一个找到的类，其他被忽略。

* 换行

	windows "\r\n" Linux "\n"
	平台无关 System.getproperty("line.seperator");
	分隔符  System.getproperty("file.seporator");
	建议无论何时都使用"/"作为文件分隔符

* web服务器跟踪客户状态的四种

	① 建立含有隐藏数据的隐藏表单字段(hidden field)

	② 重写包含额外参数的URL (URL rewritting)

	③ 使用持续的cokkie

	④ 使用session会话机制

* Get请求的参数添加在URL后面会有安全问题，对于提交的数据大小有限制。Post请求的参数封装在请求体中较安全，可支持大数量的提交。 
















	








