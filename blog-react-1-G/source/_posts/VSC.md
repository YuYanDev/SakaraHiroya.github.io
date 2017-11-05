---
title: Visual Studio Code初体验
id: 36
date: 2015-10-04 14:30:00
tags:
---

<!--markdown-->今年早期的时候，<del>巨硬</del>发布了这款编辑器，跨平台，支持多种编程语言，据说未来还会提供插件支持（像Atom一样）无奈系统装的是32bit的，无缘Atom（<del>主要是懒得折腾</del>）
一开始是打算Sublime Text的，后来有个小伙伴推荐了这个编辑器，（据说更专业，看起来也是，嗯 ∑(O_O;) ）
<!--more-->
Linux通用安装方式(Ubuntu除外)
下载地址：[https://www.visualstudio.com/products/code-vs][1]
挑自己的系统下载，挺小的
下载完后，mv到你想放的地方，然后unzip，
点击.code来启动
最好把这个code给 ln -s到/usr/local/bin/code里，方便启动
当然你也可以 code 文件位置 来直接编辑文件
最好chmod +x code一下

Ubuntu百度一下一堆，非要另类去搞make……

第一次使用这种专业编辑器，感觉太好用了，自己补全，自动排版，还具有很多提示！<del>以前用nano写东西好煞笔呀</del>
如果你wget一下tris.pw当中的网页看看，代码排版一团糟，web软件的conf也是经常因排版问题不识别……有时候还会掉括号（多层负载均衡的的配置文件还要一个个的排错简直要疯掉……）
wget命令`wget -m -e robots=off -U "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) Firefox/3.5.6" "http://网站域名/"`手机端改下UI即可（这个可以用来镜像静态站点，并且可以无视robots.txt)



PS:关于debian没有预置intel网卡驱动的解决方案
起初尝试的方案:去Intel官网寻找相关内容，得到了一下两个页面
[http://www.intel.com/support/cn/wireless/wlan/sb/cs-034398.htm?wapkw=iwlwifi+-1000-5&_ga=1.176836653.927392567.1439982352][2]
[https://wireless.wiki.kernel.org/en/users/Drivers/iwlwifi][3]
高高兴兴的去下载，然后只得到了5个当中的2个，`当然，这两个页面可能对你的机器有用`
接下来去wiki上找找，得到方案
`sudo apt-get install firmware-iwlwifi`
`sudo modprobe -r iwlwifi ; modprobe iwlwifi`
收工

  [1]: https://www.visualstudio.com/products/code-vs
  [2]: http://www.intel.com/support/cn/wireless/wlan/sb/cs-034398.htm?wapkw=iwlwifi%20-1000-5&_ga=1.176836653.927392567.1439982352
  [3]: https://wireless.wiki.kernel.org/en/users/Drivers/iwlwifi
