---
title: ReactNative入坑记-初始化项目遇到的错误
date: 2018-03-22 23:28:33
categories: 爬坑
tags: [ReactNative,Mac OS]
---

# 问题一

 <!--more-->

```
error An unexpected error occurred: "/Users/xxxx/Library/Caches/Yarn/v1/npm-strip-ansi-4.0.0-
a8479022eb1ac368a871389b635262c505ee368f/.yarn-metadata.json: Unexpected end of JSON input".

Error: Command failed: yarn add react-native --exact
    at checkExecSyncError (child_process.js:575:11)
    at execSync (child_process.js:612:13)
    at run (/usr/local/lib/node_modules/react-native-cli/index.js:294:5)
    at createProject (/usr/local/lib/node_modules/react-native-cli/index.js:249:3)
    at /usr/local/lib/node_modules/react-native-cli/index.js:217:7
    at /usr/local/lib/node_modules/react-native-cli/node_modules/prompt/lib/prompt.js:316:32
    at /usr/local/lib/node_modules/react-native-cli/node_modules/async/lib/async.js:142:25
    at assembler (/usr/local/lib/node_modules/react-native-cli/node_modules/prompt/lib/prompt.js:313:9)
    at /usr/local/lib/node_modules/react-native-cli/node_modules/prompt/lib/prompt.js:322:32
    at /usr/local/lib/node_modules/react-native-cli/node_modules/prompt/lib/prompt.js:597:5
  status: 1,
  signal: null,
  output: [ null, null, null ],
  pid: 1380,
  stdout: null,
  stderr: null }
Command `yarn add react-native --exact` failed.


```

<p>如上是我执行react-native init 时遇到的一个错误,之前是没有问题的,不知道为啥突然之间不行了- -!

### 解决方案一

```
npm config set registry https://registry.npm.taobao.org
npm config set disturl https://npm.taobao.org/dist


```
这个解决方法事实上对我一点作用也没有,不过绝大数人说的就是这个解决方法,不过就是对我这情况无效呗

### 解决方案二
``` 
rm -rf /usr/local/bin/yarn

```
这个方法就成功了

