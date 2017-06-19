---
title: Native、Web App、Hybrid、ReactNative和WEEX
date: 2017-06-08
tags:
categories: "移动开发"
description: 
---
![](http://upload-images.jianshu.io/upload_images/1494908-6de32b291475c560.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 一句话概要
> Native、Web App、Hybrid、ReactNative（后面以RN简称）、WEEX间的异同点

# APP常用开发模式【简介】
此处APP为应用，application，并非我们通常讲的手机App
[常用的几种APP开发模式-脑图](http://naotu.baidu.com/file/6af15fcbb72f89926043779811b1ea44?token=df0378691ecdcef2)

## Native App
传统的原生APP开发模式，有IOS和AOS两大系统，需要各自语言开发各自APP。

**优点**：性能和体验都是最好的
**缺点**：开发和发布成本高
**举个栗子**：[网易管家APP](https://id.163.com/gj/) （Tab1，Tab2）
**应用技术**：jQuery，NEJ

## WebApp
移动端的网站，常被称为H5应用，说白了就是特定运行在移动端浏览器上的网站应用。一般泛指 SPA(Single Page Application)模式开发出的网站，与MPA（Multi-page Application，再后面做介绍）对应。

**优点**：开发和发布成本最低
**缺点**：性能和体验不能讲是最差的，但也受到浏览器处理能力的限制，多次下载同样会占用用户一定的流量
**举个栗子**：[网易管家APP](https://id.163.com/gj/)（Tab3）
**应用技术**：ReactJS，RegularJS

## Hybrid App
混合模式移动应用，介于web-app、native-app这两者之间的app，兼具“Native App良好交互体验的优势”和“Web App跨平台开发的优势”（百度百科解释）

主要的原理是，由Native通过JSBridge等方法提供统一的API，然后用HTML+CSS实现界面，JS来写逻辑，调用API，最终的页面在webview中显示，这种模式下，Android、iOS的API一般有一致性，Hybrid App所有有跨平台效果。

**优点**：开发和发布都比较方便，效率介于Native App、Web App之间
**缺点**：学习范围较广，需要原生配合
**举个栗子**：[网易云音乐](https://music.163.com/#/download)，我爱我家App
**应用技术**：PhoneGap，AppCan，Wex5

## React Native App 
Facebook发现Hybrid App存在很多缺陷和不足，于是发起开源的一套新的APP开发方案RN App。使用H5语言写原生界面，js通过JSBridge调用底层API。

**优点**：效率体验接近Native App，发布和开发成本低于Native App
**缺点**：学习有一定成本，且文档较少，免不了踩坑
**举个栗子**：Facebook、youtube、discord、QQ、百度等等

## WEEX App 
阿里巴巴开发团队在RN的成功案例上，重新设计出的一套开发模式，站在了巨人肩膀上并有淘宝团队项目做养料，广受关注，2016年4月正式开源，并在v2.0版本官方支持Vue.js，与RN分庭抗礼。

**优点**：开发效率和体验上跟RN不相上下，并且跨平台性更强
**缺点**：刚刚起步，社区没有RN活跃
**举个栗子**：淘宝、天猫、饿了么等

# 继续剖析
## Native App
![](http://upload-images.jianshu.io/upload_images/1494908-70fa348f3db0fca7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Native App是一种基于智能手机本地操作系统如iOS、Android、WP并使用原生程式编写运行的第三方应用程序,也叫本地app。一般使用的开发语言为JAVA、C++、Objective-C。

自iOS和Android这两个的手机操作系统发布以来，在互联网界从此就多了一个新的名词：App意为运行在智能的移动终端设备第三方应用程序)。

Native App因为位于平台层上方，向下访问和兼容的能力会比较好一些，可以支持在线或离线，消息推送或本地资源访问，摄像拨号功能的调取。但是由于设备碎片化，App的开发成本要高很多，维持多个版本的更新升级比较麻烦，用户的安装门槛也比较高。但是比较乐观的是，AppStore培养了一种比较好的用户付费模式，所以在Apple的生态圈里，开发者的盈利模式是一种明朗状态，其他market也在往这条路上靠拢。

### 优势
1、相比于其它模式，提供最佳的用户体验，最优质的用户界面，最华丽的交互
2、针对不同平台提供不同体验
3、可节省带宽成本，打开速度更快
4、功能最为强大,特别是在与系统交互中,几乎所有功能都能实现
### 劣势
1、门槛高，原生开发人才稀缺，至少比前端和后端少，开发环境昂贵
2、无法跨平台，开发的成本比较大，各个系统独立开发
3、发布成本高，需要通过store或market的审核，导致更新缓慢
4、维持多个版本、多个系统的成本比较高，而且必须做兼容
5、应用市场逐渐饱和，怎么样抢占用户时间需要投入大量时间和金钱，这也导致“僵尸”App的增多

## WebApp
![](http://upload-images.jianshu.io/upload_images/1494908-a16fa91905d4214e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
说到Web App不少人会联想到WAP，或者有人认为，WAP就是WebApp，其实不然。

WebApp与WAP最直接的区别就是功能层面。WAP更侧重使用网页技术在移动端做展示，包括文字、媒体文件等。而Web App更侧重“功能”，是使用网页技术实现的App。总的来说，Web App就是运行于网络和标准浏览器上，基于网页技术开发实现特定功能的应用。

响应式的大部分技术都是为实现WebApp能适配多类客户端而设计的。

Web网站一般分两种，MPA(Multi-page Application)和SPA(Single-page Application)。而WebApp一般泛指SPA形式开发出的网站。这样更像是一个App。


### 优势
1、可以跨平台，调试方便
2、无需安装，不会占用手机内存，而且更新速度最快
3、不存在多版本问题，维护成本低
4、临时入口，可以随意嵌入
### 劣势
1、依赖于网络，第一次访问页面速度慢，耗费流量
2、受限于手机和浏览器性能，用户体验相较于其他模式最差
3、功能受限，大量移动端功能无法实现
4、入口强依赖于第三方浏览器，且只能以URL地址的形式存在，导致用户留存率低（优点即缺点）

## Hybird App
![](http://upload-images.jianshu.io/upload_images/1494908-e8ed4f0375d2639b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
混合开发，也就是半原生半Web的开发模式，由原生提供统一的API给JS调用，实际的主要逻辑有Html和JS来完成，最终是放在webview中显示的，所以只需要写一套代码即可达到跨平台效果，另外也可以直接在浏览器中调试，很方便。最重要的是只需要一个前端人员稍微学习下JS api的调用即可。

Hybird App 的较早实践者是PhoneGap，随后遍地开花，如Titanium、Salama、WeX5、Kerkee和国内的AppCan，项目各有各的实现方式，大致的原理基本相同。有幸在AppCan上海总部参与过一段时间的学习研究，如下大致简介：

AppCan是基于HTML5技术的Hybird跨平台移动应用开发工具。开发者利用HTML5+CSS3+JavaScript技术，通过AppCan IDE集成开发系统、云端打包器等，快速开发出Android、iOS、WP平台上的移动应用。

AppCan的平台构成
![](http://upload-images.jianshu.io/upload_images/1494908-cd8c63378a612022.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在实际的APP开发中，AppCan可以完成大部分的工作量，如图示：
![](http://upload-images.jianshu.io/upload_images/1494908-634591b2598fa8ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

AppCan将APP底层复杂的原生功能封装在引擎、插件中，开发者仅需调用接口、打包编译，就可以获得原生功能；灵活的插件扩展机制。

开发者可以像开发WebApp一样开发app的视觉UI，以及绝大部分的交互，当需要使用原生功能（如摄像头，陀螺仪等功能）时，只需要调用官方的API就可以轻松实现Native的效果。至于JS和Native的通信，常用的有URL监听和绝大部分Hybrid厂商使用的JSBridge通信，两者原理相近。

![JsBridge通信简图](http://upload-images.jianshu.io/upload_images/1494908-400b4c6162d774f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

关于JsBridge的原理详解，可见[http://blog.csdn.net/xiangzhihong8/article/details/66970600](http://blog.csdn.net/xiangzhihong8/article/details/66970600)

在Hybird概念盛行的时候，国内外各大公司也参与了探索，国外代表有Facebook、google、亚马逊，国内的有腾讯、阿里巴巴、网易等，慢慢的他们发现Hybird严重受限于WebView的解析渲染效率，于是Facebook开始了他的类原生的研究探索。

## React Native App 
![](http://upload-images.jianshu.io/upload_images/1494908-846d1bcbe4f9b505.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### RN简介
RN的设计理念是“Learn Once ,Write Anywhere”，

## WEEX App 
![](http://upload-images.jianshu.io/upload_images/1494908-ec1e7c9c4de9f9c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)