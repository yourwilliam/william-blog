---
layout: post
title: 博客系统Hexo介绍
description: "Node.js框架下Hexo博客系统的介绍"
modified: 2015-11-23
category: articles
tags: [开源工具]
comments: true
share: true
---

###博客系统Hexo介绍

[Hexo git 地址](https://github.com/hexojs/hexo)

做了这么久的云，自己这个博客是直接放在github上面的，但是有时候国内访问比较慢之类的问题总是造成响应不太好，今天有时间看到Hexo系统。这里来大概分析一下他的好处。

Hexo是直接用Node.js开发的，它天生的优势就是完全静态化。传统的我们做个人博客的做法是先要去AWS或者国内阿里云之类的租用一个云主机，然后绑上公网IP，最后安装wordpress等博客系统。整完一套最低配置一年也需要1000大洋左右。

而Hexo免费静态博客程序，Hexo基于Node.js，出自台湾一博主，Hexo生成的静态网站可以放在任意空间上，例如常见的PHP、ASP空间、FTP服务器、百度BAE、新浪SAE等空间，可以说只要可以用Web访问的就可以搭建起Hexo博客。

而且Hexo可以直接通过架接在github上面提供访问，提供更好的体验。

详细的可以参考这篇博文 [如何搭建一个独立博客——简明Github Pages与Hexo教程](http://blog.csdn.net/poem_of_sunshine/article/details/29369785)

###题后

自己的使用的也是一个github上面的框架提供的类似服务。

Hexo的主要优势还是可选的模板比较多，自己可以配置自己想要的模板样式。
