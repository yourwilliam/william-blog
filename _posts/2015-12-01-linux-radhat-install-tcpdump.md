---
layout: post
title: linux radhat6.6下手动安装tcpdump
description: "linux radhat6.6下手动安装tcpdump"
modified: 2015-12-01
category: articles
tags: [linux,tcpdump]
comments: true
share: true
---

##直接挂载镜像安装

#首先加载radhat安装光盘，然后mount到本地
		[root@localhost ~]# mount /dev/cdrom /mnt/cdradhat/
		#进入安装包目录
		[root@localhost Packages]# cd /mnt/cdradhat/Packages/
		#安装tcpdump
		[root@localhost Packages]# rpm -ivh tcpdump-4.0.0-3.20090921gitdf3cb4.2.el6.x86_64.rpm

##linux radhat6.6下手动安装tcpdump

###安装GCC

		#首先加载radhat安装光盘，然后mount到本地
		[root@localhost ~]# mount /dev/cdrom /mnt/cdradhat/
		#进入安装包目录
		[root@localhost Packages]# cd /mnt/cdradhat/Packages/
		#安装几个依赖包
		[root@localhost Packages]# rpm -ivh kernel-headers-2.6.32-504.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh glibc-headers-2.12-1.149.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh glibc-devel-2.12-1.149.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh ppl-0.10.2-11.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh cloog-ppl-0.15.7-1.2.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh mpfr-2.4.1-6.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh cpp-4.4.7-11.el6.x86_64.rpm
		[root@localhost Packages]# rpm -ivh gcc-4.4.7-11.el6.x86_64.rpm

表示GCC安装成功

（后续完善）

###安装libpcap

http://blog.csdn.net/rekken/article/details/7497600

###安装tcpdump
http://www.cnblogs.com/cipc/articles/2428282.html









