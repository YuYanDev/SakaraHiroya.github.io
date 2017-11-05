---
title: 给树莓派滚到了Jessie
id: 46
date: 2015-11-28 01:54:00
tags:
---

<!--markdown-->难得周六不补课，折腾一下小pi，性能优化一下。
这次是在pi2上进行的，老的b+版编译到kernel4.3内核后，操作还好好的，然后滚Jessie的时候就滚挂了，原因不详………

 1\. 移除web相关组件
 2\. 精简系统
 3\. 升级系统
 4\. 再次精简优化
 5\. 配置vim
 6\. 无   #差点把桌面换成了gnome
<!--more-->
上次的lamp拖得小pi每次top一下都吓死人，先把这个东西移除了吧……
```
sudo apt-get autoremove apache2
sudo apt-get autoremove mysql
sudo apt-get autoremove php5
sudo apt-get remove phpmyadmin
```
精简系统，把python，pythongame，小奶瓶，wifite都移除掉，用法如上。
升级系统，换源，这次tuna源炒鸡卡，100kb的速度……下载依赖总共差不多花了2个小时……
以下是具体食用方法
```  
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```    
进入`/etc/apt/sources.list`把所有wheezy字符换成jessie，重复以上操作
```  
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```
中途会提示很多是否保留源配置，更具自己的需要选择即可，也可以查看更新内容
有提示说是否是从ssh操作，需如实选择，否则会boom

重启`sudo reboot`

继续精简，系统居然少了许多组件，只需要移除python相关即可

安装vim，vim是树莓派上能用的编辑器中，最好用的一个了……
```   
sudo apt-get install vim
```    
然后开始塞插件233333333
