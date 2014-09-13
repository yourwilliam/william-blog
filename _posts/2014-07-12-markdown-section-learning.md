---
layout: post
title: markdown段落语法
description: "markdown段落语法结构"
modified: 2014-07-12
category: articles
tags: [markdown,语法]
comments: true
share: true
---

####markdown段落语法

#####4种列表方式
无序列表使用星号、加号或是减号作为列表标记：

	*  Red
	*  Green
	*  Blue
等同于：

	+  Red
	+  Green
	+  Blue
也等同于：

	-  Red
	-  Green
	-  Blue
有序列表则使用数字接着一个英文句点：

	1.  Bird
	2.  McHale
	3.  Parish

在标记上使用的数字不会影响输出的HTML的结果。当前使用有序列表形成的HTML依然为<ol>和<li>标签。但是后续不清楚是否会对markdown语法进行有序支持。


#### 列表之间的空行会使用段落标记填充

	*  Bird	

	*  Magic
会被转换为：

	<ul>
	<li><p>Bird</p></li>
	<li><p>Magic</p></li>
	</ul>

#### 列表包含多个段落的处理方法

	*  This is a list item with two paragraphs.	

	    This is the second paragraph in the list item. You're
	only required to indent the first line. Lorem ipsum dolor
	sit amet, consectetuer adipiscing elit.	

	*  Another item in the same list.
如果要在列表项目内放进引用，那 > 就需要缩进：

	*  A list item with a blockquote:	

	    > This is a blockquote
	    > inside a list item.
如果要放代码区块的话，该区块就需要缩进两次，也就是 8 个空格或是 2 个制表符：

	*  一列表项包含一个列表区块：

        	<代码写在这>

####如果在行首出现数字＋点的情况，需要在点后加上反斜杠

1986\. What a great season.














