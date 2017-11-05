---
title: Shell学习笔记(1)
id: 42
date: 2015-11-08 17:21:00
tags:
---
<!--more-->
为何要学习Shell，不说了，懒人必备，大多数一键安装包都是用shell写的，今天开坑了。
简单的大家都会玩，一个简单的例子说明一切
For example 重启一下lamp
```
#!/bin/bash
service httpd restart
service mysqld restart
service php-fpm restart
echo "Lamp restart complete"
```
这样大家都会，所以我们所要学习的，就是交互式shell，比如今天学会的这个：
```
#!/bin/bash
PIDNUM=$(pgrep httpd | wc -l) #检测apache进程
if [[ $PIDNUM -eq 0 ]];then #检测apache的pid状态
          echo "Apache is stopped."
          service httpd start
else
          echo "Apache is running."
fi
```
这样我们的脚本就聪明多了
这次学到的东西，就是用$去定义一个活动，（检查pid 启动apache），以及if条件句的使用。当然我们还可以输出一个apache的pid号，方便使用
```
echo "Proccess number is $PIDNUM."
```
23333333
