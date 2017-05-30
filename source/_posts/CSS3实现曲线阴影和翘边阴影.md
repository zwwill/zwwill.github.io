---
title: CSS3实现曲线阴影和翘边阴影
date: 2016-10-10 08:00:00
tags: "css"
---
> 往往我们在做一些特殊的阴影效果的时候，使用图片做背景的方式实际上是很差劲的，在不考虑适配老版本浏览器的前提下，我们可以使用css3的自定义出自己想要的阴影风格。

下面将以曲线阴影和翘边阴影为例，讲解自定义阴影的过程。
先来看下效果图
![这里写图片描述](http://img.blog.csdn.net/20161010191703289)

##曲线阴影
曲线阴影其实可以使用双层阴影重叠的方式实现
我们将取消阴影的图片做下分解应该会更容易理解，示意图如下：
![这里写图片描述](http://img.blog.csdn.net/20161010192148622)
![这里写图片描述](http://img.blog.csdn.net/20161010192155873)
如上图，图1基础的阴影下，在加一个有弧度的阴影即可。
1、图1基础阴影很容易实现，内阴影+外阴影
```
.box-shadow1{
	box-shadow: 0 1px 4px rgba(0,0,0,0.3),0 0 40px rgba(0,0,0,0.1) inset;
}
```
2、然后使用伪类在元素的后面添加一个“可适配”的阴影，为了可适配，我们就要使用相对定位，实现代码如下
```
.box-shadow1{
	position:relative;
	box-shadow: 0 1px 4px rgba(0,0,0,0.3),0 0 40px rgba(0,0,0,0.1) inset;
}
.box-shadow1:after{
	content:"";
	position:absolute;
	background:transparent;
	top:50%; //设置宽高仅仅设置上下左右边距是为了让系统自动定位。
	bottom: 1px;
	left: 10px;
	right: 10px; 
	z-index: -1; //将副阴影置于主阴影下
	box-shadow: 0 0 20px rgba(0,0,0,0.7); 
	border-radius: 100px/10px;
}
```
如此即实现了曲线阴影的效果。
```
<div class="box box-shadow1">将box-shadow1作为类使用即可</div>
```

##翘边阴影
同理，翘边阴影可以在基础阴影下叠加两个平行四边形即可。
这里就直接上分解图和源码了，不再做讲解
![这里写图片描述](http://img.blog.csdn.net/20161010192148622)
![这里写图片描述](http://img.blog.csdn.net/20161010194338026)
![这里写图片描述](http://img.blog.csdn.net/20161010194354866)
![这里写图片描述](http://img.blog.csdn.net/20161010194405741)

```
.box-shadow2{
	position:relative;
	box-shadow: 0 1px 4px rgba(0,0,0,0.3),0 0 40px rgba(0,0,0,0.1) inset;
}
.box-shadow2:before,.box-shadow2:after{
	content: "";
	position:absolute; 
	top:20px;bottom: 22px;  
	background: transparent; 
	box-shadow: 0 8px 20px rgba(0,0,0,0.7);  
	z-index: -1; 
	background: #fff; 
}
.box-shadow2:before{ 
	left: 22px;  
	right:12px; 
	transform: skew(-12deg) rotate(-4deg); 
}
.box-shadow2:after{  
	left: 12px;  
	right:22px; 
	transform: skew(12deg) rotate(4deg); 
}
```