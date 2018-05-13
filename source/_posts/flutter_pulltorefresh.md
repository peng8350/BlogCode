---
title: flutter下拉上拉组件
date: 2018-05-08 23:37:57
categories: 作品
tags: [Flutter,Dart,跨平台]

---

# flutter下拉上拉组件轮子

## 简介
一个开源的下拉刷新+上拉加载组件,What?又是下拉刷新,是在重复造轮子吗?不!如果你搜索flutter里的下拉刷新组件,很难有相当好的轮子。进入正题,该组件是在外部进行封装,几乎适合所有容器,例如:listView,gridView,Container...等等.

<!-- more -->


## 为什么写这个?
因为flutter现在组件真的太少了,我想找一个下拉刷新的组件很难,很多都不满足我的需求,要不缺少UI,要不就扩展性很低限制性高。所以为了写项目第一步,没有一个好的下拉刷新组件真的不能写项目- -。

## 老板求个GIF爽下?
IOS:
![](/images/flutter/flutter_pulltorefresh1.gif)
Android:
![](/images/flutter/flutter_pulltorefresh2.gif)


## 如何实现?
一开始其实我打算用它提供的那个Size的动画,通过对高度改变来实现头部和尾部,但中途我发现这种想法不行,因为这个动画只能沿着顶部坐标来缩放，不能沿着底部坐标为原点来缩放。所以导致Revert过去了。后来也想了很多很多动画,偏移动画也试过,还是无果。最后,决定用的方法也是要使用到Size动画,不过有点不同的是这里头部事实上有两个部件在那,一个是真正的头部组件,一个是占位把头部控件压上去的占位View,这个占位View要配合Size的动画改变大小来使头部组件能在刷新状态里面悬浮一定距离。这样就可以实现下拉上拉，并且利用IOS的弹性也是相当吊,在外部组件封装扩展性也很高。
框图:<br>
![](/images/flutter/flutter_pulltorefresh3.png)

## 感想
实现的过程中,遇到很多很多坑爹的地方。第一,flutter不允许你在build方法里获取子组件的高度,其实好像react native也是,因为你界面还没被渲染出来,肯定没法知道里面高度,并且没有提供一个渲染完成的回调方法,像Android里的oncreate,这个问题很多人在它的issue起码提过5,6个帖子。第二,滚动的监听方法提供有点不完善,具体表现在很多地方。第三,布局控件设计得有点复杂,大家都知道,Android布局最常用五种对吧?Relative,Linear,Table...等等,但你知道flutter提供了多少种给我们吗?Row,Column,Center,Align,Flow,Container,Stack....等等数不清,它就是把控件的属性分割为控件了,并且这也是大众吐槽flutter代码可读性的原因,导致学习成本很高。

## 附地址
![Github](https://github.com/peng8350/flutter_pulltorefresh)

