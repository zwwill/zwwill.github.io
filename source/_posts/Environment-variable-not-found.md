---
title: Environment variable $ANDROID_HOME not found
date: 2017-06-04
tags:
categories: "环境踩坑"
description: 
---
MacOS开发Android app经常会遇到环境的坑，$ANDROID_HOME就是其中之一
![](http://upload-images.jianshu.io/upload_images/1494908-9523c98edad35e24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果碰到这个信号就要注意了，解决这个问题的方式有很多,度娘、狗狗马上告诉你答案，但适合自己的才是最好的，因为方法治标不治本（新启动的terminal由被还原了）

## 解决方法
### SDK在哪？
首先你要知道你的SDK安装在哪里，有几种可能
1、直接从WEB上下载的SDK
```
ANDROID_HOME= .../ADT/sdk
```
如果拖拽到了【应用程序】（Applications）目录下
```
ANDROID_HOME=~/Applications/ADT/sdk
```
2、使用Homebrew (brew install android-sdk)下载
```
ANDROID_HOME=/usr/local/Cellar/android-sdk/{YOUR_SDK_VERSION_NUMBER}
```
3、 随Android Studio下载
```
ANDROID_HOME=/Users/{YOUR_USER_NAME}/Library/Android/sdk
```
### 配置
```
export ANDROID_HOME={YOUR_PATH}
#mine: export ANDROID_HOME=/Users/zwwill/Library/Android/sdk
#or  : export ANDROID_HOME=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
```
或者将以上脚本配置到```~/.bash_profile```文件下，如果没有就新建一个，然后执行
```
source ~/.bash_profile
```