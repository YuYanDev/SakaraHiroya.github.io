---
title: 一个Shadowsock Bug
date: 2016-02-12 09:32:15
tags:
---

这个bug发现于安装shadowsock-qt5客户端时

由于debian没有ppa源的支持，我们只能用dpkg手动构建shadowsock-qt5，那么问题来了.....
```
sudo apt-get install qt5-qmake qtbase5-dev libqrencode-dev libqtshadowsocks-dev libappindicator-dev libzbar-dev libbotan1.10-dev
```
debian源里木有这个`libqtshadowsocks-dev` QAQ

好在Github有libqtshadowsocks [https://github.com/shadowsocks/libQtShadowsocks](https://github.com/shadowsocks/libQtShadowsocks)
```
sudo apt-get install qt5-qmake qtbase5-dev libbotan1.10-dev
dpkg-buildpackage -uc -us -b
```
恩，正常，但是安装时 报错了
```
dpkg-split：错误：读取 libqtshadowsocks 时出错: 是一个目录 #理论上包管理器已经纠正
dpkg:../../src/unpack.c:123:deb_reassemble: 内部错误：unexpected exit status 2 from dpkg-split
```
查找到两个bug反映，可以推测是ss的问题

至此在debian下构建ss客户端只能通过手动编译的方式，强行PPA源的方式不推荐，也不期望.....
<!--more-->
