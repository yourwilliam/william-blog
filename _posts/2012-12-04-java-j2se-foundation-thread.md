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


#### 线程基本问题

* run() 的访问控制符必须是public，返回值必须是void run() 不带参数。
* Java是面向对象的程序设计语言，不同的对象的数据是不同的，r1,r2有各自的run方法，而synchronize使同一对象的多个线程，在某个时刻只有其中一个线程可以访问这个对象的synchronize数据

代码如下

	public class ThreadTest implements Runnable
	{
		public synchronized void run()
		{
			for(int i=0; i<10;i++)
			{
				System.out.print(" " + i);
			}
		}

		public static void main(String[] args)
		{
			Runnable r1 = new ThreadTest();
			Runnable r2 = new ThreadTest();
			Thread t1 = new Thread(r1);
			Thread t2 = new Thread(r2);
			t1.start();
			t2.start();
		}
	}

或者

	public static void main(String[] args)
	{
		Runnable r = new ThreadTest();
		Thread t1 = new Thread(r);
		Thread t2 = new Thread(r);
		t1.start();
		t2.start();
	}

* Thread 提供方法 `setProperity(Thread.MAX_PRIORITY)` 优先级并不代表一个执行完再执行下一个，而是优先开始
* 一个线程在sleep的时候，并不会释放对象的锁标识
* join()方法，能够使调用该方法的线程在此之前执行完毕
* sleep方法可以使低优先级的线程得到执行机会，当然也可以让同优先级和高优先级的线程有执行的机会，而yield()方法只能使同优先级的线程有执行的机会。
* wait()、notify()、notifyAll() 这三个方法用于协调多个线程对共享数据的存取，所以必须在synchronized语句块内使用者三个方法。
* wait() 可以释放锁标识
* 当线程执行了对一个特定对象的wait()调用时，那个线程被放到与那个对象相关的等待池中，此外，调用wait()的线程会自动释放对象的锁标识
* 对一个特定的对象执行notify()调用时，将从对象的等待池中移走任意一个线程，并放到锁标识等待池中，那里的线程一直等待，直到可以获得对象的锁标识。
* notifyAll()方法将从对象等待池中移走所有等待那个对象的线程并放到锁标识等待池中。
* 不管是否有线程等待都可以调用notify()





	