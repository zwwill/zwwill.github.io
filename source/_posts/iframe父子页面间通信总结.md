---
title: iframe父子页面间通信总结
date: 2015-10-09
tags:
---
随着网页结构的复杂化，页面内嵌frame越来越常见，但不管是作为内容页来显示，还是作为组件模块嵌入，都有父子页面间通信的需求可能，因此为了更好的实现需求就必须了解父子页面间的通信。

iframe子页面与父页面通信根据iframe中src属性是同域链接还是跨域链接，通信方式也不同。

## 同域下父子页面的通信

页面间通信常以页面间调用实现，包括以下几个方面：（调用包含html dom，js全局变量，js方法）

- 父页面调用子iframe页面；
- 子iframe页面调用父页面；
- 主页面内兄弟iframe页面之间相互调用；

*下面我们详细分析*
### 父页面调用子iframe

```

<iframe name="iframeName" id="iframeId" src="child.html"></iframe>

/**
*1、通过iframe的ID获取子页面的dom，然后通过内置属性contentWindow取得子窗口的window对象
*   此方法兼容各个浏览器
*/
document.getElementById('iframeId').contentWindow.func(); 
document.getElementById('iframeId').contentWindow.document.getElementById('子页面中的元素ID');

/**
*2、通过iframe的name（名字）直接获取子窗口的window对象
*/
iframeName.window.func(); 
iframeName.window.document.getElementById('子页面中的元素ID'); 

/**
*3、通过window对象的frames[]数组对象直接获取子frame对象
*/
window.frames[0].func();
window.frames[0].document.getElementById('子页面中的元素ID');
//或
window.frames["iframeName"].func();
window.frames["iframeName"].document.getElementById('子页面中的元素ID');

```

### 子iframe页面调用父页面

```
/**
*通过parent或top对象获取父页面的window对象内元素及方法
*/
parent.window.func(); 
parent.window.document.getElementById('父页面中的元素ID');
//同理
top.window.func(); 
top.window.document.getElementById('父页面中的元素ID');
```
### 页面内兄弟iframe页面相互调用
其实任意页面间通信是类似的，根据上面两个方法组合就可以实现页面内兄弟iframe页面相互调用

> 原理：在子页面中获取父页面（parent）或顶层页面（top）的window元素，然后再根据兄弟iframe的name或id获取兄弟页面的window对象，得到window对象后就可以随意操作了。

```
/*以下为在child1.html页面内访问兄弟frame页面*/
<iframe name="iframe1Name" id="iframe1Id" src="child1.html"></iframe>
<iframe name="iframe2Name" id="iframe2Id" src="child2.html"></iframe>
<iframe name="iframe3Name" id="iframe3Id" src="child3.html"></iframe>

/**
*1、通过兄弟iframe的ID获取其dom，然后通过内置属性contentWindow取得window对象
*   此方法兼容各个浏览器
*/
parent.window.document.getElementById('iframe2Id').contentWindow.func(); 
parent.window.document.getElementById('iframe3Id').contentWindow.document.getElementById('兄弟页面3中的元素ID');

/**
*2、通过iframe的name（名字）直接获取子窗口的window对象
*/
parent.window.iframe2Name.window.func(); 
parent.window.iframe3Name.window.document.getElementById('兄弟页面3中的元素ID'); 

/**
*3、通过window对象的frames[]数组对象直接获取子frame对象
*/
parent.window.frames[1].func();
top.window.frames[2].document.getElementById('兄弟页面3中的元素ID');
//或
parent.window.frames["iframe2Name"].func();
parent.window.frames["iframe3Name"].document.getElementById('兄弟页面3中的元素ID');

```

## 跨域下父子页面的通信

### 父页面向子页面传递数据
如果iframe所链接的是外部页面，因为安全机制就不能使用同域名下的通信方式了。

实现的技巧是利用location对象的hash值，通过它传递通信数据。在父页面设置iframe的src后面多加个data字符串，然后在子页面中通过某种方式能即时的获取到这儿的data就可以了，例如：

1. 在子页面中通过setInterval方法设置定时器，监听location.href的变化即可获得上面的data信息

2. 然后子页面根据这个data信息进行相应的逻辑处理

### 子页面向父页面传递数据

