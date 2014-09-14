---
layout: post
title: java线程详解
description: "java基础————线程详解"
modified: 2012-12-03
category: articles
tags: [java,j2se]
comments: true
share: true
---

### Java线程

#### 线程阻塞

* sleep() : 指定以毫秒为单位的一段时间作为参数，他使得线程在指定的时间进入阻塞状态
* suspend() ：使线程进入阻塞状态，并且不会自动恢复，必须使用resume()对其进行resume()调用们才能使得线程重新进入可执行状态
* yield()： 使线程放弃当前分得得CPU时间，但不使线程进入阻塞状态，随时可能再次分得CPU时间
* notify() , wait():

	wait()：使得线程进入阻塞状态，它有两种形式，一种允许指定以毫秒为单位的一段时间作为参数，另一种无参数。当对应notify()被调用或者超出指定的时间线程重新进入可执行状态

	两个要求：1 直接隶属于Object类，所有对象拥有这一方法。 2 必须在synchronized方法或块中调用。

	wait() 导致线程阻塞，并且该对象上的锁被释放。

	notify() 导致因调用该对象的wait()方法而阻塞的线程中随机选择一个接触阻塞。(但要等到获得锁后才真正被执行)

	① 调用notify()方法导致解除阻塞的线程因调用该对象的wait()方法而阻塞的线程中随机选取的，我们无法预料哪一个线程将被选择。

	② notifyAll()方法将把因调用该对象的wait()方法而阻塞的所有线程一次性全部解除阻塞。

* join()： 将几个并行线程的线程合并为一个单线程执行。

	当一个线程必须等待另一个线程执行完毕才能执行可使用join方法。

	join(): 等待线程终止

	join(ling mills): 等待线程终止的时间最长为mills 毫秒

	join(long mills, int nanos)






	