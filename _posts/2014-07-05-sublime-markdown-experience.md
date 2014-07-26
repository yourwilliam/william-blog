---
layout: post
title: sublime添加markdown语法插件
description: "sublime添加markdown语法插件"
modified: 2014-07-05
category: articles
tags: [sublime,markdown,语法,插件]
comments: true
share: true
---

#sublime添加markdown语法插件

用sublime完成markdown写博客，让写博客成为了一件很有乐趣的事情，再也不需要那些繁杂的HTML，也不需要去修改那些编译器生成的神奇的HTLM标签了。

### 安装markdown preview插件
markdown preview适合于本地修改完成后在本地预览md最终的样式，适用于本地构建调试md文档。

使用sublime text的Package install搜索markdown preview。这个包提供在python markdown和github markdown两种语法，支持在浏览器中对markdown进行预览，也支持将markdown直接转化为html。

安装方法：
`sublime->preference->package control->install package->markdown preview`

使用方法：
`ctrl+shift+p -> markdown preview : preview in browser`


### 为markdown在sublime中添加高亮语法插件

默认的sublime是不支持markdown语法高亮的，这样写起来总有点觉得怪怪的。sublime也有很多相应的插件来开发实现sublime的编辑语法高亮。试了几个之后，觉得最好用的还是**Monokai extended** 加 **Markdown Extended**的方式。

##### MarkdownEdit
这个应该是网上介绍的最多的关于sublime下编辑markdown的插件了。但是使用之后觉得相应的观感不是很好。他用一种类似直接开发HTML页面的样式来表现相应的MD文件。变更了sublime的原始主题样式，需要诸多修改才能把样式改为原生的。当前还是喜欢sublime的原生样式来编辑，给coding一种很亲近的感觉。
要安装直接在package install里搜索就可以了，这里不过多的赘述这种方式。

#### **Monokai extended**
Monokai extended这个color scheme支持Markdown语法高亮。
同样也是使用sublime text的Package install搜索Monokai Extended，安装之后通过Preferences>Color Scheme>Monokai Extended启用。


#### **Markdown Extender**

Monokai extended对于代码的处理使用的是markdown的默认高亮，即缩进的内容处理为代码，但是代码部分则是没有针对不同的语言处理语法高亮的。

Markdown Extended，也是通过sublime text的Package install搜索Markdown Extended进行安装。安装后，打开一个markdown文件在右下角的语言栏选择Markdown Extended激活这种语言高亮，也可以在ctrl + shift + p启用set syntax:markdown extended。
将Markdown Extended作为markdown文件的默认语言。通过导航栏，View -> Syntax -> Open all with current extension as... -> Markdown Extended。


使用**Monokai extended** 加 **Markdown Extended** 之后就是原生sublime样式加上语法高亮了。


