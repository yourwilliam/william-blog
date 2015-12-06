---
layout: post
title: linux radhat6.6tcpdump使用指南
description: "linux radhat6.6tcpdump使用指南"
modified: 2015-12-01
category: articles
tags: [linux,tcpdump]
comments: true
share: true
---

##linux radhat6.6tcpdump使用指南

		#截取本端口8080，源地址为192.168.11.12的所有包，截取的所有包保存到catch2.cpp
		tcpdump -i eth2 -nn port 8080 and src host 192.168.11.12 -w catch2.cap


