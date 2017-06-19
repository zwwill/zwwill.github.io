---
title: could not find gradle wrapper within android sdk
date: 2017-06-04
tags:
categories: "weex"
description: 
---
当weex项目使用到了第三发插件时，需要执行```weex plugin add ***```添加插件，如果你遇到下面当报错，跟着以下步骤执行或许可以解决你的问题 
> error: could not find gradle wrapper within android sdk. might need to update your android sdk
![](http://upload-images.jianshu.io/upload_images/1494908-3dc2a7dbdd3adcd3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

log中提示，Looked here：/Users/.../tools/templates/gradle/wrapper
CD过去提示```cd: no such file or directory:...wrapper```
依然是环境安装的问题

## 解决
如果你的SDK是同Android Studio一起安装的，那去找到安装目录下的
```
.../plugins/android/lib/templates
#我的是 /Applications/Android\ Studio.app/Contents/plugins/android/lib/templates
```
将该目录（templates）复制到你的SDK目录下的tools目录下
```
.../Android/sdk/tools
#我的 ~/Library/Android/sdk/tools
```
或者直接实用命令
```
cp -r /Applications/Android\ Studio.app/Contents/plugins/android/lib/templates ~/Library/Android/sdk/tools
```

## 第二个问题 Error: spawn EACCES
重新执行```weex plugin add ***```后如果你又遇到了这个问题
![](http://upload-images.jianshu.io/upload_images/1494908-b5eed85b32ecc9d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说明没有权限，执行以下命令，添加下权限就可以了
```
chmod a+x ~/Library/Android/sdk/tools/templates/gradle/wrapper/gradlew
```