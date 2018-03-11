---
title: MBR+Clover黑苹果安装教程
date: 2018-3-11 21:00:43
categories: 黑果
tags: [Mac OS]
---

# 介绍
  这几天为了研究Reaect-native,看不到在IOS上的调试,实在不爽,所以选择安装回以前的苹果系统,本教程使用MBR分区+Clover的方式安装黑苹果,实现与WIN7双系统。基本上95%完美,除了睡眠和外接显示器有一点小问题
  
  <!-- more -->
## 驱动安装情况
### 显卡
使用RehabMan提供fakepciid假冒显卡驱动,水波纹透明,动画流畅,基本完美,dsdt注入实现亮度调节,可以HDMI外接显示器输出
### 声卡
仿冒283驱动,没有爆音等奇怪现象,电脑连接耳机有声音
### 有线网卡
系统显示已经加载了这个驱动了,因现在不用有线接网,所以没测试过,过去使用也是没有问题的,不知道在这个版本OS行不行,应该没什么问题
### 无线网卡
在过去2年,一直以为ar9565这个无线无解,没想到竟然有人破解了这个网卡驱动,亲测试,没有问题,状态栏有图标,搜索到信号
### 其他
摄像头已驱动,USB3.0可用,外接显示器VDI转HDMI可以使用

## 安装环境:
WIN7<br>
Acer E5 572G 52DX<br/>
MAC OS X 10.12.6
## 配置
* 显卡:HD4600
* 声卡:ALC283
* 无线:AR9565
* 有线:RT81111

## 使用到的工具:
### 8GU盘
作用不多说
### MacDrive8(需破解)
该工具的作用就是让MAC光驱可以被看到,如果没有这个工具,你安装好镜像或者装好MAC系统后会看不到光盘

### Transmac
用于安装mac懒人版镜像(也就是安装盘)到U盘

### 7-zip
用于解压Clover bootloader

