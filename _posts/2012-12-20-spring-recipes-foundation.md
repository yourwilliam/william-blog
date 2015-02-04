---
layout: post
title: spring基础
description: "spring攻略——基础篇"
modified: 2012-12-05
category: articles
tags: [java,spring]
comments: true
share: true
---

###spring基础篇

#####面向接口编程： 设计良好的架构可保证系统重构的工作被封闭在重构的层内，绝对不影响其他层。

依赖注入： 在程序运行过程中，如果需要调用另外一个对象协助时，无须再代码中创建被调用者，而是依赖于外部的注入。
设值注入： 调用set方法进行注入
构造注入： 调用构造函数进行注入


#####实例化SpringIOC容器

Spring容器有两个接口： BeanFactory 和 ApplicationContext

这两个接口也被称为Spring 上下文

ApplicationContext又可分为：ClassPathXmlApplicationContext, FileSystemXmlApplicationContext, XmlWebApplicationContext, XmlPortletApplicationContext.


#####配置Spring Bean

* 可以通过XML文件、属性文件、注释甚至API来配置Spring IOC中的Bean。
* Spring允许一个或多个Bean配置文件来配置Bean。
* 每个bean都应该提供一个唯一的名称或者id，以及一个完全限定的类名，用于让Spring IOC容器对其进行实例化。
* 配置文件通过property方法来对应每个Set方法。
* 配置文件通过constructor-arg方法来对应构造函数。
* 

(2) BeanFactory
     BeanFactory负责配置、创建及管理bean，它有个子接口：ApplicationContext，因此也被称为Spring上下文。
     BeanFactory基本方法： 
          public boolean containsBean(string name)
          public Object getBean(String name)
          public Obejct getBean(String name, Class requiredType) 
          public Class getType(String name) 
     调用者只需使用getBean方法获得指定的bean的引用，无须关心bean的实例化过程。

     BeanFactory实现类，通常使用org.springframework.beans.factory.xml.XmlBeanFactory类。
但对于大部分J2EE应用而言，推荐使用ApplicationContext，其常用的实现类是org.springframework.context.support.FileSystem
XmlApplicationContext。

     在创建BeanFactory的时候，应该提供xml配置文件作为参数，XML配置文件通常使用Resource对象传入。
     对于大部分J2EE应用而言，可以在启动Web应用时自动加载ApplicationContext实例，接受Spring管理的Bean无须知道ApplicationContext的存在，也一样可以利用ApplicationContext的管理。

     如果应用里有多个属性配置文件，则应该采用BeanFactory的子接口ApplicationContext来创建BeanFactory的实例，ApplicationContext通常使用如下两个实现类。
• FileSystemXmlApplicationContext: 以指定路径的XML 配置文件创建ApplicationContext。
• ClassPathXmlApplicationContext: 以CLASSPATH路径下的XML配置文件创建ApplicationContext。


(3) Bean的基本定义
   <beans> 是spring配置的根元素， <bean>是其子元素
     在定义bean时，通常需要指定两个属性： 
          id  确定该bean所属的唯一值
          class 指定该bean的具体实现类，通常情况下，spring 会直接使用new关键字创建该bean的实例。

(4) bean的行为方式
     两种基本行为：
     singleton:    默认行为
     non-singleton 或 prototype:

     通常要求将Web 应用的控制器bean 配置成non-singleton行为。

(5) 创建bean实例
     创建bean通常有如下方法：
          调用构造器创建一个bean实例。
          BeanFactory调用某个类的静态工厂方法创建bean。
          BeanFactory调用实例工厂方法创建bean。

4. 依赖关系配置
(1) 


