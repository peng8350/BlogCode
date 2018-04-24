---
title: Flutter第一个坑
date: 2018-4-24 19:23:43
categories: 爬坑
tags: [Flutter,Dart,跨平台]
---

## 简介
先说下这个框架吧,总体上感觉不错,性能总体上测试过了,加载大量图片和数据的情况性能绝对完爆React native,并且语法我也一看就觉得很熟悉,dart这个语言对于熟悉Android开发的人来说很容易上手,这挺像是Java和javascript的结合体,基本不用看语法也可以入门,并且有react native的状态这个概念更容易理解,总的来说,如果给我react native和flutter选一个来开发大型App的话,毫无疑问,我肯定是选Flutter,最重要的是性能,最重要的是性能,最重要的是性能!!

<!-- more -->

## 掉了什么坑?
不吹了,该时候进入正题了,其实本来我是想找一下如何实现Toast的,结果百度这东西很多都找不到的,毕竟这个没有RN那么火吧,很少人去爬坑。看文档,没有Toast概念提及到好像,但也有SnackBar这个Material的控件是实现Toast,虽然我没用过这个,但以前起码知道一点它要干嘛的。看文档,就那么几行代码实现。

```

    Scaffold.of(context).showSnackBar(new SnackBar(
      content: new Text("测试Toast"),
    ));

```

等等,你以为真的那么简单吗?运行点击没反应,显示下面的异常

```

Another exception was thrown: Scaffold.of() called with a context that does not contain a Scaffold

```

它是说我尝试去通过Context来获取Scaffold这个东西时找不到, Scaffold相当于布局结构的东西,可能是通过这个东西来确定Toast到底要摆放在哪个位置上。问题来了,为什么找不到?

```

  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      color: const Color.fromARGB(255, 255, 255, 255),
      home: new Scaffold(
          backgroundColor: const Color.fromRGBO(255, 255, 255, 1.0),
          appBar: new CupertinoNavigationBar(
              middle: new GestureDetector(
                  onTap: () {
                  	//这里就是展开Toast语句,只不过被我封装了
                    SnackUtils.show(context);
                  },
                  child: new Text('we'))),
          body: new GestureDetector(
            onTap: () {
              SnackUtils.show(context);
            },
            child: new Text('eewe'),
          )),
	.....

```

## 解决方法
最后,我是通过把body里的控件再次封装到另外一个类,然后这样就解决了这个问题

```

  home: new Scaffold(
          backgroundColor: const Color.fromRGBO(255, 255, 255, 1.0),
          appBar: new CupertinoNavigationBar(
              middle: new GestureDetector(
                  onTap: () {
                    SnackUtils.show(context);
                  },
                  child: new Text('we'))),
          body: new MyHomePage(),
          ),
   
   .........
   
   class MyHomePage extends StatelessWidget {
   @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new GestureDetector(
      onTap: () {
        SnackUtils.show(context);
      },
      child: new Text('eewe'),
    );
  }
}
```

## 分析
感觉这是一个BUG,毕竟哪有这么麻烦要我们开多一个类去写Toast呢对吧。
