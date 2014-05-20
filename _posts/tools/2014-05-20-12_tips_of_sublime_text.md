---
layout: post
title: "12种高效的Sublime Text技巧(译)"
tagline: "12种高效的Sublime Text技巧(译)"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。12种高效的Sublime Text技巧(译)."
category: Tools
tags: [Tools]
---
{% include JB/setup %}



你可能知道目前我们都是 Sublime Text 的粉丝的，虽然它看起来是一个非常简单的编辑器，但是它有很多隐藏的特性。探索它一段时间后,你也许会感到惊讶,你可以在SublimeText做很多很酷的东西。我们去深入了解了以下,以下是一些提示和技巧,我们认为你会喜欢它的。

让我们来提升你的 Sublime Text 编码经验吧。

> 推荐阅读: [Identify Code Error In Sublime Text With Sublime Linter](http://www.hongkiat.com/blog/identify-code-errors-sublime-linter/)

## 1.选择
作为一名Web开发者，我们将会频繁的编辑代码。下面是一些方便的快捷键让你更快的用 SublimeText 做一些不同类型的选择。

	Command + D：选择一个单词
    Command + L:选择一行
    Command + A：全选
    Ctrl + Command + M:选择任何块(这在处理CSS或JavaScript很有用）

此外, SublimeText 让我们可以一次选择多行，这可以极大地提高你的生产率。有几种方法来执行这一功能：

	Command:按住 Command 键然后点击你想要选择的行
    Command + Ctrl + G:选择一个代码，一行或一个词，然后可以选择到和这相同的块
    Command + D:用这个组合键可以快速选择下一个和这相同的代码，行或者词。

看看下面的多行选择是怎么做的。

![multi-line](/static/images/sublime-text/multi-line-selection.gif)

## 2.CSS排序
通常，我们并不会介意CSS属性值是否是排序好的，因为不管CSS在标签内的位置在哪里都将给我们在浏览器中给我们想要的内容。但是把它们放在一个特定的顺序将会使你的代码更有组织性。在 SublimeText 中，你可以选择你的CSS属性然后按F5进行按字母表顺序排序。

![multi-line](/static/images/sublime-text/sorting-css.gif)


## 3.命令面板
你可以用命令面板快速的做任何事，例如重命名一个文件，设置文件语法，和插入代码片段。按下 Command + Shift + P 就可以看到命令面板，然后你就可以执行你想执行的命令了。这里有几个例子。

**重命名文件**

![rename](/static/images/sublime-text/rename-file.jpg)

**设置语法**

![syntax](/static/images/sublime-text/syntax-html.jpg)

**插入代码**

![snippet](/static/images/sublime-text/insert-snippet.jpg)

## 4.切换Tabs和工程
我们可能在操作一个项目时同时打开了多个文件。在 Sublime Text 中，我们可以通过以下的快捷键来快速的切换这些文件(或者Tabs).

	Command + T:列出当前已经打开的文件，选择切换到其中一个
    Command + Shfit + ]:进入下一个tab
    Command + Shift +［:进入上一个tab
    Command + Ctrl + P:侧边栏中多个项目直接切换

## 5.夸文件编辑
当我们同时处理多个文件时，这个功能是非常有用的，例如，我们有一些非常相似的代码块分布在项目的不同文件中。为了高效的改变这些代码，你可以：

1. 按 Command + Shift + F ,输入你想要改变的内容到输入框。

	提示：按 Command +Ｅ　可以快速的将已选择的代码放入输入框
2. 在输入框中指定文件名或添加<打开文件>,这样只会影响当前打开的文件。

3. 输入单词或代码块到替换框中，然后点击替换按钮

	![](/static/images/sublime-text/find-search-fields.jpg)

## 6.文件跳转
我发现在编辑CSS代码时这个功能真的非常有用。按 Command + R，会出现一个对话框与一个CSS选择器的文档列表,你可以从下面的截图看到。你可以搜索选择你想要查看的 CSS 选择器。

我发现这个功能比使用常规方式去寻找代码块更加方便。

![](/static/images/sublime-text/file-crawling.jpg)

## 7.拼写检查
我经常用代码编辑器写东西，但是我也经常把单词写错，如果你像我一样，你可以在 Sublime Text 用这种方式来做拼写检查。

进入Sublime Text 的 Preferences > Settings - User，增加下面这一行：

	"spell_check": true,

## 8.增强侧边栏
这个插件， SideBarEnhancements，这个插件给 Sublime Text 侧边栏带来了一些强大的功能，安装之后,右键单击工具栏,你将会看到额外增加的一些目录，例如打开文件夹，新建文件，新建文件夹，用其他方式打开，和用浏览器打开。

提示：按 F12 可以在浏览器中打开该文件。

![](/static/images/sublime-text/sidebar-enhancement.jpg)

## 9.改变 Sublime Text 主题
我们也可以改变整个SublimeText外观,我最喜欢的一个主题就是 [Soda](https://github.com/buymeasoda/soda-theme),我们可以通过包管理工具安装它。

![](/static/images/sublime-text/soda-theme.jpg)

如果您打算安装的主题不在控制存储库里,你可以用手工安装。

1. 下载然后解压主题包
2. 进入 Preferences > Browser Packages...
3. 把主题文件夹放入 Packages 文件夹
4. 然后进入 Preferences > Settings - Users,然后增加下面一行来激活你的主题


		"theme": "Soda Light.sublime-theme"

如果你用的是 Windows,你可以查看这个 [3步改变你的Sublime Text 2 主题](http://creatiface.com/tutorials/change-sublime-text-2-theme)

## 10.改变Sublime Text 图标
除了改变主题，你还可能改变图标，你可以在Dribbble上找到很多设计很漂亮的 Sublime Text 图标，这样你就可以改变图标的外观:

1. 从Dribbble下载一个图标，确保图标是 .icns格式，否则你可以先把这个工具:iConvert。
2. 在终端中运行下面的命令.

	open /Applications/Sublime\ Text.app/Contents/Resources/

3. 用你下载的一个图标替换 Sublime Text 3.icns 或者 Sublime Text 2.icn。


## 11.语法设置
如果你在多台电脑上工作，你可能想要在这些电脑上用相同的设置，我们可以在Dropbox的帮助下设置这个。

首先，在终端中运行下面这些命令。

    mkdir $HOME/Dropbox/sublime-text-3/
    mv "$HOME/Library/Application Support/Sublime Text 3/Packages" "$HOME/<a href="http://www.hongkiat.com/blog/out/dropbox" style="" target="_blank" rel="nofollow" onmouseover="self.status='http://www.hongkiat.com/blog/out/dropbox';return true;" onmouseout="self.status=''">Dropbox</a>/sublime-text-3/"
    mv "$HOME/Library/Application Support/Sublime Text 3/Installed Packages" "$HOME/Dropbox/sublime-text-3/"

然后，在其他电脑终端运行这个命令通过我们放在Dropbox的数据来同步你想要设置。

    DSTPATH="$HOME/Library/Application Support/Sublime Text 3"
    DROPBOX_PATH="$HOME/<a href="http://www.hongkiat.com/blog/out/dropbox" style="" target="_blank" rel="nofollow" onmouseover="self.status='http://www.hongkiat.com/blog/out/dropbox';return true;" onmouseout="self.status=''">Dropbox</a>/sublime-text-3"
    rm -rf "$DSTPATH/Installed Packages"
    rm -rf "$DSTPATH/Packages"
    mkdir -p "$DSTPATH"
    ln -s "$DROPBOX_PATH/Packages" "$DSTPATH/Packages"
    ln -s "$DROPBOX_PATH/Installed Packages" "$DSTPATH/Installed Packages"

## 12.可点击的URL
ClickableURLs是一个非常有用的SublimeText小插件,当你在您的代码里发现一群url。基本上,它将使url可点击。

## 更多
我已经写了一些，想了解更多可以访问：

* [Manage Notes And Lists With Sublime Text](http://www.hongkiat.com/blog/sublime-text-task-management/)
* [Easy Color Picking In Sublime Text](http://www.hongkiat.com/blog/sublime-text-color-addition/)
* [How To Refresh Changes On Browser With Sublime Text](http://www.hongkiat.com/blog/sublime-text-refresh-browser/)
* [Working With Code Snippets In Sublime Text](http://www.hongkiat.com/blog/sublime-code-snippets/)
* [Identify Code Error In Sublime Text With Sublime Linter](http://www.hongkiat.com/blog/identify-code-errors-sublime-linter/)
* [Adding CSS Vendor Prefix Automatically With Sublime Text](http://www.hongkiat.com/blog/css-automatic-vendor-prefix/)


原文：http://www.hongkiat.com/blog/sublime-text-tips/