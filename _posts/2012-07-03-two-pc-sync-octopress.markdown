---
layout: post
title: "两台计算机同步octopress"
date: 2012-07-03 00:21
comments: true
categories: Octopress
---
1）为什么会有这一篇文章？

我刚开始是在ubuntu中搭建ruby环境，但是经历了两次失败之后，我就放弃了使用ubuntu，转而在我的win7系统中搭建的octopress，参考的文章是[在 Windows7 下从头开始安装部署 Octopress](http://sinosmond.github.com/blog/2012/03/12/install-and-deploy-octopress-to-github-on-windows7-from-scratch/) ，后来我ubuntu使用的比较郁闷了就换了archlinux，于是在arhclinux中搭建环境很顺畅。这时候要想在archlinux中写博客，那就要重新搭建octopress了。

2）具体的步骤：

**安装Ruby**

具体的参考[Octopress Setup](http://octopress.org/docs/setup/) 安装步骤来，我没有出错，如果出错，请自行google。

**获取自己github上的octopress仓库**

	git clone -b source git@github.com:username/username.github.com.git octopress
	cd octopress
	git clone git@github.com:username/username.github.com.git _deploy
<!--more-->
**安装依赖gems**

	gem install bundler
	bundle install
	rake install
网上有一个兄弟说gem install bundler比较慢，可以使用gem install bundler --pre来提高速度，但是我表示教育网安装的很快，但是在电信网确实蛮慢的，估计是个例。

**更新_deploy和source内容**

	cd _deploy
	git pull origin master
	cd ..
	git pull origin source
至此，我们在另外一台pc上的octopress环境就搭建完成了，**以下是比较重要的说明**：
试想一下，如果我们此时在这个环境中执行：

	rake new_post['title']
	rake generate
	rake deploy
会发生什么呢？显然，我们所有的文章只剩下刚刚新建的这个了。所以我们需要做的是将旧的\_post目录下的文章***同步***到新的_post目录下，这就是git的用法了。

进入旧的_post的目录下：

	git add *
	git commit -m
	git push origin source
每次写文章之前，进入当前的_post目录下：

	git pull origin source
写完之后，执行：

	git add *
	git commit -m
	git push origin source
**也就是说，每次写之前写之后都要与仓库进行同步！**