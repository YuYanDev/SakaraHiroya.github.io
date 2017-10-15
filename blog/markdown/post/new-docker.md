---
title: 新年新气象，全新的服务器环境
id: 50
date: 2016-01-01 02:45:23
tags:
---

<!--markdown-->终于把这个坑填了，趁着元旦3天假期。
CentOS6.6 + Nginx-1.8.0(with spdy3.1) + MySQL5.5 + PHP7.0.1

nginx+spdy3.1的速度感觉的确是比apache快的，尤其是启用了gzip之后。

好多人吐槽窝的网站环境比较奇葩（nginx去编译，而MySQL和PHP用包管理器），但这是按窝的偏好来配置的23333333。包管理器方便省时，nginx编译又可以增加一些其他的模块。

至于为何不编译nginx1.9.9 with HTTP/2。原因很简单，ssl加密算法兼容性还不够好。

感谢epel,remi,atomic源的支持。
<!--more-->
