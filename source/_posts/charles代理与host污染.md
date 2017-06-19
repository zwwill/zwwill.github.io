---
title: charles代理与host污染
date: 2017-06-07 
tags:
categories: "环境踩坑"
---
mac谈到代理就不得不说神器charles，虽然是神器也有不方便或者不合理的地方。
## 前言
产品在完成开发后会经过开发测试、冒烟测试、预发布测试，最后才能上线，为了保证测试代码统一，几个环境的代码和配置包括域名必须一致，那么QA在移动端测试时就要配置host、IP共享或者代理，windows上的代理多用Finder，mac下是Charles，正常情况下，我们测试都会很顺利，但有的时候会出现配置和环境的坑，下面详细解释。
## 坑
举个栗子：【前提：MAC+Charles】

.|.  
-|-
项目域名|www.zwwill.com
线上ip | 192.168.1.1
测试ip|192.168.1.2
项目依赖资源域名|st.zwwill.com

> *** 重要前提：www.zwwill.com与st.zwwill.com的线上IP相同，且由ngnix做rewrite定位 ***

由于我们在测试环境时【项目依赖资】需要使用线上的资源，所以测试环境的ip映射关系应为：

域名|IP
-|-
www.zwwill.com|192.168.1.2
st.zwwill.com|192.168.1.1
测试时的host配置为：
![host配置](http://upload-images.jianshu.io/upload_images/1494908-6e87f74d72121f41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在不做charles代理的情况下此坑触及不到，如果开启了charles，坑就出现了。

## 坑的模样
![控制台信息](http://upload-images.jianshu.io/upload_images/1494908-fc97ce166e231bd6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
奇怪的是，我们的test.js（st.zwwill.com/common/test.js）文件404错误！
看下Charles的record信息：

![www.zwwill.com下的文件](http://upload-images.jianshu.io/upload_images/1494908-4708d4e70d9aba40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【www.zwwill.com】指向IP【192.168.1.2】正确！

![st.zwwill.com下的文件](http://upload-images.jianshu.io/upload_images/1494908-39d09a11c33fd747.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【st.zwwill.com】指向IP【192.168.1.2】错误！❌
## 查查原因
检查完log后发现，Charles的代理模式在处理此重情况下会出现误判！导致同源下的资源也走了重定向，这不是我们想要的结果！
## 解决方案很简单
既然Charles没有我们想想的这么智能，那我们就找个方法绕过这个坑。
很简单，只需要将【st.zwwill.com】指向我们的线上IP【192.168.1.1】就好。

![正确的效果](http://upload-images.jianshu.io/upload_images/1494908-88c01f2a9dee9868.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)