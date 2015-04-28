---
layout: post
title: RDS Xen 远程桌面License区别
description: "RDS Xen 远程桌面License区别"
modified: 2015-03-09
category: articles
tags: [RDS,Xen]
comments: true
share: true
---

##RDS Xen 远程桌面License区别

###RDS：

####有两种类型的远程桌面服务客户端访问许可证 (RDS CAL)：

RDS－每设备 CAL和 RDS－每用户 CAL

每设备和每用户的区别：

如果使用每设备授权模式，并且客户端计算机或设备初次连接到RD 会话主机服务器，默认情况下，向该客户端计算机或设备颁发一个临时许可证。在客户端计算机或设备第二次连接到RD 会话主机服务器时，如果许可证服务器已激活，并且有足够的 RDS－每设备 CAL 可用，许可证服务器将向该客户端计算机或设备颁发永久 RDS－每设备 CAL。

RDS－每用户 CAL 向一个用户授予通过任意数量的客户端计算机或设备访问RD 会话主机服务器的权限。RD 授权不强制使用 RDS－每用户 CAL。因此，无论许可证服务器上安装了多少个 RDS－每用户 CAL，都可以建立客户端连接。这样，管理员不必考虑 Microsoft 软件许可条款对每个用户都需要有效的 RDS－每用户 CAL 的要求。使用每用户授权模式时，如果不是每个用户都有 RDS－每用户 CAL，将会违反许可条款。

####Microsoft RDS CALs 卖的地方：
[Amazon售卖Microsoft Windows Server 2008 Remote Desktop Services User 5 CAL Licenses](http://www.amazon.com/Microsoft-Windows-Desktop-Services-Licenses/dp/B0038TFAZ0/ref=sr_1_1?ie=UTF8&qid=1425878641&sr=8-1&keywords=rds+cal+2008)

This is for Remote Desktop licenses. If you have users that need to remote into the server to do their work then you will need one license per user. If you only have 2 users then that is already included. This license is installed on the server and will be tracked by the server. If you have 5 users that need to use the server at one time then you will need to buy 1 pack of 5 licenses. If you have 10 people that will use the server but only 5 people will need to use the server at any given time then you will most likely want to use the "Per User" not the "Per Seat" option. If you use the "Per Seat" then you would have to buy 2 packs of 5 licenses to allow the 10 users to remote in. I would not confuse this license with the "Per Device" license required to allow other computers to network to this server. If that is what you want then this is the wrong product. Another thing to be aware is this will work on other servers that are not HP.

RDS的每客户License是根据客户的峰值来卖的，如果是5个用户的License，那么保证同时只能有5个用户在线。

5客户  921美元。


[ebay售卖链接](http://www.ebay.com/sch/i.html?_nkw=rds+cal)
可支持银联付款  
5用户   3229.88    $749 per year
10用户  8134.70    $1,679.86 per year
25用户  15083.70   $1599 per year
75用户  34446.04   $5500 per year 
100用户 40708.96   $6,500.00 per year

2012约  $118 one concurrent user per year

###Citrix Xen 

5用户   945 美元  

多用户的待询问

Citrix Xen 包含三个版本  Citrix XenDesktopTM 7.6, Platinum Edition、Citrix XenDesktopTM 7.6, Enterprise Edition、Citrix XenDesktopTM 7.6, VDI Edition三个版本，SAP 要求使用白金版本。

> Citrix XenApp product utilizes a concurrent licensing model. With the XenApp concurrent model, users are anonymous and a license is consumed by each concurrent user to one or more apps and/or XenApp published desktops. A licensed concurrent user is an anonymous floating user only licensed for the period during which access to the Citrix environment is required. Once access is terminated by the anonymoususer, the license is immediately returned to the license pool and available for another anonymous user to consume.

Citrix Xen 提供并发License模式。和微软RDS一样是采用最大并发限制进行license出售。


在网上查到一个2013年的报价
XenApp 白金版   Renewal SRP  $75 per CCU       Recovery SRP  $ 335 per CCU 

Citrix支持两种方式报价，按年计算和终身计算方式，终身方式约为按年方式的4倍。