### CloverISO-4392 bootloader
[链接](https://sourceforge.net/projects/cloverefiboot/files/Bootable_ISO/),版本最好用这个,因为我实际测试的时候不同版本还是有很大区别的,会造成无法进入clover引导界面

### EasyBcd
用于安装CloverISO-4392镜像到引导选项

### Kext wizard
MAC下的工具,这个用来安装驱动到s/l/e,并且可以修复缓存

### 黑苹果懒人版镜像(CDR)
这个是肯定要的,没有安装盘那又如何安装黑苹果呢,是吧。不过选择懒人版的时候,看清楚它的要求,比如有一些它不适用Clover或者用于变色龙引导的,或者只用于GPT。我有一次掉坑就是选错了,造成无法安装的问题。这里,我推荐这个. 链接: [https://pan.baidu.com/s/1gflA8iJ](https://pan.baidu.com/s/1gflA8iJ) 密码: 6u7e

### Ext2Fsd(可选)
这个工具作用和上面差不多,也是要查看分区,不过这个通过改盘符来查看分区

### Clover Configurer(可选)
Mac的工具,用来配置config.plist,快捷




# 步骤
## 将懒人版镜像安装到你的U盘
对于U盘的要求,只要你的U盘有8G大的就可以了,因为CDR本来就6G,安装上去也就6G左右
![1.ng](/images/mac安装教程/图1.png)<br/>
安装到U盘,要使用到这个工具叫transmac,如上图,我有三个盘,第一个是我的本来笔记本硬盘,第二个是我的SSD,第三个就是我的u盘。右击第三个,选择Restore with Disk Image。
然后弹出对话框，选择你刚刚安装好的懒人版镜像(CDR,安装盘镜像),然后就开始安装到你的u盘了,这个时间很长,你要等待20-40分钟左右,就看U盘的读写速度了,这个时候可以去喝杯茶。

## 新建一个卷
这个卷的作用就是真正放Mac系统的盘,过程不多说,就是右击我的电脑,然后点击管理,选择你有多余空间的盘,然后新建,注意新建的时候,要选不要格式化光驱,不要格式化光驱,不要格式化光驱,新建好的盘就是你要待会安装MAC系统进去的盘(Mac盘只有一个)

## 设置待安装Mac格式
通过Win+R cmd,然后输入

```

	diskpart
	//这里要变化,下面意思选择磁盘1(下标从0开始),因为我的SSD位于第二,
	//如果你想知道你的要安装的磁盘在哪里，你可以输入list disk命令
	sel disk 1
	//我的光区在第二个位置
	sel par 1
	//重点下面这句,意思把格式改为af
	set id=AF
	
```

## 为安装盘(U盘)添加EFI,删除一些阻碍驱动
<p>至于EFI的选择,要根据你的机型实际的情况,每个人的EFI里面的内容会不同,主要的文件有:EFI/CLOVER/config.plist和EFI/CLOVER/kexts,/EFI/CLOVER/ACPI/patched,第一个就是你的clover配置文件,第二个就是放置要加载的驱动,第三个就是DSDT实现一些特别的功能.这个会影响到你进入安装界面是否会五国,可以参考我的EFI,<a href="https://github.com/peng8350/E5-572G-EFI">地址</a>
<p>第二,删除S/L/E(System/Library/Extensions)不是必须的,但也是必要的当你出现五国(无法进入安装界面)的时候,至于删除什么驱动,这个不是确定的,因为机型不同,要看实际情况,容易造成五国无法进入安装界面的驱动有:NV开头(Nvdia驱动),AppleIntelCpu两个kexts,iobluotooth两个(蓝牙)。

## 用easybcd添加CloverISO-4392 bootloader.iso
添加镜像引导进去clover引导,easybcd添加clover引导镜像过程省略(没截图...)。装好后，可以重启进入选项引导了,选择你刚刚添加的引导选项,不出意外的话就可以进去
![2.png](/images/mac安装教程/图2.jpg)
上图,进去选好你的安装盘按空格,会出现这个,然后boot with injected kexts还有Verbose(-v,就是调试嘛,看日志),别选without,因为要加载kexts里面的fakesmc,没有这个是没办法进入的。
![3.ng](/images/mac安装教程/图3.jpg)
会刷一大堆日志出现，这里你要最注意的是waiting for dsmos...还有dsmos has arrived这两句话很重要,如果dsmos has arrivedz这句话没出来,原因大概是这两个:

* 如上面我所说,s/l/e一些驱动干扰
* 没有成功加载fakesmc.kext,如果确定加载了，你可以试下换最新版的fakesmc

### 现象二
出现system uploadtime...的字样

* 一般就是你的电源驱动问题,没有驱动那个nullpowermanagement.kext，或者没有删除S/L/E下的两个电源管理驱动
* 把USB 3.0的位置换到2.0位置试试

### 现象三
刷完一堆代码后立马电脑自动重启,原因就是内核问题,像我这笔记本出现过这个问题很多次,变色龙引导也有这问题,最后打了haswell内核补丁,在clover里config.plist设置一个参数为true就可以解决了,-xcpm为true好像是这个。

## 进入界面开始安装
![4.png](/images/mac安装教程/图4.jpg)
第一步,就是打开磁盘工具(左上角,慢慢找),对你刚刚新建并且改ID的盘进行摸盘操作,这里要选择MAC的格式要选择Mac分区(日志),但我这里显示英文字样的。然后可以开始安装了，等待安装完毕，重启

## 重启进入WIN7复制EFI(可以是安装EFI或者是你别人弄好的EFI)到MAC盘
因为你要依赖FAKESMC这个驱动才能进入系统的,这个过程不可少,复制完后,确保config.plist参数适合你的机子,fakeSMC.kext一定要存在

## 重启进入clover引导
这里也不要忘了boot with injected kext,能进去的话,把你自己准备好的驱动放到S/L/E然后修复权限和缓存,这里就靠你自己驱动了,可以到远景论坛(收费注册)或者自己百度找驱动吧..

# 存在问题

* 外界显示器突然间花屏,好像是电脑几分钟不动会有概率产生这情况,或者是我自己的外界显示器本身问题吧,目前原因不明
* 睡眠问题,可以实现睡眠,但是一旦睡眠时间长了,再次开机的时候无法唤醒一直黑屏

# 结尾!



