---
title: 首次为开源项目添加Travis Ci的徽章
date: 2017-09-29 16:09:44
categories: 教程
tags: [Markdown,Android]
---

## Travis Ci有什么用?
   我经常一直看到一些很强的开源项目里,都带着build passing这个徽章,至于build passing这个如何获得呢,这就需要Travis Ci这个持续集成的东西。Travios CI即持续集成系统。对个人而言，就是让你的代码在提交到远程——这里是GitHub——后，立即自动编译，并且在失败后可以自动给你发邮件的东西。当然，除了编译，还能做自动化测试、自动部署等。对团队或企业而言，这意味着更多的东西，是敏捷开发的一种践行。

## 注册项目
   登录 https://travis-ci.org/ 这个网站,首先你必须要有Github帐号,因为待会要获取你的Github里面的项目,点右上角acounnts,然后会要求同步github的项目,接着来到这个界面
![ci1.ng](/images/ci1.png)
   如上图,打开你需要同步的项目,AndroidResideMenu这种状态就处于打开了,你项目每次提交它都会检测了！

## 添加.travis.yml
   在你的项目工程中,新建一个.travis.yml,添加一下代码
```

language: android
jdk: oraclejdk8
android:
  components:
    - platform-tools
    - tools
    - build-tools-23.0.1
    - android-22
    - extra-android-support
    - extra-android-m2repository


script:
    - cd library
  
```
   以上代码,其实我自己也不太明白,但是android-22,build-tools,tools那些大家应该大概猜到什么意思吧,下面那个cd library这个有没有必要实际也没测试过,因为我的开源库是在项目根目录的library,不过之前我没有那东西好像一直是build failing。

   网上很多的教程,都没有说如何去查看为什么会build failing,因为我自己也是一直build failing,代码编译肯定是没有问题的,出问题的就是.travis.yml配置有问题!!那么你会问:那我怎么知道.travis.yml哪里有问题呢,看图说话！
![ci2.png](/images/ci2.png)
   上面提示好像是我没有那个gradlew文件吧,确实我gradlew没有那个文件。这个页面是在图1点击项目进去后那个页面里的,或许你会觉得我说这个有点多余,但是我相信很多人刚开始都不知道如何解决build failed!

## 徽章图标获取
![ci3.ng](/images/ci3.png)
点击那个build passing就出来这个界面了,就直接copy markdown的代码!

## 提交.travis.yml到远程仓库
   
   把代码添加完了,push上去后,它自动会帮你编译,结果无非就是成功和失败。如果失败的话,第一,要确定自己项目有没有问题,第二,就是.travis.yml的配置问题了。
   基本上三个步骤,就实现流行的build passing徽章!
