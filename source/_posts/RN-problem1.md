---
title: ReactNative入坑记-No Bundle Url Present
date: 2018-3-14 15:23:43
categories: 爬坑
tags: [ReactNative,Mac OS]
---

# 起源
  当我有时想要重新打包运行IOS的时候,偶尔会出现这个坑爹的问题
  <!-- more -->
  <p>![](/images/爬坑/图1.png)
  <p>如图,上面这个奇葩错误在我入门搭建环境的时候就碰到了,那时我的解决方案:
  
  * 重新启动电脑 
  * 关闭模拟器,关闭所有终端,重新react-native run-ios
  
<p>而今天连续好几次react-native run-ios都出现这个问题,因为我为了调试fetch 那个API在IOS的异常,尝试多次修改info.plist并重新打包无果！实在不能忍,放掉当前问题,去百度stackoverflow解决这个问题先。
  <p>经过一番的网上找答案,绝大多人都是给出这个解决方案:
  
  ```
  
  	npm install
  	
  	react-native run-ios
  	
  	
  ```
  <p>然而,This is Nothing to do with it。(无果)
  <p>最后,偶尔找到一个博客是说<b><font color='red'>shadowsocks</font></b>的问题,确实!这个才是真正的答案,虽然不知道为什么<b><font color='red'>shadowsocks</font></b>翻墙会阻碍IOS重新生成。但关闭<b><font color='red'>shadowsocks</font></b>的代理之后,重新react-native run-ios却解决了这个问题
  
# 解决方案
  ![](/images/爬坑/图2.png)
  
  * 关闭<b><font color='red'>shadowsocks</font></b>代理
  * 切换全局模式为代理模式
  

# 参考
![http://blog.csdn.net/suyie007/article/details/70768410](http://blog.csdn.net/suyie007/article/details/70768410)



