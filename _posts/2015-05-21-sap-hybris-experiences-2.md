---
layout: post
title: Hybris 经验总结——2
description: "Hybris经验总结-2"
modified: 2015-04-26
category: articles
tags: [Hybris]
comments: true
share: true
---

###hybris 学习总结2 

####Extension 内容

* 访问http://localhost:9001/platform/extensions 可以查看到当前所有的加载的Extension。
* 编译目录，在eclipse中得源文件会编译到eclipsebin目录中去，在系统环境ant中的编译会到classes目录中去，这样的好处是两个环境互相不会影响。



#### Data Model 

* <extension>-items.xml 在每个扩展的这个文件里面可以配置数据模型，在Hybris里面我们把实体模型成为item，我们把item之间的关系定义为关系元素relation elements
* Hybris 系统会抽取所有Extension里面的*item.xml文件，并自动生成一系列的Java源文件来支持Hybris来访问这些实体。
* 由item系统自动生成的类文件基本上会进入以下三个目录中： 模型类——用于开发业务逻辑；WenService相关类——使用Restful URI来支持CRUD逻辑，当在item文件中配置了扩展平台webservice的时候才会生成；低级别的Jalo类。

item文件内容，添加一个item业务对象

	<?xml version="1.0" encoding="ISO-8859-1"?>
	<!--
	 [y] hybris Platform
	 
	 Copyright (c) 2000-2013 hybris AG
	 All rights reserved.
	 
	 This software is the confidential and proprietary information of hybris
	 ("Confidential Information"). You shall not disclose such Confidential
	 Information and shall use it only in accordance with the terms of the
	 license agreement you entered into with hybris.
	 
	-->
	 
	<items   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	            xsi:noNamespaceSchemaLocation="items.xsd">
	    
	    <itemtypes>
	        <itemtype code="Stadium" generate="true" autocreate="true">
	            <deployment table="CuppyTrailStadium" typecode="10123" />
	            <attributes>
	                <attribute qualifier="code" type="java.lang.String" >
	                    <persistence type="property"/>
	                    <modifiers optional="false" unique="true"/>
	                </attribute>
	                <attribute qualifier="capacity" type="java.lang.Integer">
	                    <description>Capacity</description>
	                    <persistence type="property" />
	                </attribute>
	            </attributes>
	        </itemtype>
	    </itemtypes>
	</items>


添加对象间关系

	<relations>
	    <relation code="StadiumMatchRelation" localized="false" generate="true" autocreate="true">
	       <sourceElement type="Stadium" qualifier="stadium" cardinality="one" />
	       <targetElement type="Match" qualifier="matches" cardinality="many"/>
	    </relation>
	</relations> 
