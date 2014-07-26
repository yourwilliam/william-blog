---
layout: post
title: mac下Postgre.app 使用指南——安装和启动(1)
description: "mac下Postgre.app 使用指南——安装和启动(1)"
modified: 2013-12-13
category: articles
tags: [mac,postgre.app,install,安装,使用,指南]
comments: true
share: true
---

# mac下Postgre.app 使用指南——安装和启动(1)

Mac下面的Postgre.app 为mac提供了一个简单的PostgreSQL Server 方式，拷贝到Application之后双击便可以直接启动。无需复杂的安装和操作方式。

下载地址：[http://postgresapp.com/](http://postgresapp.com/)

文档地址：[http://postgresapp.com/documentation](http://postgresapp.com/documentation)

1. 添加Path系统环境变量

	1. 临时环境变量添加方法 `export PATH="/Applications/Postgres93.app/Contents/MacOS/bin:$PATH”`
这种方法可以把 临时变量添加到path中去，但是在重新启动终端后这种方式就不起作用了，因为这是临时的。
	2.查看path `echo $PATH`
	3.添加持久PATH方式 `vi /etc/paths`
     将`/Applications/Postgres93.app/Contents/MacOS/bin` 添加后保存
     如果不关闭终端，直接进行`echo $PATH` ，是不会出现新的。
     需要重新启动终端，才能看到。

2. 双击Application里面的Postgres93 ，启动postgreSQL进程
3. 进入pgsql console 。 `psql -h localhost`