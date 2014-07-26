---
layout: post
title: mac iTerm2 使用指南 
description: "google上iTerm2的使用指南都总结的很少，这里花点时间把这个shell神器指南整理一下"
modified: 2014-07-26
category: articles
tags: [mac,software,iTerm2,指南]
comments: true
share: true
---

mac的终端一直在用，但是多屏的情况下总是各种不顺手，快捷键也是总是不方便切换。慢慢的维护这个iTerm2 指南把。

#### 窗口操作
* 新建窗口: `Command + N`
* 关闭窗口: `Shift + Command + W`
* 前一个窗口:  `` Command + ` ``
* 后一个窗口: `Command + ~`
* 进入窗口1，2，3...: ` Option + Command + 窗口编号`

#### 标签页操作

* 新建标签页:`Command + T`
* 关闭标签页:` Command + W`
* 前一个标签页:` Command + 左方向键，Shift + Command + [`
* 后一个标签页:` Command + 右方向键，Shitf + Command + ]`
* 进入标签页1，2，3...:` Command + 标签页编号`
* Expose 标签页:` Option + Command + E（将标签页打撒到全屏，并可以全局搜索所有的标签页）`

#### 面板操作

* 垂直分割: `Command + D`
* 水平分割: ` Shift + Command + D`
* 前一个面板: ` Command + [`
* 后一个面板: ` Command + ]`
* 切换到上/下/左/右面板: `Option + Command + 上下左右方向键`

#### 其他功能

* 支持自定义全局快捷键用于显示和隐藏iTerm2 `Preference -> Keys －> Show/hide iTerm2 with a system-wide hotkey 打上勾之后`
* 进入和退出全屏: `Command + Enter`
* 查看当前终端中光标的位置: `Command + /`
* 命令自动补全: `Command + ;（很少用这个，还是感觉Zsh的补全更好用）`
* 开启和关闭背景半透明: `Command + u`
* 清屏（重置当前终端）: `Command + r`


### 特色功能

####文本选取

文本选取有使用鼠标和不使用鼠标两种方式。

#####使用鼠标
默认情况下，选取的文字会自动复制到剪切板，可以使用以下方式进行文本选取：

* 常见的点击并拖拽方式
* 双击选取整个单词
* 三击选取整行
* 选取某一部分，按住Shift，再点击某处，可以选取整个矩形内的文本（类似Windows下按住Shift可以批量选取图标）
* 按住Command + Option，可以用鼠标画出一个矩形，用类似截图的方式选取文本
另外，还可以使用鼠标完成以下操作：
按住Command然后点击某个URL，会在浏览器中打开这个URL，点击某个文件夹，会在Finder里打开这个文件夹（再也不用open . 啦），点击某个文件名，会打开这个文件（文本文件支持MacVim，TextMate和BBEdit，如果后面跟随一个冒号和行号，文件会在行号处打开，其它格式的文件似乎不能调用默认程序打开）
选取文本之后，按住Command 同时拖动文本，可以将文本粘贴到目标位置（Drag and Drop）
鼠标中键粘贴（这个太感人了，一下子找回Linux的感觉了）

##### 不使用鼠标
(这种方式最多只能选取一行文本) 使用 Command + f，会呼出一个搜索框，可以在当前面板中进行搜索，输入想要选取的部分内容，输入过程中，按Tab可以将选取部分向右扩展，按Shift + Tab向左扩展，按回车转到下一个匹配位置。使用Tab或Shift+Tab扩展得到想要的内容之后，选取内容会自动复制到剪切板，再次按Command + f隐藏搜索框。

#### 位置书签

在当前会话中按`Command + Shift + m`可以保存当前位置，之后可以按`Command + Shift + j`跳回这个位置。

#### 粘贴历史

使用`Command + Shift + h` 可以呼出粘贴历史，支持模糊检索。还可以设置将粘贴历史保存在磁盘上`（Preferences -> General）`

粘贴历史

即时回放

使用`Command + Opt + b` 打开即时回放，按Esc退出。即时回放可以记录终端输出的状态，让你“穿越时间”查看终端内容。默认每个会话最多储存4MB的内容，可以在设置中更改`（Preferences -> Genernal -> Instant Replay）`。

即时回放

#### 窗口状态

通过 `Window -> Save Window Arrangement` 可以保存当前窗口状态的快照，包括打开的窗口，标签页和面板。通过 `Window -> Restore Window Arrangement` 还原。还可以在 `Preferences -> General -> Open saved window arrangement` 中设置在启动iTerm2时自动恢复窗口状态


### 终端颜色设置

##### 使用solarized配色方案
######下载solarized  
		
		$ git clone git://github.com/altercation/solarized.git
######Terminal
在 solarized/osx-terminal.app-colors-solarized 下双击 Solarized Dark ansi.terminal 和 Solarized Light ansi.terminal 就会自动导入两种配色方案 Dark 和 Light 到 Terminal.app 里。

######iTerm2
到 solarized/iterm2-colors-solarized 下双击 Solarized Dark.itermcolors 和 Solarized Light.itermcolors 两个文件就可以把配置文件导入到 iTerm 里。

###### vim
Vim 的配色最好和终端的配色保持一致，不然在 Terminal/iTerm2 里使用命令行 Vim 会很别扭：

		$ cd solarized
		$ cd vim-colors-solarized/colors
		$ mkdir -p ~/.vim/colors
		$ cp solarized.vim ~/.vim/colors/		

		$ vi ~/.vimrc
		syntax enable
		set background=dark
		colorscheme solarized  

###### ls

Mac OS X 是基于 FreeBSD 的，所以一些工具 ls, top 等都是 BSD 那一套，ls 不是 GNU ls，所以即使 Terminal/iTerm2 配置了颜色，但是在 Mac 上敲入 ls 命令也不会显示高亮，可以通过安装 coreutils 来解决（brew install coreutils），不过如果对 ls 颜色不挑剔的话有个简单办法就是在 .bash_profile 里输出 CLICOLOR=1：

		$ vi ~/.bash_profile
		export CLICOLOR=1





