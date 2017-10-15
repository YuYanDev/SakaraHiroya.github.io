---
title: Centos安装Munin服务器监控系统
date: 2016-12-31 22:34:00
---

为什么窝选择Munin系统，原因很简单，安全，轻量快速，可分析性强。

其实很多开源镜像站和一些老网站都采用这个系统，历史悠久，占用小，各种强悍的图表

Munin分为监控机软件(munin)和被监控机软件(munin-node),两个，其中一台机器上必须都安装（作为监控机）其余机器安装munin-node即可

安装非常简单。munin基于Perl，我们应该先装好perl全家桶。由于rrdtool在epel源里才有，所以我们应该提前设置epel源。
```
yum install munin munin-common munin-node rrdtool
```

这是两个非常重要的配置文件位置，关于如何增加节点在配置文件里都有配置说明和例子
```
/etc/munin/munin.conf  #主监控机配置
/etc/munin/munin-node.conf  #节点机器配置
```

有关nginx访问也很简单，munin安装好后，默认已经生成在`/var/www/html/munin/`下，我们只需把网站配置文件的location指向这个目录即可

munin默认记录监控日志，并自动压缩储存在`/var/log/munin`下

```
service munin-node start
```

我在使用中发现一个巨大问题，就是网页不会刷新，于是尝试手动刷新
```
/usr/bin/munin-cron
```
但是返回这个错误
```
This program will easily break if you run it as root as you are
trying now.  Please run it as user 'nobody'.  The correct 'su' command
on many systems is 'su - munin --shell=/bin/bash'
Aborting.
```
OK,我们切换到munin用户，当再次执行的时候，又报错
```
[ERROR] Could not copy contents from /etc/munin/static/ to /[path to static file] at /usr/share/perl5/vendor_perl/Munin/Master/HTMLOld.pm line 716.
```
Google一圈，找到解决方案[http://serverfault.com/questions/605226/munin-cron-unable-to-copy-contents](http://serverfault.com/questions/605226/munin-cron-unable-to-copy-contents)
明显是epel源那堆打包的工程师粗心好不好orz，好在可以结决了

```
su - munin --shell=/bin/bash  #切换到munin用户
chown -R munin:munin  /var/www/html/munin  #更改权限
exit #返回root用户
```

等五分钟你就可以看到正常刷新了


默认监听端口4949，多机集群别忘了放行端口。
<!--more-->
