---
layout: post
title: mac下markdown贴图流程优化安装
description: "markdown贴图流程"
modified: 2015-11-22
category: articles
tags: [mac,markdown，贴图]
comments: true
share: true
---

##markdown贴图流程优化安装

markdown对于文档的制作非常方便，更利于程序员去书写自己的文档，但是一直有感于markdown下面的图片的操作实在是不方便，今天偶然找到了优化的流程，把详细的安装使用过程整理出来。
也感谢tiann的工具分享。 [github地址](https://github.com/tiann/markdown-img-upload)

###插件安装

#####1. 下载安装插件
直接在github上面下载全文件即可。
其中info.plist 定义为markdown的相关配置文件
双击markdown img.alfredworkflow 就可以在alfred里面添加一个workflow，并且把相应的workflow中的定义
如下
<figure>
<img src="7xocbv.com1.z0.glb.clouddn.com/test/1448170927829.png" width="845"/>
<figcaption>创建工作流</figcaption>
</figure>
并且在Hotkey里面设置相应的markdown热键

#####2. 设置
在里面有mdimgsetup的一个Inputs 的Keyword，用于设置相应的初始化参数。在Alfred里面，输入mdimgsetup，进度相应设置页面
<figure>
<img src="7xocbv.com1.z0.glb.clouddn.com/test/1448171644750.png" width="604"/>
<figcaption>启动配置</figcaption>
</figure>
进入后输入

		; 详细设置见 https://github.com/tiann/markdown-img-upload
		[qiniu]
		ak=在七牛的ak
		sk=七牛的sk
		url=七牛注册的url
		bucket=william
		prefix=test

设置这个之后等于把自己在七牛注册的空间配置成功，后续会自己上传到在七牛上面注册的空间中了。

#####3. 使用
使用mac下的截屏快捷键ctrl+cmd+shift+4，进行相应屏幕截屏，或者直接复制图片。复制完成后，在文本输入处，或者markdown处是使用刚才设置的快捷键就可以看到相应的img口的插入。

