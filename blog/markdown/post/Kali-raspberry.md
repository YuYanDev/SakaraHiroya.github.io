---
title: Kali在树莓派上的调教笔记
date: 2017-06-25 23:57:00
---

一个偶然的时刻，我发现某个地方的IP是开放80和443端口的，那如果不好好利用一下简直太可惜了。当然，人家可是良民，当然会遵守国家的法律法规啦，那废话就不多说了。

小插曲：好久不用raspbian了，刷好镜像通电后才知道raspbian已经在一年前就已经默认关闭了ssh，没ssh那玩个鬼。

索性放弃raspbian，那么目前有Ubuntu Mate，Kali OS，Arch Linux三个系统供我选择，Ubuntu Mate也是需要通过显示器设定，故放弃；Arch Linux arm安装需要用到linux的机子；所以选择了Kali OS。
<!--more-->
这是Kali的安装指导页面[https://docs.kali.org/introduction/download-official-kali-linux-images](https://docs.kali.org/introduction/download-official-kali-linux-images)里面包括了自定义构建镜像的方法和已构建好的镜像的下载链接。
也可以直接从这个页面寻获下载链接[https://www.offensive-security.com/kali-linux-arm-images/](https://www.offensive-security.com/kali-linux-arm-images/)


接下啦是刻录的方式，我的本子是OS X的系统，所以我写的是该系统的烧录方式
1. 下载
``` bash
wget https://images.offensive-security.com/arm-images/kali-2017.01-rpi2.img.xz
unxz kali-2017.01-rpi2.img.xz
mv kali-2017.01-rpi2.img ~/
```
2. 打开OS X的自带的磁盘工具，将tf卡格式化成MS-DOS(FAT)格式的。
3. 用`df -h`命令找到你的tf卡的挂在地址.
4. 假设我的是`/dev/disk2s1`，就执行`diskutil unmount /dev/disk2s1`，以此类推。
5. 执行`diskutil list`来找到tf卡，假设我返回来的是`/dev/disk2`
6. 使用dd命令来刻录`sudo dd bs=4m if=kali-2017.01-rpi2.img of=/dev/rdisk2`，注意of后指向的disk要加上r。刻录时间稍长，多等等。最后会打印record和transferred信息的。
7. 最后`diskutil unmountDisk /dev/disk2`来推出设备，就可以插上树莓派了。

这时，把树莓派接上网线。登入你的路由器控制面板。有以下几件事需要做

1. 打开客户端列表，查看主机名为kali设备的内网IP地址和MAC地址
2. 打开保留列表，将记录下来的内网IP和MAC地址绑定
3. 打开端口转发列表，将80和443或者其他端口转发至内网IP，协议默认即可，也可指定。
4. 如果路由器自带ddns服务的话，也可以设定。

好了，这个时候，我们可以登入Kali了，官方打包的系统，默认使用root用户。密码toor。登入后修改密码。

第一步先换源，树莓派版本的Kali是基于kali-rolling构建的，所以我们在修改软件源时要务必注意和检查
比如清华tuna源，在`https://mirrors.tuna.tsinghua.edu.cn/kali/dists/`里有`kali-rolling`所以理论上是可以用这个源的（当然的确是可以用的）
编辑镜像源列表`nano /etc/apt/sources.list`
```
deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main non-free contrib

deb http://http.kali.org/kali kali-rolling main non-free contrib
deb-src http://http.kali.org/kali kali-rolling main non-free contrib
```
刷新一下`apt-get update`

接下来是安装nginx的部分，由于对树莓派操纵数据库的阴影，(可以查看本博客最早的那些文章)所以不打算安装数据库，我不会写php，所以也没必要安装php。

``` bash
apt-get install -y unzip curl build-essential make gcc libpcre3 libpcre3-dev libpcre++-dev zlib1g-dev libbz2-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev libssl-dev
wget https://www.openssl.org/source/openssl-1.0.2l.tar.gz
wget http://nginx.org/download/nginx-1.12.0.tar.gz
tar -zxvf openssl-1.0.2l.tar.gz
tar -zxvf nginx-1.12.0.tar.gz
cd nginx-1.12.0

./configure \
--prefix=/etc/nginx                                                \
--sbin-path=/usr/sbin/nginx                                        \
--conf-path=/etc/nginx/nginx.conf                                  \
--error-log-path=/var/log/nginx/error.log                          \
--http-log-path=/var/log/nginx/access.log                          \
--pid-path=/var/run/nginx.pid                                      \
--lock-path=/var/run/nginx.lock                                    \
--http-client-body-temp-path=/var/cache/nginx/client_temp          \
--http-proxy-temp-path=/var/cache/nginx/proxy_temp                 \
--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp             \
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp                 \
--http-scgi-temp-path=/var/cache/nginx/scgi_temp                   \
--user=nginx                                                       \
--group=nginx                                                      \
--with-openssl=../openssl-1.0.2l                                   \
--with-http_ssl_module                                             \
--with-http_realip_module                                          \
--with-http_addition_module                                        \
--with-http_sub_module                                             \
--with-http_dav_module                                             \
--with-http_flv_module                                             \
--with-http_mp4_module                                             \
--with-http_gunzip_module                                          \
--with-http_gzip_static_module                                     \
--with-http_random_index_module                                    \
--with-http_secure_link_module                                     \
--with-http_stub_status_module                                     \
--with-http_auth_request_module                                    \
--with-file-aio                                                    \
--with-http_v2_module                                              \
--with-threads                                                     \
--with-stream                                                      \
--with-stream_ssl_module                                           \
--with-http_slice_module

make && make install
useradd -r nginx
mkdir /var/cache/nginx
touch /var/cache/nginx/client_temp
```

这时我们再创建一个systemctl启动脚本`nano /lib/systemd/system/nginx.service`
``` bash
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

就可以启动`systemctl start nginx`

接下来，我们去/etc/nginx里编辑脚本就可以啦,博客之前的文章也有。node还没有装，不过后续可能会更新吧。
![](/img/kalirasp.png)