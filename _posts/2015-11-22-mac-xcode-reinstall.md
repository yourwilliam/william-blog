---
layout: post
title: mac升级EI Capitan后Xcode被卸载
description: "mac升级EI Capitan后Xcode被卸载，需要重装Xcode"
modified: 2015-11-22
category: articles
tags: [mac,Xcode]
comments: true
share: true
---

##mac升级EI Capitan后Xcode被卸载 

####问题出现情况
mac升级了之后，今天使用git的时候发现问题了，相关报错
>xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun

####问题原因
Mac刚刚升级了EI Capitan之后，老版本的Xcode会自动卸载，这时候会发现以前的git不能使用，需要重新安装Xcode。

####解决办法
重新安装Xcode： 

		xcode-select --install

完成后再运行git，发现问题得以解决。


