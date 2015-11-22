---
layout: post
title: Hybris 经验总结——Impex格式
description: "Hybris Impex格式"
modified: 2015-05-26
category: articles
tags: [Hybris,Impex]
comments: true
share: true
---

####Impex格式

	mode type[modifier=value];attribute[modifier=value];attribute[modifier=value];attribute[modifier=value][;...];attribute[modifier=value]

* mode ：提供四种操作 insert、update、insert_update、remove等操作
* type ：定义处理的item类型，catagory,product,media,type等等
* attribute ：映射到对象的column属性
* Modifier :

####Header Mode 

#####Insert : 在Hybris中创建一个item，Impex默认不检查是否存在相同属性的item

#####Update : 在hybris中通过一个unique的属性，选择一个存在的item，将属性值设置到相应的值上。

如果不存在对应的item，则当前value行不能处理

属性的默认值为null

例子：

	UPDATE User;uid[unique=true];name;customerID;
	;ahertz;Anja Hertz;;
	;ahertz;;K2006-C0005;

可以使用下面三种方法之一来更新：

Specify all values in one single line:

	UPDATE User;uid[unique=true];name;customerID;
	;ahertz;Anja Hertz;K2006-C0005;

Re-specify all values that have been set in a prior line:

	UPDATE User;uid[unique=true];name;customerID;
	;ahertz;Anja Hertz;;
	;ahertz;Anja Hertz;K2006-C0005;

Use individual headers:
	
	UPDATE User;uid[unique=true];name;
	;ahertz;Anja Hertz;
	 
	UPDATE User;uid[unique=true];custmerID;
	;ahertz;K2006-C0005;


#####INSERT_UPDATE : 将insert和Update方式合并，

#####REMOVE : hybris会尝试寻找到正确的item，如果一个item存在，它将被删除。

####Header Type







