---
layout: post
title: 开源选型指南
description: "开源软件商用选择指南"
modified: 2012-09-13
category: articles
tags: [java,j2se]
comments: true
share: true
---

###开源选型指南

之前在公司的时候有左一些开源检测相关的东西，也趁着机会整理了一下一些简单的开源选型意见。

在公司的开源检测平台大方向主要分成代码检测和二进制包检测。代码检测主要检测代码中使用的开源代码以及开源包(如Java中得jar包引用)。二进制检测主要进行一些发布包中得开源软件的检测，比如jdk编译环境等得检车以及开源容器的检测。

使用相应的扫描软件对将扫描出来的开源风险项进行一个开源说明即可。也把相应的开源选型进行了总结，如下如所示：

<figure>
	<img src="{{ site.url }}/images/blog2014/OpenSourceLicense.png">
    <figcaption>开源选型指南，开源库选择指南</figcaption>
</figure>