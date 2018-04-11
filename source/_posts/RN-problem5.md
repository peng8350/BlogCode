---
title: ReactNative官方bug->onEndReached被多次回调在ios上
date: 2018-04-11 20:11:24
categories: 爬坑
tags: [ReactNative,Mac OS]
---

# onEndReached多次频繁调用bug

## 起因
首先呢,这个bug经过我大量的网上查阅,并不是因为我自己的代码问题所造成的多次回调的bug。
而且经测试,Android平台上不存在这个多次回调的问题,只有ios。
<!-- more -->
## 分析
如果只在ios上发生这问题的话,我怀疑90%是因为ios弹性listview对高度的错误判断引起多次回调。

## 尝试的解决方法:
<p>1.有个人说把header:'100%'?,然而并没卵用

2.

```

   <FlatList
          data={this.props.data}
          onEndReached={...}
          onEndReachedThreshold={0.5}
          ...
          onMomentumScrollBegin={() => { this.onEndReachedCalledDuringMomentum = false; }}
        />
 ....
 
    
      onEndReached = () => {
    if (!this.onEndReachedCalledDuringMomentum) {
      this.props.fetchData();
      this.onEndReachedCalledDuringMomentum = true;
    }
  };    
        

```

<p>3.这个是我自己想出来的办法,就是定义一个flag,一旦onEndReached被毁掉然后判断

```

if(!this.flag){
	this.flag = true;
	this.props._onLoadMore(() => {
	    this.flag = false
	}
	)
}

...
//After the state callback
this.setState({
	...
},() => {
   this.flag = false;
}
)


```

<p>之前我犯了一个错误,没有考虑到setState是一个异步函数回调,所以我在post获取到数据的同时,直接把flag更新了,这样会导致还没把数据加载到flatlist,再次拖动就会再次回调onEndReached,所以唯有再setState调用完以后,再把flag设置回原来的,就解决这种bug了

## 说明
这个bug是上一年就有的了,现在最新版本也没有修复这个问题,至于为啥不修复,要问FB官方!



