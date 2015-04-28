---
layout: post
title: Windows 下tomcat进程守护脚本 bat
description: "Windows 下tomcat进程守护脚本 bat"
modified: 2015-04-26
category: articles
tags: [Windows,tomcat]
comments: true
share: true
---


###Windows 下tomcat进程守护脚本

tomcat一般在生产环境下会采用heartbeat等主备方式进行部署，或者干脆使用集群方式部署。但是现在这个需求由于需要使用某个特定主机上面的特定包，所以也不能做主备，只有采用单机部署，这时候就需要一些自动拉起等方法来保证tomcat的灵活性。

自己写了一个脚本来定时轮询tomcat进行，如果进行挂掉之后会保证自动拉起，由于当前环境只能使用Windows，所以只能在Windows下写bat来执行。

脚本如下

		@echo off	

		set _task=java.exe
		set _svr=E:\developsoftware\apache-tomcat-7.0.57\bin\startup.bat
		set _des=start.bat
		set CATALINA_HOME=E:\developsoftware\apache-tomcat-7.0.57		

		:checkstart
		for /f "tokens=5" %%n in ('qprocess.exe ^| find "%_task%" ') do (
		 if %%n==%_task% (goto checkag) else goto startsvr
		)	
		
			

		:startsvr
		echo %time% 
		echo ********程序开始启动********
		echo 程序重新启动于 %time% ,请检查系统日志 >> restart_service.txt		

		echo set CATALINA_HOME=E:\developsoftware\apache-tomcat-7.0.57> %_des%
		echo set CURRENT_DIR=E:\developsoftware\apache-tomcat-7.0.57\bin>> %_des%		

		echo start %_svr% >> %_des%
		echo pause>> %_des%
		start %_des%
		set/p=.<nul
		for /L %%i in (1 1 10) do set /p a=.<nul&ping.exe /n 2 127.0.0.1>nul
		echo .
		echo Wscript.Sleep WScript.Arguments(0) >%tmp%\delay.vbs 
		cscript //b //nologo %tmp%\delay.vbs 10000 
		del %_des% /Q
		echo ********程序启动完成********
		goto checkstart	
			

		:checkag
		echo %time% 程序运行正常,10秒后继续检查.. 
		echo Wscript.Sleep WScript.Arguments(0) >%tmp%\delay.vbs 
		cscript //b //nologo %tmp%\delay.vbs 10000 
		goto checkstart

	