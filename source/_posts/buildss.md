---
title: Ubuntu系统下搭建SS或者SSR翻墙教程
date: 2017-10-13 19:58:10
categories: 教程
tags: [Ubuntu,翻墙]
---

本文采用Chrome+switchyomega+shadowsocks实现翻墙。

工具: Chrome+Shadowsocks
<!-- more -->
## Shadowsocks-qt5
### First one
通过命令行直接安装

```
   sudo add-apt-repository ppa:hzwhuang/ss-qt5
   sudo apt-get update
   sudo apt-get install shadowsocks-qt5
```

### First two
通过上面的步骤,shadowsocks已经安装好了
接下来打开shadowsocks,点连接->新建->手动,这里我就不截图了,因为工具本身很难支持
![p1](/images/buildss-p1.png)
<p>如上,服务器地址就是填SS服务器的IP地址,密钥就是它提供的密码,端口就不解析!本地地址一般指向本机,本地端口随意,但是待会要用到的,加密一般都是AES-256-CFB，选HTTPS不选socks5。确定。

### First three
<p>然后,百度找一个叫switchyomega的插件
![p2](/images/buildss-p2.png)
如图,上面的地址和端口都是对应我们刚才填写的本地地址和本地端口!!
![p3](/images/buildss-p3.png)
选择代理,这样几乎好了,然后你打开google.com应该没问题,假如不行,看一下代理可不可以用,测试方法是点击测试延迟,看一下有没有网速!

## ShadowsocksR
同样的这种方法,SSR也是和Shadowsocks差不多
```
   wget http://www.djangoz.com/linux_setup_ssr/ssr
   sudo mv ssr /usr/local/bin
   sudo chmod 766 /usr/local/bin/ssr
   ssr install
   ssr config
```
然后你可以使用ssr start,ssr stop等命令启动或停止SSR


