---
title: '作死向:利用树莓派(RaspberryPi)打造LAMP服务器'
id: 8
categories:
  - 树莓派
date: 2015-07-05 15:39:00
tags:
---

<!--markdown-->目前树莓派是最火的开源平台之一，性能拓展性也是最强的，使用它来打造服务器是个很好的主意

(LAMP=Linux+Apache+MYSQL+PHP)别问我为何不用Nginx，他跟我有仇orz
这里用的是Pi2(因为老B+今天骄傲了，死活开不了机)

看了汪汪姐的[起司博客][1]非编译手动安装服务器环境的原理后，决定动手一试

<!--more-->
```
sudo apt-get install apache2 mysql-server mysql-client php5 php5-gd php5-mysql
```
等待即可，po主家里网速感人，大家可以先换源，po主用的是重庆大学的源http://mirrors.cqu.edu.cn换源方法进入此地址可见

当中会提示键入MySQL的root密码

赋予权限
```
sudo chmod 777 /var/www/
```
安装phpmyadmin
```
sudo apt-get install phpmyadmin
```
（当中会提示选用什么版本 我们选Apache2 空格选择，回车确定）
启用重写规则
```
sudo a2enmod rewrite
```
启用phpmyadmin（就是在浏览器上能访问）
```
sudo ln -s /usr/share/phpmyadmin /var/www
```
重启apache
```
sudo service httpd restart
```
⑧用phpinfo验证
```
cd /var/www
nano phpinfo.php
```
写入以下内容
```
<?php

phpinfo();

?>
```
保存退出
键入ifconfig查看ip地址 在浏览器上键入`http://IP/phpinfo.php`即可查看
<!--more-->
  [1]: http://tntsec.com
