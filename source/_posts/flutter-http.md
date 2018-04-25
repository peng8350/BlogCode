---
title: flutter-获取网络数据两种方法
date: 2018-04-25 23:04:17
categories: 教程
tags: [Flutter,Dart,跨平台]
---



## 总结
首先,flutter加载网络所用到的模式和React Native的思想有点相似,React Native加载网络用到javascript ES6里的一个叫Promise的东西,Promise简单来说,就是一个异步链式调用的东西。而Flutter也是一样的,但不是Promise，是叫Future,就是一个改了名字的东西而已。他们的目的都是一样的。那在Flutter里,如何加载网络呢?这里,我借干货集中营为例子

<!-- more -->

## 方案一
第一种方法就是利用dart里http库(不采用链式写法)

```
  
 http.Response response =
    await http.get("http://gank.io/api/day/2015/08/07");
    final resJson = json.decode(response.body)['results']['Android'];
    List<Gank> list = [];
    for (var item in resJson) {
      
      list.add(new Gank.fromJson(item));
    }

```

## 方案二
第二种就是通过io库里的httpClient(链式写法),上面其实也可以和下面这种那样的写法

```
	
 httpClient.getUrl(url).then((HttpClientRequest request){
      return request.close();//关闭连接并返回一个带response的future对象
    }).then((HttpClientResponse response){
      return response.transform(utf8.decoder).join();
    }).then(
        (data) {
	  //data 就是对应第一种方案的response.body
          var responJson = json.decode(data)['results']['Android'];
          this.setState((){
            for(final item in responJson){
              data2.add(new Gank.fromJson(item));
            }
          });

        }
    );

```

