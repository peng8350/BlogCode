---
title: ReactNative入坑记-调试奇葩错误集合
date: 2018-03-25 23:11:24
categories: 爬坑
tags: [ReactNative,Mac OS]
---

# 调试运行错误集合

## 错误一
<!-- more -->
![错误一](/images/爬坑/图3.png)
<p>如图,貌似是想表达因为什么原因不能把一个View添加进入结点,我英文很差的别吐槽。这个错误很奇怪,确实很奇怪,为什么呢?因为我IOS里跑的话，完全没有这个问题的,而Android就出现了这个问题。
<p>我找了半天代码都没发现什么问题,经过大量google和stackoverflow,搜索到的信息里面很多人都说因为return 里面有注释语句的原因,导致你出现这个调试错误

```
  render() {
  
    return (
      <View style={styles.container}>
        <TabBar navigation={this.props.navigation}/>>
      </View>
    );
    
  }
  
```
经过本人的大量仔细调查,我并没有在render方法里写注释,不过唯一让我起疑的就是我昨天刚刚封装的组件里出了问题。

```

	render() {
  
    return (

        <TabBar navigation={this.props.navigation}/>>
  
    );
    
  }

```
果然不出我所料,尝试把View根节点删去就没事了,虽然解决了这个问题,但是还是很疑惑,为什么不能让我在外面多添加一个View呢?是它内部人员写的BUG还是固定为了性能让我们去掉多余的View?这个问题有待以后探讨...

## 错误二

<p>在ios上尝试调用fetch Api都会报 type Error:Network Request Failed,而在Android平台上没有这个问题.估计这个问题是fb官方的一个bug

### 解决方法

 ![错误一](/images/爬坑/图4.png)
 
* 在Info.plist中添加 NSAppTransportSecurity 类型 Dictionary ; 
* 在 NSAppTransportSecurity 下添加 NSAllowsArbitraryLoads 类型Boolean ,值设为 YES;
 