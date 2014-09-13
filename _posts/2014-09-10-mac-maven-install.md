---
layout: post
title: mac下安装maven环境
description: "mac下折腾maven及eclipse以及其中的相关配置"
modified: 2014-09-10
category: articles
tags: [java,maven]
comments: true
share: true
---

### mac下maven的安装

#####1. 下载maven组件包
访问[maven官方下载链接](http://maven.apache.org/download.cgi)，下载apache-maven-3.2.3-bin.tar.gz linux软件版本包。

#####2. 安装maven组件

将apache-maven-3.2.3-bin.tar.gz 解压到需要的文件夹
	
	mv apache-maven-3.2.3-bin.tar /Users/valentine/workspace/tools/
	tar -zxvf apache-maven-3.2.3-bin.tar

修改首启动文件，让环境变量启动时加载

	vi ~/.bash_profile
	
	MAVEN_HOME=/Users/valentine/workspace/tools/apache-maven-3.2.3
	PATH=$PATH:$MAVEN_HOME/bin
	export MAVEN_HOME
	export PATH

将上述设置好之后注销当前用户，重新登录后生效。
执行`mvn -version`之后可以测试是否安装成功

#####3. 配置setting.conf文件
修改setting.conf文件之后在eclipse中加入即可。


#### maven一些使用说明
maven有时候同步失败的话，重启一下eclipse然后再重新下载，可以继续同步中央仓库，不然一直开着是不能同步的。
有时候同步不了的时候将repo文件夹中的文件删掉之后会重新进行同步。









