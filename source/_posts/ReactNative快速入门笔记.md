---
title: ReactNative快速入门笔记
date: 2017-06-19 20:22:01
tags:
categories: "ReactNative"
description: 
---
![](http://upload-images.jianshu.io/upload_images/1494908-3b99aa61063c171a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> ReactNative的文档地址有多个，如果你英文够好，就去研读[官方的文档](http://facebook.github.io/react-native/docs/getting-started.html)吧，
如果读原文比较吃力，[中文官网](http://reactnative.cn/docs/0.45/getting-started.html)也是不错的选择。

*下面是我个人记录的一些笔记，仅供初学者入门参考*
# 预科
入门React Native前需要了解一下知识，这样能帮助你更快的掌握RN
Node：[Node.js 教程](http://www.runoob.com/nodejs/nodejs-tutorial.html)
ReactJS：[《React 入门实例教程》](http://www.ruanyifeng.com/blog/2015/03/react.html)
ES6：[《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)

# 环境
## 系统环境要求
IOS : ``MacOS``, ``Linux``, ``Windows``
Android : ``Linux``, ``Windows``

## 配置
所有的技术学习都应该从环境搭建开始，这里也没什么好总结的，最好的方法就是跟着[官网指导配置环境](http://reactnative.cn/docs/0.45/getting-started.html#content)
如果你是node的老手，那就直接动手安装以下环境吧：
- node
- npm
- react-native-cli
- Xcode
安装Xcode IDE和Xcode的命令行工具（IOS开发依赖）
- Android Studio
 **下载必须的插件：**
a) JDK1.8+
b) Show Package Details
c) Android SDK Build Tools **（指定23.0.1版本）**
d) Android Support Repository
**配置基础环境：**
a) ANDROID_HOME （如运行是遇到问题可参考此文[http://www.jianshu.com/p/a77396301b22](http://www.jianshu.com/p/a77396301b22)）
b) JAVA_HOME 

## 测试
```
react-native init RNDemo
cd RNDemo
react-native run-ios
```
如果你的虚拟机启动了，那么恭喜你，你的环境已经配置成功！

