---
title: 针对HTTP/2的一些优化
date: 2016-02-20 14:49:00
tags:
---

HTTP/2现在来说已经不新鲜了,很多博客第一时间都用上了HTTP/2.但是里面有很多小细节值得推敲!

很多人打开chrome的控制台一看,WoW~ h2启用成功了.实际上并不然,这只代表了服务器给你发出了HTTP/2的CipherSuite,不一定真的启用了HTTP/2.但是我们可以用抓包工具来check.这个就不详细说明了.看使用文档去23333333
但我们可以用更简单的方法来检验->SSLLABS[https://www.ssllabs.com](https://ssllabs.com)这是一款在线的ssl检测的网站,权威且专业.
检测完毕后,我们可以查看Handshake Simulation,从IE6到最新的Edge都有
如果没有使用HTTP/2,则显示
`TLS 1.2 > http/1.1`
如果HTTP/2启用成功,则显示
`TLS 1.2 > h2`

如果算法调不好,甚至还会出现Chrome和ff无法访问的情况
如何正确的使用HTTP/2.关于握手算法我推荐
```
ssl_ciphers  ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:AES128-GCM-SHA256:AES256-GCM-SHA384;
```
如果你使用了BoringSSL 我推荐[JerryQu](https://www.imququ.com)提供的算法
```
ssl_ciphers [ECDHE-ECDSA-AES128-GCM-SHA256|ECDHE-ECDSA-CHACHA20-POLY1305]:[ECDHE-RSA-AES128-GCM-SHA256|ECDHE-RSA-CHACHA20-POLY1305]:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:AES128-GCM-SHA256:AES256-GCM-SHA384:DES-CBC3-SHA;
```
另外,博主发现了几个BoringSSL的bug,希望大家注意!
1:BoringSSL下,nginx-ct模块目前处于失效状态,无法启用
2:安装好后编译nginx时会报错
` || n == SSL_R_NO_CIPHERS_SPECIFIED              /*183*/ `
解决方案 `nano ./src/event/ngx_event_openssl.c` ,然后删除这一行
另外,关于食用BoringSSL的方法,以及另外的一个BUG.可以查看JerryQu的文章[https://imququ.com/post/optimize-ssl-ciphers-with-boringssl.html](https://imququ.com/post/optimize-ssl-ciphers-with-boringssl.html)
<!--more-->
