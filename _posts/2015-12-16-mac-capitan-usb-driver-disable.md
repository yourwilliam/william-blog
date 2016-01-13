---
layout: post
title: Mac升级Capitan之后USB网卡无法使用
description: "Mac升级Capitan之后，小米的USB网卡无法使用"
modified: 2015-12-16
category: articles
tags: [mac]
comments: true
share: true
---

##Mac升级Capitan之后USB网卡无法使用

公司的无线网总是有这样那样的问题导致访问过慢，所以买了一个小米的USB网卡，一开始使用还是挺不错的，网卡也不会造成系统的卡顿。但是自从升级EI Capitan之后各种问题就接踵而来了。

第一次的时候发现这个问题是因为升级之后就立刻不能用了。 简单点说原因就是El Capitan加强了安全机制，不再加载未正确签名的驱动。

解决办法： 安装最新的驱动程序。驱动可以到网卡的公司网站，也可以到网卡用的芯片的公司网站找，例如我买的小米USB外接百兆网卡用的是Realtek公司的RTL8152B，所以去Realtek的网站下了最新适用Mac的驱动就搞定了，地址如下：http://www.realtek.com/DOWNLOADS/downloadsView.aspx?Langid=1&PNid=14&PFid=55&Level=5&Conn=4&DownTypeID=3&GetDown=false

如果找不到最新的驱动，还有一个办法是关闭最新的安全机制，然后安装旧的驱动，但是这个方法有一定的风险，这边不详细说了，有需要的可以看http://inkandfeet.com/how-to-use ... -os-1011-el-capitan


###再次升级后又出现问题

今天系统再次升级后又出现新的问题，当待机一段时间之后，会出现重启的时候没有加载网卡的问题。但是重启之后又好了，正在重新定位问题中。

###驱动卸载

实在是不想被USB口的网卡折腾来折腾去了，放假去香港买了一个Thunderbolt口的网卡，回来后发现无法识别。 查了一圈原因，后来在系统PCI驱动里面定位到驱动没有加载造成的。网上搜了一圈没有相关的原因发生。想了想大概是之前装的驱动覆盖了mac自带的驱动。去之前找的安装包里面一看果真有卸载的命令行，执行后问题解决了。以后再也不需要纠结这个网卡的问题了。