实现技巧就是利用一个代理iframe，它嵌入到子页面中，并且和父页面必须保持是同域，然后通过它充分利用上面第一种通信方式的实现原理就把子页面的数据传递给代理iframe，然后由于代理的iframe和主页面是同域的，所以主页面就可以利用同域的方式获取到这些数据。使用 window.top或者window.parent.parent获取浏览器最顶层window对象的引用。



### 详解
>此段博文转自[http://tid.tenpay.com/?p=4695](http://tid.tenpay.com/?p=4695)感谢作者lyndon

*先来看看哪些情况下才存在跨域的问题*

|编号|URL|说明|是否允许通信|
|:-:|:--:|:--:|:--------:|
|1| http://www.a.com/a.js<br/>http://www.a.com/b.js|同一域名下|允许|
|2| http://www.a.com/a/a.js<br/>http://www.a.com/b/b.js|同一域名下不同文件夹|允许|
|3| http://www.a.com:8000/a.js<br/>http://www.a.com/b.js|同一域名，不同端口|不允许|
|4| http://www.a.com/a.js<br/>https://www.a.com/b.js|同一域名，不同协议|不允许|
|5| http://www.a.com/a.js<br/>http://127.0.0.1/b.js|域名和域名对应ip|不允许|
|6| http://www.a.com/a.js<br/>http://will.a.com/b.js|主域相同，子域不同|不允许|
|7| http://www.a.com/a.js<br/>http://a.com/b.js|同一域名，不同二级域名（同上）|不允许（cookie这种情况下也不允许访问）|
|8| http://www.a.com/a.js<br/>http://www.b.com/b.js|不同域名|不允许|

其中编号6、7两种情况同属于主域名相同的情况，可以设置domain来解决问题，今天就不讨论这种情况了。 对于其他跨域通信的问题，我想又可以分成两类，**其一（第一种情况）**是a.com下面的a.js试图请求b.com下某个接口时产生的跨域问题。**其二（第二种情况）**是当a.com与b.com下面的页面成父子页面关系时试图互相通信时产生的跨域问题，典型的应用场景如a.com/a.html使用iframe内嵌了b.com/b.html，大家都知道a.html内的js脚本试图访问b.html时是会被拒绝的，反之亦然。**第一种情况**，目前主流的方案是JSONP，高版本浏览器支持html5的话，还可以使用XHR2支持跨域通信的新特性。**第二种情况**，目前主要是通过代理页面或者使用postMessageAPI来做，这也是今天要讨论的话题。 第二种情况，有这样一些类似的案例：a.com/a.html使用iframe内嵌了b.com/b.html，现在希望iframe的高度能自动适应b.html的高度，使iframe不要出现滚动条。我们都知道跨域了，a.html是没办法直接读取到b.html的高度的，b.html也没办法把自己的高度告诉a.html。 直接说可以用代理页面的方法搞定这个问题吧，但是怎么代理法，先来看下面这张图：
![这里写图片描述](http://img.blog.csdn.net/20151009182823247)
b.html与a.html是不能直接通信的。我们可以在b.html下面再iframe内嵌一个proxy.html页面，因为这个页面是放在a.com下面的，与a.html同域，所以它其实是可以和a.html直接通信的，假如a.html里面有定义一个方法_callback，在proxy.html可以直接top._callback()调用它。但是b.html本身和proxy.html也是不能直接通信的，所谓代理页面的桥梁作用怎么实现呢? b.html内嵌proxy.html是通过一段类似下面这样的代码： 
```html
<iframe id=”proxy” src=”a.com/proxy.html” name=”proxy” frameborder=”0″ width=”0″ height=”0″></iframe>
``` 
这个iframe的src属性b.html是有权限控制的。如果它把src设置成a.com/proxy.html?args=XXX,也就是给url加一个查询字符串，proxy.html内的js是可以读取到的。对的，这个url的查询字符串就是b.html和proxy.html之间通信的桥梁，美中不足的是每次通信都要重写一次url造成一次网络请求，这有时会对服务器及页面的运行效率产生很大的影响。同时由于参数是通过url来传递的，会有长度和数据类型的限制，搜集的资料显示：

>- IE浏览器对URL的长度现限制为2048字节。 
>- 360极速浏览器对URL的长度限制为2118字节。 
>- Firefox(Browser)对URL的长度限制为65536字节。 
>- Safari(Browser)对URL的长度限制为80000字节。 
>- Opera(Browser)对URL的长度限制为190000字节。 
>- Google(chrome)对URL的长度限制为8182字节。

上面的方法，通过迂回战术实现了b.html跟a.html通信，但是倒过来，a.html怎么跟b.html通信呢?嵌入在b.html里面的proxy.html可以用top快速的联系上a.html，但是要想让a.html找到proxy.html就不容易了，夹在中间的 b.html生生把它们分开了，a.html没法让b.html去找到proxy.html然后返回给它。只能采用更迂回的战术了。 顺着前面b.html到a.html的通信过程，逆向的想一下，虽然a.html没有办法主动找到proxy.html，但是proxy.html可以反过来告诉a.html它在哪里： 在proxy.html加这么一段脚本：

```javascript
var topWin = top;  
function getMessage(data) {  
    alert("messageFormTopWin:" + data);  
}  
function sendMessage(data) {  
    topWin.proxyWin = window;  
    topWin.getMessage(data);  
} 
```
在a.html加这么一段脚本：
```javascript
var proxyWin = null;  
function getMessage(data) {  
   alert("messageFormProxyWin:"+data);  
   sendMessage("top has receive data:"+data);  
}  
function sendMessage(data) {  
    if (null != proxyWin) {  
        proxyWin.getMessage(data);  
    }  
} 
```
也就是必须由proxy.html先主动发送一个消息给a.html，a.html得到proxy.html页面window的引用，就可以反过来向它发送请求了。 现在a.html可以把消息发给proxy.html了，但是proxy.html怎么把消息转送到b.html？似乎这才是难点，因为它们之间才真正有着“跨域”这一道鸿沟。 这回我们不再用前面那个iframe内嵌代理页面的方法再在proxy.html内嵌一个b.com下面的代理页面了，这样实在会给人感觉嵌的太深了，四层。但是为了跨越这道鸿沟，b.com下面也加一个代理页面是免不的。不过现在我们要利用一下window.name。window.name有一个特性，就是页面在同一个浏览器窗口（标签页）中跳转时，它一直存在而且值不会改变。比如我们在a.html中设置了window.name=”a”,然后location.href=”http://b.com/b.html”跳转后，b.html可以读取window.name的值为”a”;而且window.name的值长度一般可以到达2M，ie和firefox甚至可以达到32M，这样的存储容量，足够利用起来做跨域的数据传递了。好吧，我们现在要做的就是当proxy.html拿到a.html发送过来的数据后把这个数据写入window.name中，然后跳转到b.com下面的代理页面，我们这里假设是bproxy.html。bproxy.html读取到window.name值后，通知给它父页面b.html就简单了。我们再来看这个过程可以用图大概示意一下：
![这里写图片描述](http://img.blog.csdn.net/20151009183805363)
图例中绿色的双向箭头表示可以通信，橙色的双向箭头表示不能直接通信。 最后我们简单看一下双向通信的实测效果：
![这里写图片描述](http://img.blog.csdn.net/20151009183823009)
b.html每次加载的时候都先给a.html发一个”连接请求”，让a.html可以找到proxy.html。所以页面第一次加载的时候会产生三个请求：
![这里写图片描述](http://img.blog.csdn.net/20151009183840678)
每次b.html向a.html发送消息的时候会产生一个请求：
![这里写图片描述](http://img.blog.csdn.net/20151009183858566)
每次a.html向b.html发送消息的时候会产生两个请求，其中一个是a.com/proxy.html向b.com/bproxy.html跳转产生的，另一个是b.html重新向a.html发起“连接请求”时产生的：
![这里写图片描述](http://img.blog.csdn.net/20151009183913229)

最后简单看一下实测的几个测试页面代码： 
*代码片段一，a.com/a.html:*
```html
<html xmlns="http://www.w3.org/1999/xhtml">  
<head>  
   <title>a.com</title>  
</head>  
<body>  
    <div id="Div1">  
        A.com/a.html</div>  
    <input id="txt_msg" type="text" />  
    <input id="Button1" type="button" value="向b.com/b.html发送一条消息" onclick="sendMessage(document.getElementById('txt_msg').value)" />  
    <div id="div_msg">  
    </div>  
    <iframe width="800" height="400" id="mainFrame" src="<A href="http://localhost:8091/b.com/b.htm">http://localhost:8091/b.com/b.htm</A>">  
    </iframe>  
    <script type="text/javascript"> 
        var proxyWin = null;  
        function showMsg(msg) {  
            document.getElementById("div_msg").innerHTML = msg;  
        }  
        function getMessage(data) {  
            showMsg("messageForm b.html to ProxyWin:" + data);  
        }  
        function sendMessage(data) {  
            if (null != proxyWin) {  
                proxyWin.getMessage(data);  
            }  
        }  
    </script> 
</body>  
</html> 
```
*代码片段二，a.com/proxy.html:*
```html
<html xmlns="<A href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</A>">
<head>
    <title>a.com</title>
</head>
<body>
<div id="Div1">A.com/proxy.html</div>
<div id="div_msg"></div>
<script type="text/javascript">
    var topWin = top;
    function showMsg(msg) {
        document.getElementById("div_msg").innerHTML = msg;
    }

    function getMessage(data) {
        showMsg("messageForm A.com/a.html:" + data + "<br/>两秒后将跳转到B.com/bproxy.html");
        window.name = data;
        setTimeout(function () { location.href = "<a href="http://localhost:8091/b.com/bproxy.htm">http://localhost:8091/b.com/bproxy.htm</a>" }, 2000);// 为了能让大家看到跳转的过程，所以加了个延时
    }

    function sendMessage(data) {
        topWin.proxyWin = window;
        topWin.getMessage(data);
    }

    var search = location.search.substring(1);
    showMsg("messageForm B.com/b.html:" + search);
    sendMessage(search);
    </script>
</body>
</html>
```
*代码片段三，b.com/b.html*
```html
<html xmlns="<A href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</A>">
<head>
    <title>b.com</title>
</head>
<body>
    <div id="Div1">
        B.com/b.html</div>
    <input id="txt_msg" type="text" />
    <input id="Button1" type="button" value="向A.com/a.html发送一条消息" onclick="sendMessage(document.getElementById('txt_msg').value)" />
    <div id="div_msg">
    </div>
    <iframe id="proxy" name="proxy" style="width: 600px; height: 300px"></iframe>
    <script type="text/javascript">
        function showMsg(msg) {
            document.getElementById("div_msg").innerHTML = msg;
        }
        function sendMessage(data) {
            var proxy = document.getElementById("proxy");
            proxy.src="<A href="http://localhost:8090/a.com/proxy.htm?data">http://localhost:8090/a.com/proxy.htm?data</A>=" + data;
        }
        function connect() {
            sendMessage("connect");
        }
        function getMessage(data) {
            showMsg("messageForm a.html to ProxyWin:" + data);
            connect();
        }

        connect(); // 页面一加载，就执行一次连接
    </script>
</body>
</html>
```
*代码片段四，b.com/bproxy.html*
```html
<html xmlns="<A href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</A>">
<head>
    <title>b.com</title>
</head>
<body>
    <div id="Div1">
        B.com/bproxy.html</div>
    <div id="div_msg">
    </div>
    <script type="text/javascript">
        var parentWin = parent;
        var data = null;

        function getMessage() {
            if (window.name) {
                data = window.name;
                parentWin.getMessage(data);
            }
            document.getElementById("div_msg").innerHTML = "messageForm a.com/proxy.html:" + data;

        }
        getMessage();         
    </script>
</body>
</html>
```
好吧，现在我必须把话锋调转一下了。前面讲的这么多，也只是抛出来一些之前我们可能会采用的跨域通信方法，事实上代理页面、url传参数和window.name、甚至还有一些利用url的hash值的跨域传值方法，都能百度到不少相关资料。但它们都逃不开代理页面，也就不可避免的要产生网络请求，而事实上这并不是我们的本意，我们原本希望它们能够直接在客户端通信，避免不必要的网络请求开销——这些开销，在访问量超大的站点可能会对服务器产生相当大的压力。那么，有没有更完美一点的替代方案呢？ 必须给大家推荐postMessage。postMessage 正是为了满足一些合理的、不同站点之间的内容能在浏览器端进行交互的需求而设计的。利用postMessage API实现跨域通信非常简单，我们直接看一下实例的代码： 
*代码片段五，A.com/a.html：*
```html
<html xmlns="<A href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</A>">
<head runat="server">
    <title>A.com/a.html</title>
    <script type="text/javascript">
        var trustedOrigin = "<A href="http://localhost:8091/">http://localhost:8091</A>";

        function messageHandler(e) {
            if (e.origin == trustedOrigin) {//接收消息的时候，判断消息是否来自可信的源，这个源是否可信则完全看自己的定义了。
                showMsg(e.data);//e.data才是真实要传递的数据
            } else {
                // ignore messages from other origins
            }
        }

        function sendString(s) {//发送消息
            document.getElementById("widget").contentWindow.postMessage(s, trustedOrigin);
        }

        function showMsg(message) {
            document.getElementById("status").innerHTML = message;
        }

        function sendStatus() {
            var statusText = document.getElementById("statusText").value;
            sendString(statusText);
        }

        function loadDemo() {
            addEvent(document.getElementById("sendButton"), "click", sendStatus);
            sendStatus();
        }

        function addEvent(obj, trigger, fun) {
            if (obj.addEventListener) obj.addEventListener(trigger, fun, false);
            else if (obj.attachEvent) obj.attachEvent('on' + trigger, fun);
            else obj['on' + trigger] = fun;
        }
        addEvent(window, "load", loadDemo);
        addEvent(window, "message", messageHandler);

    </script>
</head>
<body>
    <h1>A.com/a.html</h1>
    <p><b>源</b>: <A href="http://localhost:8090</p">http://localhost:8090</p</A>>
    <input type="text" id="statusText" value="msg from a.com/a.html">
    <button id="sendButton">向b.com/b.html发送消息</button>
    <p>接收到来自a.com/a.html的消息: <strong id="status"></strong>.<p>
    <iframe id="widget" width="800" height="400" src="<A href="http://localhost:8091/PostMessage/Default.aspx%22%3E%3C/iframe">http://localhost:8091/PostMessage/Default.aspx"></iframe</A>>
</body>
</html>
```
*代码片段六，B.com/b.html：*
```html
<html xmlns="<A href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</A>">
<head runat="server">
    <title>B.com/b.html</title>
    <script type="text/javascript">
        //检查postMessage 是否可以用：window.postMessage===undefined
        //定义信任的消息源
        var trustedOrigin = "<A href="http://localhost:8090/">http://localhost:8090</A>";
        function messageHandler(e) {
            if (e.origin === "<A href="http://localhost:8090/">http://localhost:8090</A>") {
                showMsg(e.data);
            } else {
                // ignore messages from other origins
            }
        }

        function sendString(s) {
            window.top.postMessage(s, trustedOrigin); //第二个参数是消息传送的目的地
        }

        function loadDemo() {
            addEvent(document.getElementById("actionButton"), "click", function () {
                var messageText = document.getElementById("messageText").value;
                sendString(messageText);
            });
        }

        function showMsg(message) {
            document.getElementById("status").innerHTML = message;
        }

        function addEvent(obj, trigger, fun) {
            if (obj.addEventListener) obj.addEventListener(trigger, fun, false);
            else if (obj.attachEvent) obj.attachEvent('on' + trigger, fun);
            else obj['on' + trigger] = fun;
        }
        addEvent(window, "load", loadDemo);
        addEvent(window, "message", messageHandler);
    </script>
</head>
<body>
    <h1>B.com/b.html</h1>
    <p><b>源</b>: <A href="http://localhost:8091</p">http://localhost:8091</p</A>>
    <p>接收到来自a.com/a.html的消息: <strong id="status"></strong>.<p>
            <div>
                <input type="text" id="messageText" value="msg from b.com/b.html">
                <button id="actionButton"> 向a.com/a.html发送一个消息</button>
            </div>
</body>
</html>
```
代码的关键是message事件是一个拥有data（数据）和origin(来源)属性的DOM事件。data属性是发送的实际数据，origin属性是发送来源。Origin属性很关键，有了这个属性，接收方可以轻易的忽略掉来自不可信源的消息，也就能有效避免跨域通信这个开口给我们的源安全带来的隐患。接口很强大，所以代码很简单。我们可以抓包看一下，这个通信过程完全是在浏览器端的，没有产生任何的网络请求。同时这个接口目前已经得到了绝大多数浏览器的支持，包括IE8及以上版本，参见下面的图表：
![这里写图片描述](http://img.blog.csdn.net/20151009184516692)
但是为了覆盖ie6等低版本浏览器，我们完整的方案里面还是要包含一下兼容代码，就是最开始介绍的代理页面的方法了，但必须是以postMessage为主，这样即便最后会有某些浏览器因为这种通信产生一些网络请求，比例也是非常低的了。
