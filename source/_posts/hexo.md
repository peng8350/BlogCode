---
title: 记录Hexo+Page坑爹记录
date: 2017-09-28 22:07:14
categories: 爬坑
tags: [HTML+CSS,Hexo]
---

## 安装HEXO坑记录
搭建过程好像用了3-4天,由于本人对网页方面都不懂,真心累,什么Hexo+Node.js,以前也没有听说过这种东西。话说这种静态博客的模式限制很大,比如我想自定义一个网页什么的。在Ubuntu系统下,在第一个搭建Hexo步骤中:

```
   sudo apt-get install npm
   sudo apt-get install node.js
   npm install hexo-cli -g

```
<!-- more -->
在hexo安装的过程中，突然会抛出下面的错误:
```
   npm ERR! Failed at the hexo-util@0.6.0 postinstall script 'npm run build:highlight'.
npm ERR! Make sure you have the latest version of node.js and npm installed.
npm ERR! If you do, this is most likely a problem with the hexo-util package,
npm ERR! not with npm itself.
npm ERR! Tell the author that this fails on your system:
npm ERR!     npm run build:highlight
npm ERR! You can get information on how to open an issue for this project with:
npm ERR!     npm bugs hexo-util
npm ERR! Or if that isn't available, you can get their info via:
npm ERR!     npm owner ls hexo-util
npm ERR! There is likely additional logging output above.	

```
出现这个问题是因为我的node.js版本太低了,通过命令行apt-get安装版本是4.2.6,最新是6.3多,解决方法是去他官方下载一个,可能是因为写教程的那时版本还不算太旧。

## 环境变量
   解决上面的那个问题后,也不是这样就完了的,解压了node.js后放在一个地方后,要设置node.js的环境变量才能生效,这里我也搜了一下百度,至于为什么不google,就是因为本人的英语也不是太好,所以很难找关键字。那么问题来了,我配置环境变量的时候遇到了什么挫折呢?这里,网上我找到最多配置环境变量方法如下
首先sudo gedit /etc/profile,在后面下面两行,NODE_HOME指向你nodejs的目录

```
 export NODE_HOME=/usr/local/node
      export PATH=$NODE_HOME/bin:$PATH
	
```
最后,source /etc/profile

   这样我就撞到了一个坑,就是确实在普通用户下输入hexo,确实没有问题,但是一旦我切换超级用户,sudo -s后,再输入hexo,它会提示你找不到没有node那个环境变量

```
   /usr/bin/env: node: 没有那个文件或目录
```

   最后,我是通过在/~这个目录下(/home/$user),通过bashrc这个文件来生效环境变量,这可能是我ETC文件夹权限问题吧,重装后没有出现这个问题,那个时候莫名其妙地突然系统文件全部权限都不可写。

## hexo d坑1:
   博主重装了系统,从github把博客代码clone了下来,修改了一下文章,打算重新post上去,hexo clean->hexo g这里步骤都没有问题,但是hexo d就遇到了一个很坑的问题。
```
   Error: error: src refspec HEAD does not match any.
   error: 无法推送一些引用到 'https://github.com/xxxx/xxxx.github.io.git'

```
   如上,从错误的意思来看,大概是说我没有什么东西可以提交上去,然后我看了看里面的public文件夹,确实被hexo g生成了,搞了半天原来是要先配置ssh,才可以上传上去,Ubuntu系统下操作如下:
```
git config --global user.name "yourname"
git config --global user.email "youremail"

```
   接下来,就是要生成ssh key
```
ssh-keygen -t rsa -C "youremail"
```
   接着再hexo d就没有出现刚刚的问题了。

## 结束!
   






