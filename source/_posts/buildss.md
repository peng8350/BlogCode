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
ssr config输入完后,这里有一大堆东西
```
{

    "server": "108.61.119.120",
    "server_ipv6": "::",
    "server_port": 3612,
    "local_address": "127.0.0.1",
    "local_port": 1080,

    "password": "ntdtv.com",
    "method": "aes-256-cfb",
    "protocol": "auth_sha1_v4",
    "protocol_param": "",
    "obfs": "tls1.2_ticket_auth",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {}, // only works under multi-user mode
    "additional_ports_only" : false, // only works under multi-user mode
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",

}

```
<p> 这些都不用我说了吧,我们要注意的是protocol还有obfs这两个键字,这两个东西代表SSR的协议和混淆,通常情况下都要修改一下的,其他参数看你自己实际情况更改!接着,和上面第三个步骤需要switchyomega这个插件,不过这里区别最大的是,一定要用SOCKS代理,不用HTTP代理,因为SSR默认的本地服务器类型就是SOCKS5!

