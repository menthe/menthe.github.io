---
layout: post
title: "ubuntu主题集锦"
date: 2013-01-21 12:56
comments: true
categories: LINUX
---
最近打算入手一块128G的SSD，这样电脑就又两块硬盘了，分区是肯定要改变的。所以记录一下ubuntu上自己比较喜欢的一些软件以及其安装的ppa，方便重做系统之后再使用。

主要是记录一些比较容易忘记的软件，至于像chrome,opera,filezilla,dropbox,eclipse,这样的基本软件就不用记录了，相信大家在ubuntu-tweak上都很容易就看到了。

####GTK主题
1）Orion
<!--more-->
一款非常好看的GTK主题，安装之后的样子如下图：<img src="http://i.imgur.com/ZgsTcXX.png" alt="" title="Orion GTK主题" />
安装方法
	sudo apt-add-repository ppa:satyajit-happy/themes
	sudo apt-get update 
	sudo apt-get install orion-gtk-theme

2）Zokitwo
也是一款很好的GTK主题<img src="http://i.imgur.com/0Hka3A3.png" alt="" title="Zokitwo GTK主题" />

安装方法：
	sudo add-apt-repository ppa:tiheum/equinox
	sudo apt-get update
	sudo apt-get install faenza-icon-theme

####图标主题
1）Faenza图标主题
这应该是在ubuntu和feodra上用的最多的图标主题
<img src="http://i.imgur.com/2Ryx34q.png" alt="" title="Faenza图标主题" />

安装方法：
	sudo add-apt-repository ppa:tiheum/equinox
	sudo apt-get update
	sudo apt-get install faenza-icon-theme

2)AwOken图标主题
一款很清新的图标主题
<img src="http://i.imgur.com/3xrI1D7.png" alt="" title="AwOken图标主题" />

安装方法：
	sudo add-apt-repository ppa:alecive/antigone
	sudo apt-get update
	sudo apt-get install awoken-icon-theme

3)NITRUX图标主题
<img src="http://i.imgur.com/5T79diu.png" alt="" title="NITRUX主题" />

安装方法：
	sudo add-apt-repository ppa:kokoto-java/omgubuntu-stuff
	sudo apt-get update
	sudo apt-get install nitrux-umd