![虚拟机启动界面](http://upload-images.jianshu.io/upload_images/1494908-279025ab7557c2ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 语法
首先需要了解一些基本的React的概念，比如JSX语法、组件、state状态以及props属性。
还需要掌握一些React Native特有的知识，比如原生组件的使用。
> 教程上的东西我就不多说了，[官方文档](http://reactnative.cn/docs/0.45/getting-started.html)上有详细的讲解

直接从代码上讲解新手注意点吧

## Hello World
传统惯例，入门先行，Hello World

*你可以新建一个项目，然后用上面的代码覆盖你的index.ios.js或是index.android.js 文件，然后运行看看。*
```jsx
import React, { Component } from 'react';
import { AppRegistry, StyleSheet, Text } from 'react-native';
class HelloWorldApp extends Component {
  render() {
    return (
      <Text style={styles.red}>Hello world!</Text>
    );
  }
}
const styles = StyleSheet.create({
  bigblue: {
    color: 'red',
    fontWeight: 'bold',
  }
});
// 注意，这里用引号括起来的'HelloWorldApp'必须和你init创建的项目名一致
AppRegistry.registerComponent('HelloWorldApp', () => HelloWorldApp);
```

从语法上看，RN和ReactJS语法区别不大，都是采用JSX和ES6的形式，如果你对ReactJS和ES6不熟悉，建议你先拜读下阮一峰的博文教程：[《React 入门实例教程》](http://www.ruanyifeng.com/blog/2015/03/react.html)，[《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)

相较写Web App，区别在于RN的语法引入了原生的组件

```jsx
import { AppRegistry, StyleSheet, Text } from 'react-native';
```

RN中虽然使用JS写原生UI，但不再使用常规HTML标签 ``<div>`` 或是 ``<span>`` ，而是使用RN的组件 ``<Text>``
``AppRegistry`` 模块写在index.ios.js或是index.android.js文件里，用来告知React Native哪一个组件被注册为整个应用的根容器，一般一个应用只运行一次。

仅仅使用props和基础的View、Text、Image以及TextInput组件，就足以编写各式各样的UI组件了

## 样式
按照JSX的语法要求使用了驼峰命名法：
- font-weight -> fontWeight
- background-color -> backgroundColor

React Native中的尺寸都是无单位的，表示的是与设备像素密度无关的逻辑像素点：

```html
<View style={{width: 50, height: 50, backgroundColor: 'powderblue'}} />
```

## 事件
事件的注册跟ReactJS没什么区别
```jsx
class MyButton extends Component {
  _onPressButton() {
    console.log("You tapped the button!");
  }

  render() {
    return (
      <TouchableHighlight onPress={this._onPressButton}>
        <Text>Button</Text>
      </TouchableHighlight>
    );
  }
}
```
此处注册的组件为```TouchableHighlight```，具体使用哪种组件，取决于你希望给用户什么样的视觉反馈
- 一般来说，你可以使用[**TouchableHighlight**](http://reactnative.cn/docs/0.45/touchablehighlight.html)来制作按钮或者链接。注意此组件的背景会在用户手指按下时变暗。
- 在Android上还可以使用[**TouchableNativeFeedback**](http://reactnative.cn/docs/0.45/touchablenativefeedback.html)，它会在用户手指按下时形成类似墨水涟漪的视觉效果。
- [**TouchableOpacity**](http://reactnative.cn/docs/0.45/touchableopacity.html)会在用户手指按下时降低按钮的透明度，而不会改变背景的颜色。
- 如果你想在处理点击事件的同时不显示任何视觉反馈，则需要使用[**TouchableWithoutFeedback**](http://reactnative.cn/docs/0.45/touchablewithoutfeedback.html)。

常用的事件有：
点击：onPress
长按：onLongPress
缩放：maximumZoomScale，minimumZoomScale


> 另外关于Props、State、样式、布局、事件等知识点的详解，[官方文档](http://reactnative.cn/docs/0.45/getting-started.html)上都有详细的讲解，比较基础，这里就不做介绍了

# 跨平台
 > 'Learn Once,Write Anywhere' and not 'Write Once,Running Anywhere'.

RN并不能算上是真正的跨平台的语言，虽然可以通过打包实现不同平台打包不同组件，但是有些组件需要我们针对不同平台编写不同代码。这就要求我们不用储备一些原生开发的知识。

# 工作原理
![通信示意图](http://upload-images.jianshu.io/upload_images/1494908-35bfd488c9f68fd7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
RN的本质是在两个模块之间搭建双向桥梁，让他们可以相互调用和响应，简单的示意图为
![](http://upload-images.jianshu.io/upload_images/1494908-21daeeddecceb4eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## Native模块
运行在主线程上(可能会有些独立的后台线程处理运算，当前讨论中可忽略) 
iOS平台上运行Object-C/Swift代码，Android平台上运行Java/Kotlin代码 
负责处理UI的渲染，事件响应。
## JS模块
运行在JS引擎的JS线程上 
运行JS代码 
负责处理业务逻辑，还包括了应该显示哪个界面，以及如何给页面加样式。
## Bridge模块
Native和JS模块之间不能直接通信，只能通过Bridge做序列化和反序列化，查找模块，调用模块等各种逻辑，最终反应到应用上

# 性能
使用React Native替代基于WebView的框架，使App刷新可以达到每秒60帧（足够流畅），并且能有类似原生App的外观和手感，虽然RN框架已经提供了这个平衡的能力，但平衡点的选择却掌握在开发者手中，即便是Native也无法避免开发方式带来的性能消耗

## 性能影响原因
业务逻辑运行在JS线程上，负责API的调用，事件的处理，状态的更新，而事件的响应UI的变化发生在主线程上，60帧/s的频率要求每一帧的响应处理只有16.67(1000/60)ms，如果超过了16.67ms就会发生丢帧，如果丢帧超过100ms就会产生明显的卡顿现象。所有降低每一帧运算的消耗才能提升性能。

## 性能影响切面
**UI事件响应：** 性能影响小
**UI更新：** JS侧会向Native侧同步大量的UI结构和数据，界面复杂、变动数据大，或者做动画、变动频繁，容易出现性能问题。
**UI事件响应和UI更新同时出现：** 两种事件如果占用了过多的线程，就会导致另一种事件不能及时响应，表现在应用上就是卡顿

## 常见影响性能的点
console，ListView，动画Animated

# 性能优化
经过多年的发展和优化，JS和Native可以在各自的模块线程高效迅速的运行，性能的瓶颈主要在Bridge模块上，尤其是在JS和Native模块间频繁的调用会导致Bridge压力过大，产生卡顿

1. 利用React自带的Virtual Dom的Diff算法尽量减少需要同步的数据，**合理利用setState方法**
2. 在遇到动画性能问题时，可以**使用Annimated类的库**，一次性把如何变化的声明发送到Native侧，Native侧根据接收到的声明自己负责接下来的UI更新。不需要每帧的UI变化都同步一次数据。
3. Native和JS混编，把会大量变化的组件做成**Native组件**
4. 遇到UI事件响应和UI更新同时，可以使用**Interaction Manager**把那些耗时较长的**工作安排到所有互动或动画完成之后再进行**

# App高性能开发引导
RN的开发并没有一种高质量产出的方法，因为各个项目间有着不同的组件组合，因此只能通过高效的开发方式来尽可能的优化应用。
一般来说，通过几版优化都能达到“极致体验”的要求。
下面列一下高效开发方式的流水：
1. **全JS实现**，保证开发的高效率，高产出
2. 发现问题**先在JS测做优化**，如上面提到的Annimated类库，Interaction Manager。
3. 真机测试，找全问题再做处理，**避免出现连锁bug**
4. JS测解决不了的问题再有**Native组件**完成。

# 社区
RN同ReactJS一样，有着强大的社区，从RN版本更新的速度上就可以看出来
![发布序列表](http://upload-images.jianshu.io/upload_images/1494908-fbf4009e6abf2183.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
平均2个月一个版本

google的搜索结果也能说明RN的影响力

![google搜索结果](http://upload-images.jianshu.io/upload_images/1494908-4756846d58fa5844.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开发者需要用到的组件在JS.Coach基本都可以找到。
![image.png](http://upload-images.jianshu.io/upload_images/1494908-22adf87fe0d89395.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## RN in China
在中国有着专门的组织来维护RN的中文官网。

独家发布了面向React Native提供热更新功能的组件，Pushy

Pushy的特点：
1.  命令行工具&网页双端管理，版本发布过程简单便捷，完全可以集成CI。
2. 基于bsdiff算法创建的超小更新包，通常版本迭代后在1-10KB之间，避免数百KB的流量消耗。
3. 支持崩溃回滚，安全可靠。
4. meta信息及开放API，提供更高扩展性。
5. 跨越多个版本进行更新时，只需要下载一个更新包，不需要逐版本依次更新。

# 参考&分享
- [ReactNative 官方网站：http://reactnative.com](http://reactnative.com)
- [ReactNative 中文官方网站：http://reactnative.cn](http://reactnative.cn)
- [React Native性能和效率平衡之谜：http://zhuanlan.51cto.com/art/201704/537115.htm](http://zhuanlan.51cto.com/art/201704/537115.htm)
- [React Native通信机制详解：http://blog.cnbang.net/tech/2698/](http://blog.cnbang.net/tech/2698/)
- [React Native 从入门到原理：http://www.jianshu.com/p/978c4bd3a759](http://www.jianshu.com/p/978c4bd3a759)
- [React-Native学习指南：http://www.jianshu.com/p/fd4591a978ba](http://www.jianshu.com/p/fd4591a978ba)
- [【简书专题】React Native开发经验集：http://www.jianshu.com/c/45054b9e38c7](http://www.jianshu.com/c/45054b9e38c7)