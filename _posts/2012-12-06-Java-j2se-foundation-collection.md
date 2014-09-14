---
layout: post
title: java经典基础————集合框架
description: "java经典易出错的基础知识集合详解————集合框架"
modified: 2012-12-05
category: articles
tags: [java,j2se]
comments: true
share: true
---

#### ArrayList 和 Vector

* 可按索引号来获取元素，二者中数据是可重复的。
* 区别： 

	① 同步性： Vector线程安全， Hashtable线程安全， ArrayList线程不安全， HashMap线程不安全。
	② 数据增长： Vector增长原来的一倍， ArrayList 增长为原来的0.5倍。

#### HashMap 和 Hashtable

* HashMap是Hashtable的轻量级实现(非线程安全的实现)
* HashMap允许空(null)键值(key)，Hashtable不允许。

#### List、Set继承自Collection， Map不是

* Map继承自java.util.AbstractMap
* Hashtable继承自java.util.Dictionary

#### ArrayList 和 Vector都是使用数组方式存储数据，索引数据快而插入数据慢。

#### ArrayList、LinkedList

* ArrayList：以索引值访问对象效率高，插入和移走元素效率较低。优点在于随机访问。
* LinkedList：顺序数据访问，插入、移走元素效率高

#### HashSet、TreeSet、LinkedHashSet

* HashSet 不允许插入重复值，使用了一个HashMap实例
* TreeSet 实现了SortedSet接口，自动排序，使用TreeMap实例
* LinkedHashSet 迭代顺序和插入顺序一致

#### HashMap、TreeMap、WeekHashMap、LinkedHashMap

* HashMap Map基于散列表的实现
* TreeMap 基于红黑树顺序存放关键字来实现SortedMap接口

#### Vector、Stack、Hashtable

* Vector线程安全
* Stack是Vector的子类
* Hashtable同步化




