---
title: TCP-BBR拥塞控制技术的简单分析及使用
date: 2016-12-16 12:45:00
---

TCP-BBR技术是Google实现的一项技术，在其正式生产环境通过后在ACM queue期刊发表: [http://queue.acm.org/detail.cfm?id=3022184](http://queue.acm.org/detail.cfm?id=3022184) 并提交到了Linux主线中。
这项技术致力于实现在劣质网络下达到较高的稳定性和可用性。

相信大家对CUCN 和 EAC-C2C 两条丢包率丧心病狂的海底光缆深有感触，许多好盆友都使用了例如锐速或者finalspeed等暴力发包软件来解决这个问题，这类软件虽然自己用得爽了，但是会加剧海底光缆的拥堵，以及可能导致由于你懂得的原因把IP给ban掉。

在看了相关参考文章后，在这里来分析一下：  
TCP-BBR技术呢，用了一种溢水原理的思想，来预判丢包率，调配发包速率。
假设你有一支较细的U形管，下面还有一堆不可溶的填塞物，你从一边开始大量灌水，如果另一边出水正常，你就可以继续加大灌水量，达到最大带宽。如果另一边发现水时断时有，就证明下面出现了随机拥堵，这时，你就要减小灌水量，等待水位落下。这时如果采用传统继续灌水时，也就会造成水溢出（丢包现象的产生）。所以这是真正的按需发包。当然，这一切是建立在系统预估的情况下。

以实际情况说，也就是应用程序在创立会话时，这项技术会增加额外少量的的会话，这些会话用于检测带宽和延迟，根据网路设备返回的情况来分析网路质量（确认延迟和带宽口径，以及回避不易确认丢包类型）。另外，这项技术中，BBR接管了TCP的控制权。

由于这项技术公布的时间也就一周左右，可能会出现许多不稳定和不科学的现象，期待更新以及Google更详细的doc。
知乎上的许多大牛也对这项内容进行了详细的分析 [https://www.zhihu.com/question/53559433](https://www.zhihu.com/question/53559433)

这里是几份权威的文章，较为详细的说明了TCP-BBR技术
[http://www.thequilt.net/wp-content/uploads/BBR-TCP-Opportunities.pdf](http://www.thequilt.net/wp-content/uploads/BBR-TCP-Opportunities.pdf)
[http://blog.cerowrt.org/post/bbrs_basic_beauty/](http://blog.cerowrt.org/post/bbrs_basic_beauty/)


接下来是具体安装运用方面

<!--more-->

Linux kernel 4.9已经加入了，我们只需要更新下内核并启用就行了

更新内核有风险，许多vps都有镜像功能，请镜像一份disk，如果你是单点服务器，也请做好备份！！

这里是公布的是基于do的Debian8适用的，其他发行版Google一下，如果arch什么的就等下次滚动更新吧233333

```
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9-rc8/linux-image-4.9.0-040900rc8-generic_4.9.0-040900rc8.201612051443_amd64.deb

dpkg -i linux-image-4.9.0*.deb   #安装内核

dpkg -l|grep linux-image  #查看服务器上已存在的内核

apt-get purge linux-image-3.16.0-4-amd64 #替换成自己的旧内核名称即可，
（可能会有提示另外一个` linux-image-amd64 ` 内核也要删掉，那就删掉吧。

关于Abort kernel removal选择 No

update-grub  #更新引导

reboot #重启
```

重启进入系统后，先 ` uname -a ` 一下看看内核是否更新成功

然后执行
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```
校验一下 执行 `sysctl net.ipv4.tcp_available_congestion_control` 返回值有bbr。
执行 `lsmod | grep bbr` 返回值有tcp_bbr。

最后再重启一下就可以啦ovo

当你看到这篇文章时，你已经享受到了TCP-BBR技术所带来的速度了。以下是一些须注意的坑

从kernel官网下的内核更新包仅支持老内核版本在4.3以上的机器更新，所以我们应该使用Ubuntu源的
不同系统的grub配置文件可能不一样，灵活处理。
