---
layout: post
title: Mac OS X Yosemite 10.10 安装VMware Fusion及解决windows 7固定分辨率问题
description: "Mac OS X Yosemite 安装VMware Fusion,解决5.0版本在Yosemite上由于版本过低无法运行的问题"
modified: 2014-12-08
category: articles
tags: [mac,yosemite,vmware fusion]
comments: true
share: true
---

####Mac OS X Yosemite 10.10 安装VMware Fusion

由于苹果新政策允许部分10.8的电脑后续可以自动更新操作系统，所以也是跟着从10.9升级到现在的10.10版本了。更新后发现原来的Vmware Fusion 5.01版本在新的Mac OS X Yosemite 10.10运行时报错：应用程序“VMware Fusion”的这个版本不能与此版本的 OS X 配合使用。

原来Vmware 5.1.0版本不能支持新版本的IOS Yosemite系统，启动的时候就会报这个错误。

解决方法就是重新下载最新的Vmware Fusion 7.1.0，通过这中方法解决。具体的下载地址和安装方法可以去网上搜索。


####windows虚拟机自动改变分辨率解决

MAC系统下在Vmware上安装windows 7，不论怎么设置，总是自动的修改屏幕分辨率到最优分辨率，但是MAC的最优分辨率太高，在windows下面完全没有办法看。解决起来也比较简单

1. vi 到 `~/Library/Preferences/VMware\ Fusion/preferences` 文件
2. 在文件末尾添加： 

末尾添加两行：

	pref.autoFitGuestToWindow = "FALSE"
	pref.autoFitFullScreen = "stretchGuestToHost"

其中第一行表示关闭自动适配客户端分辨率，第二行表示当全屏切换的时候关闭自动适配分辨率。
3.关闭Vmware，重新启动Vmware和虚拟机即可。

具体的可以参考这个帖子：[https://communities.vmware.com/thread/456215](https://communities.vmware.com/thread/456215)
