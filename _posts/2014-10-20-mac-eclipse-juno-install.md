---
layout: post
title: mac环境下安装eclipse以及相关插件
description: "mac环境下安装eclipse以及相关插件"
modified: 2014-10-20
category: articles
tags: [mac,eclipse,maven]
comments: true
share: true
---

####导读
在mac下得eclipse版本和windows下的eclipse还是有许多区别的。一些插件的依赖不一样。
之前使用kapler版本，下载maven插件之后，用起来总是会报各种奇奇怪怪的问题，今天有空下了一个juno版本，问题就解决了，也在这里记录一下相应的配置。

#### 下载安装eclipse

下载地址[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)

最好选择javaEE版本，否则建不了web工程，选择Eclipse IDE for Java EE Developers。

安装完成后本地解压就可以了

#### 安装maven插件

##### 安装maven插件

	Eclipse -> Help -> Install New Software
	Add a new software site:
	http://download.eclipse.org/technology/m2e/releases

通过这个链接安装的是maven eclipse插件的1.5版本。 可以展开版本选择需要的版本安装。
在mac下的eclipse直接通过这种方法安装，会报一个缺少slf4j依赖的错，用下面方法可以解决。

##### 安装slf4j-api 插件 

安装maven插件的时候会报一个依赖slf4j的依赖，下载方法为

	Eclipse -> Help -> Install New Software
	Add a new software site:
	Name: slf4j
	Url: http://www.fuin.org/p2-repository/
	Expand "Maven osgi-bundles" and select "slf4j-api"
	Click "Next" and follow the installation.

##### 直接安装maven 1.3 版本

如果安装1.5版本一直出问题，可以选择安装maven的1.3版本，在mac的eclipse下就补需要别的插件补丁了。

	Eclipse -> Help -> Install New Software
	Add a new software site:
	http://download.eclipse.org/technology/m2e/releases/1.3

##### 配置maven

具体配置方法[参考这里](http://pengjunjie.com/articles/jave-maven-nexus-experience/)





