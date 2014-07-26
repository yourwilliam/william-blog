---
layout: post
title: jekyll 使用心得
description: "使用jekyll时候的一些心得分享"
modified: 2014-07-25
category: articles
tags: [jekyll,markdown,心得,分享,详解]
comments: true
share: true
---

### jekyll心得分享
1. 在序列换行之后，输入代码之前，需要添加一行文字描述，然后使用两次tab才可以形成代码，否则代码块会失效。
2. 在序列换行之后，输入``单行代码之前，需要有文字空格，否则无法生效。
3. jekyll 添加google站长管理工具，
	google端数据为 `<meta name="google-site-verification" content="3vEn1_fXhoqMpIhn7tfcWHXJsBbcXAMVbhpkYAF4IkU">`
	在_config.yml中配置相应的站长检测配置`google_verify:  3vEn1_fXhoqMpIhn7tfcWHXJsBbcXAMVbhpkYAF4IkU`
4. 如果对_config.yml配置有修改，push到github之后，最好关注一下邮件，如果github编译失败，会发送邮件到本地邮箱。


————————未完待续。