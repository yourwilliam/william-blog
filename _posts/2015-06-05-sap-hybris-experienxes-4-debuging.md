---
layout: post
title: Hybris 经验总结——Debug模式
description: "Hybris Debug模式"
modified: 2015-06-04
category: articles
tags: [Hybris,debug]
comments: true
share: true
---

###Hybris Debug 方法

####Hybris Debug配置文件

对于Hybris的启动实际上是调用 /hybris-hankport/hybris/bin/platform 这个目录下面的hybrisserver.sh文件来启动执行的，vi这个执行文件可以看到

		case $MODE in
			"minimal" | "-m" )
				export WRAPPER_CONF="$CATALINA_BASE/conf/wrapper-minimal.conf"
				COMMAND="./catalina.sh run"
				;;
			"debug" | "-d" )
				export WRAPPER_CONF="$CATALINA_BASE/conf/wrapper-debug.conf"
				COMMAND="./catalina.sh run"
				;;
			"jprofiler" | "-j" )
				export WRAPPER_CONF="$CATALINA_BASE/conf/wrapper-jprofiler.conf"
				COMMAND="./catalina.sh run"
				;;
		    "version" | "-v" )
				COMMAND="java -cp ../lib/catalina.jar org.apache.catalina.util.ServerInfo"
				;;
			* )
				export WRAPPER_CONF="$CATALINA_BASE/conf/wrapper.conf"
				COMMAND="./catalina.sh ${MODE}"
				;;
		esac

所以对于Hybris服务来说，有几种启动方式，比如minimal模式、debug模式、jprofiler模式和默认模式。
可以通过下面方法启动

		sh hybrisserver.sh 
		sh hybrisserver.sh debug
		sh hybrisserver.sh jprofiler

可以看到启动不同的模式的时候实际就是加载了不同的wrapper容器而已。

####wrapper-debug容器

在目录/hybris-hankport/hybris/bin/platform/tomcat/conf 中得wrapper-debug.conf 文件中可以看到相应的Debug方式的配置

如下的这一段可以看到相应的Hybris的Debug启动参数

		wrapper.java.additional.21=-Xdebug
		wrapper.java.additional.22=-Xnoagent
		wrapper.java.additional.23=-Xrunjdwp:transport=dt_socket,server=y,address=8000,suspend=n
		wrapper.java.additional.24=-Ddeployed.server.type=tomcat 		

也可以修改这个参数，从这里可以看到相应的默认端口是8000端口

#### eclipse配置远程Debug

在Debug configuration中配置 Remote Java Application ， 添加一个，提交相应的工程即可进行远程调试。


