---
layout: post
title: "linux单个网卡添加多个ip"
date: 2011-08-03 00:52
comments: true
categories: LINUX
---
在linux下，我们有时候需要给单网卡设置不同的IP地址，这样就涉及到单网卡绑定多个IP地址的情况。使用本方法可以方便的为单网卡绑定多个IP地址。笔者使用的环境是centos6.2，应该在fedora 和rhel上都是适用的。
我们知道linux的网络设备的存储路径是/etc/sysconfig/network-scripts/
我们要使用的网卡是eth0，再看一下该设备的IP信息。

	IP：192.168.234.128 
	Brast：192.168.234.255
	Mask：255.255.255.0
添加一个IP只需在 /etc/sysconfig/network-scripts /创建一个ifcfg-eth0:x(x可以为0,1,2.......)。为了简便我们可以将ifcfg-eth0,复制一份命名为ifcfg-eth0:1即可，然后修改该文件。

	[root@localhost network-scripts]# cat ifcfg-eth0:1
	# Intel Corporation 82545EM Gigabit Ethernet Controller (Copper)
	DEVICE=eth0（修改为eth0:1)
	BOOTPROTO=none
	HWADDR=00:0C:29:96:93:CA
	ONBOOT=yes(表示开机启用，保留）
	NETMASK=255.255.255.0
	IPADDR=192.168.234.128（修改为新的ip，192.168.234.100)
	GATEWAY=192.168.234.255
	TYPE=Ethernet
	USERCTL=no
	IPV6INIT=no
	PEERDNS=yes

最后启用设备，使用命令：ifup eth0:1