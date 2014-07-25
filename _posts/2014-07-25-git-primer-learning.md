---
layout: post
title: github 使用入门 
description: "构建自己的git时做的一些记录"
modified: 2014-07-25
category: articles
tags: [github,git,入门]
comments: true
share: true
---

# github git入门

### github
几年前就有关注github上一些开源的项目了，但是一直没有心情闲下来去研究一下git。现在git这么火了，前面偶遇jekyll，也顺便的把github初略的入门一下。

### mac安装git
git的一大好处就是可以使用命令行。在mac上安装git命令也非常简单，直接输入git后提示安装xcode之后就可以使用了。但是网上传言这种方法的git版本可能有些问题，这个现在暂时还用的不错，后面有空来具体研究研究这种安装方法的问题。

### 创建一个自己的repository
1. 使用页面上的创建创建一个public的 repository
2. 使用名称yourwilliam/valen_jekyll
3. 在本地初始化一个git
4. git init 初始化一个git
 创建一个git分支 `git checkout －－orphan gh-pages`
 使用gh-pages分支是为了使用jekyll的时候使用的，如果需要页面访问就必须使用这个分支。
5. 提交git

提交git部分代码

		git add .
		git commit -m “first post”
		git remote add origin https://github.com/yourwilliam/valen_jekyll.git
		git push origin gh-pages

### 提交代码的步骤
1. `git status` 查看修改状态
2. `git pull origin gh-pages` 同步库上的内容
3. `git status`查看状态
4. `git add .` 添加当前未提交内容到本地git仓库
5. `git commit -m "comments"` 进行日志提交
6. `git push origin gh-pages`将当前本地仓库内容同步到远程仓库

###修改提交人

之前提交的时候会报提交人错误
		
		[gh-pages f9ef6c6] delete the model md, and commit the first blog
		 Committer: william valentine <valentine@williamtekiMacBook-Pro.local>
		Your name and email address were configured automatically based
		on your username and hostname. Please check that they are accurate.
		You can suppress this message by setting them explicitly:
git上修改提交人：

	
		git config --global user.name "Your Name"
    	git config --global user.email you@example.com
    	git commit --amend --reset-author

###删除git上的文件

在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件test.txt到Git并且提交：

		$ git add test.txt
		$ git commit -m "add test.txt"
		[master 94cdc44] add test.txt 
		1 file changed, 1 insertion(+) 
		create mode 100644 test.txt
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

		$ rm test.txt

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
		
		$ git status
		# On branch master# Changes not staged for commit:
		#   (use "git add/rm <file>..." to update what will be committed)
		#   (use "git checkout -- <file>..." to discard changes in working directory)
		#
		#       deleted:    test.txt
		#
		no changes added to commit (use "git add" and/or "git commit -a")

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且commit：
		
		$ git rm test.txtrm 'test.txt'
		$ git commit -m "remove test.txt"
		[master d17efd8] remove test.txt 
		1 file changed, 1 deletion(-) 
		delete mode 100644 test.txt

现在，文件就从版本库中被删除了。 








