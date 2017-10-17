---
title: Ubuntu(Linux)系统中搭建Android开发环境遇到的问题收集
date: 2017-10-11 22:43:43
categories: 问题收集
tags: [Linux,Android,Java]
---

<p>首先,本人是经常使用Ubuntu系统,毕竟写Android的绝对都是用这个系统写代码的,也不可避免偶尔重装系统,每次都要重新布置环境,所以有必要收集一下这些问题,以免我下次重装好解决这些问题,同时也有利于很多的Android开发者也遭遇同样的问题。
<p>我的linux系统版本是Ubuntu 16.04,然后使用到InllijIdea这个智能IDE,模拟器用到genymotion+VirtualBox。genymotion版本是2.4搭配apt-get那个virtualbox版本。
<!-- more -->
# 问题1
当我把程序build完后出现了下面的错误:
```
  Error: Gradle: Execution failed for task ‘:mytask’ > A problem occurred starting process 'command '/Android/Sdk/build-tools/23.0.1/aapt''
```
它大概意思是说运行aapt的时候出现了一些问题,原因是aapt不能运行在linux 64位系统下,所以才出现这个问题。<br>
解决方法如下(终端输入):
```
 sudo -s(输入你的密码)
 apt-get install lib32stdc++6
 apt-get install lib32z1
```



# 问题2
  genymotion2.4打开遇到的坑,其他版本可能没有这个问题,但是我每次首次打开genymotion都会遇到这个问题,我在打开目录后,然后./genymotion,弹出下面错误:
```
   genymotion: error while loading shared libraries: libgstapp-0.10.so.0:
   cannot open shared object file: No such file or directory
```
这个问题相对的比较简单,就是说我缺少某一些操作库<br>
解决如下:
```
sudo apt-get install libgstreamer0.10-dev   
sudo apt-get install libgstreamer-plugins-base0.10-dev
```
再次./genymotion就解决了!

# 问题3
![genymotion problem](/images/ubuntu-p1.png)
genymotion运行模拟器出现这样的问题,这里我直接到官网下载[Virtualbox](https://www.virtualbox.org/wiki/Downloads)下载最新版本就解决这个问题了,不过网上很多人说这种解决方法
```
   sudo /etc/init.d/vboxdrc setup
```
但是我的Ubuntu系统根本就不存在vboxdrc这个文件夹,这样输入就提示找不到命令。

# 问题4
![build](/images/ubuntu-p2.jpg)
IDE一直处于build project状态,首先,你要确认你的网络有没有问题,是不是断网了,如果确定没有,那就请看下面。之所以会卡在这种状态,是因为它要下载gradle版本从一个地址上,但是那个地址需要翻墙,所以IDE一直没办法下载下来就卡了。
### 解决办法1:
   翻墙你懂的,方式很多,有ss、ssr、vpn,只要能翻墙到外国网站都可以,然后IDE->settings->http proxy,设置你的代理,重新打开IDE就能下载了

### 解决方法2:(推荐)
   首先,到[这里](https://gradle.org/releases/)下载一个你喜欢的gradle版本
   假设我下载好的4.2.1版本放在了 /user/xxxx/下载 这个目录(这里xxxx是用户名)
   打开终端如下操作:
```
   cd /user/xxxx/下载
   mv gradle-4.2.1-all.zip /user/xxxx/.gradle/wrapper/dists/gradle-4.2.1-all/55gk2rcmfc6p2dg9u9ohc3hw9
```
注意了,55gk2rcmfc6p2dg9u9ohc3hw9是随机生成的,我和大家的肯定不一样!假如没有gradle-4.2.1-all这个文件夹的话,你需要随便找一个android poj,修改它的gradle版本为4.2.1,然后进去IDE build一会儿,然后就会生成这个文件夹!

# 问题5
## 症状一
![adb](/images/ubuntu-p3.png)
这个问题也是纠结了很久,adb无法使用或者端口5037被占用。网上几乎都是说什么360,豌豆啥占用了端口,把它杀死就OK了,但我这个并不是占用端口造成的！是因为Ubuntu 64位无法兼容32位的adb!解决如下:
### 配置环境:
```
sudo vim /etc/profile(在最后追加下面两行)
export PATH="$PATH:/home/username/sdk/tools"
export PATH="$PATH:/home/username/sdk/paltform-tools"

source /etc/profile
```
### 安装32位兼容库
```
  sudo dpkg --add-architecture i386
  sudo apt-get update
  sudo apt-get install libncurses5:i386 libstdc++6:i386 zlib1g:i386

```

## 症状二
每次重启都会遇到上面这个问题,又是开启不了adb,这类情况呢,是因为我的genymotion里面设定的adb问题,是因为我adb指向了genymotion里面的adb。
### 解决方法
![p5](/images/ubuntu-p5.png)
设置genymotion adb 默认是你的adb
或许这一步你可以解决这个问题,但是我的话还是没有解决。这里我还遇到了一个坑爹问题,就是我保存后,退出genymotion再次打开genymotion,但是adb仍然默认是genymotion的adb,意思就是我每次设定了一个位置,退出后没有保存设置,这问题实在是genymotion的一个大坑,对linux系统存在bug。我也很无奈,最后我自己是通过复制我本身的adb/tools/platform-tools所有文件覆盖到genymotion/tools文件夹。

# 问题6
![p6](/images/ubuntu-p4.png)
<p>这种问题是说我工程目录下/.idea/modules/里没有这三个文件,确实我进去我.idea目录看了一下,确实没有modules这个文件夹,顿时我就觉得很奇怪了,modules文件夹不是IDE自动帮我们生成的吗?为什么这次不帮我们自动生成,神奇了,项目是从github clone下来的,但我觉得问题起因不是这个,之前也没事,估计是我系统里面安装了一些不该安装的库吧!!
<p>最后我是通过删除了.idea这个文件夹后,重启IDE就自动生成了!


