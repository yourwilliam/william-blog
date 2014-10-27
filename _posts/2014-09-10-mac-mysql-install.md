---
layout: post
title: mac下安装mysql
description: "mac下折腾mysql"
modified: 2014-09-10
category: articles
tags: [java,maven]
comments: true
share: true
---

#### mac下安装mysql主程序

访问[mysql官方下载页](http://www.mysql.com/downloads/), 选择社区版本 MySQL Community Edition ， 在MySQL Community Downloads 界面下载MySQL Community Server 。 进入之后会根据服务器选择相应的mysql版本，在Mac OS上的MySQL的版本很多，选择64位10.8版本的，然后就是文件的后缀名有.tar.gz的和.dmg的，这里我选择的是.dmg的。点击右侧的download进行下载。

安装mysql-5.5.16-osx10.6-x86_64.pkg 主程序包。


##### 安装workbench
下载mac workbench安装包，用同样的方法安装到mac上即可。

workbench提供MySQL设计器，以及MySQL相关的方便的可视化操作方法。


##### MySQL服务在mac下得操作方法

启动MySQL服务`sudo /Library/StartupItems/MySQLCOM/MySQLCOM start`
 
停止MySQL服务`sudo /Library/StartupItems/MySQLCOM/MySQLCOM stop`
 
重启MySQL服务`sudo /Library/StartupItems/MySQLCOM/MySQLCOM restart`


##### 管理数据库的访问密码

MySQL的默认账号密码是root/root，正常情况下我们如果单纯的只是使用MySQL Workbench来管理数据库的这个账号是可以的，但是当我们在编程代码中通过jdbc来访问MySQL时我们就会发现使用这个账号是不行，无法访问，因为MySQL需要我们更改密码，也就是说root这个是个默认的密码也就是弱密码，需要我们修改之后才能在代码中使用。因此我们需要来管理数据库的访问密码。

新建一个Server Instance

在“Server Administration”模块下有个“New Server Instance”点击之后会弹出一个“Create New Server Instance Profile”的对话框，跟着对话框的一步一步走就可以完成，一般本地的数据库直接跟着默认设置就ok。完成之后我们就能够在Workbench的主界面最右边看到刚才建立的instance。

##### 使用命令登陆MySQL

使用workbench可以用命令行操作，但是怎么用都觉得不太习惯，还是命令行的方式来的比较快。整合了一下相应的mac下命令行使用方法。

在mac下安装之后，需要转换相关命令行
	
	alias mysql=/usr/local/mysql/bin/mysql
	aliasmysqladmin=/usr/local/mysql/bin/mysqladmin

给root创建密码：

	/usr/local/mysql/bin/mysqladmin -u root password root
 
使用终端来打开或关闭mysql:

    sudo /Library/StartupItems/MySQLCOM/MYSQLCOM [start | stop | restart]

进入数据库：
	
	mysql -u root -p

随后输入密码:root

将命令输入到启动项中得方法：
在终端输入 : 
	
	cd ~
	vim ./bash_profile

这个文件如果配置过android开发环境是修改过的.我们添加2行

	alias mysql=/usr/local/mysql/bin/mysql
	alias mysqladmin=/usr/local/mysql/bin/mysqladmin

保存退出,重启终端或者开新窗口即可

到这里可以参考一下基本命令行操作：


