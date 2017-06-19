---
title: 封装原生ajax
date: 2016-02-02 20:14:46
tags: "ajax"
---
目使用时手动写的封装，如有不对不足之处还请指出，谢谢
直接上代码吧，没啥说的。
<!-- more -->
```javascript
var _$ajax = (function() {
		/**
		* 生产XHR兼容IE6
		*/
        var createXHR = function(){
            if(typeof XMLHttpRequest != "undefined"){ // 非IE6浏览器
                return new XMLHttpRequest();
            }else if(typeof ActiveXObject != "undefined"){   // IE6浏览器
                var version = [
                    "MSXML2.XMLHttp.6.0",
                    "MSXML2.XMLHttp.3.0",
                    "MSXML2.XMLHttp",
                ];
                for(var i = 0; i < version.length; i++){
                    try{
                        return new ActiveXObject(version[i]);
                    }catch(e){
                        //跳过
                    }
                }
            }else{
                throw new Error("您的系统或浏览器不支持XHR对象！");
            }
        };
		/**
		* 将JSON格式转化为字符串
		*/
        var formatParams = function(data) {
            var arr = [];
            for (var name in data) {
                arr.push(name + "=" + data[name]);
            }
            arr.push("nocache=" + new Date().getTime());
            return arr.join("&");
        };
		/**
		* 字符串转换为JSON对象，兼容IE6
		*/
        var _getJson = (function() {
            var e = function (e) {
                try {
                    return new Function("return " + e)()
                } catch (n) {
                    return null
                }
            };
            return function (n) {
                if ("string" != typeof n)return n;
                try {
                    if (window.JSON && JSON.parse)return JSON.parse(n)
                } catch (t) {
                }
                return e(n)
            };
        })();
		
		/**
		* 回调函数
		*/
        var callBack = function (xhr,options) {
            if (xhr.readyState == 4 && !options.requestDone) {
                var status = xhr.status;
                if (status >= 200 && status < 300) {
                    options.success && options.success(_getJson(xhr.responseText));
                } else {
                    options.error && options.error();
                }
                //清空状态
                this.xhr = null;
                clearTimeout(options.reqTimeout);
            }else if(!options.requestDone){
	            //设置超时
                if(!options.reqTimeout) {
                    options.reqTimeout = setTimeout(function () {
                        options.requestDone = true;
                        !!this.xhr && this.xhr.abort();
                        clearTimeout(options.reqTimeout);
                    }, !options.timeout ? 5000 : options.timeout);
                }
            }
        };
        return function (options) {
            options = options || {};
            options.requestDone = false;
            options.type = (options.type || "GET").toUpperCase();
            options.dataType = options.dataType || "json";
            options.contentType = options.contentType || "application/x-www-form-urlencoded";
            options.async = options.async || true;
            var params = options.data;

            //创建 - 第一步
            var xhr = createXHR();

            //接收 - 第三步
            if(options.async === true) {

                xhr.onreadystatechange = function () {
                    callBack(xhr,options);
                };
            }

            //连接 和 发送 - 第二步
            if (options.type == "GET") {
                params = formatParams(params);
                xhr.open("GET", options.url + "?" + params, options.async);
                xhr.send(null);
            } else if (options.type == "POST") {
                xhr.open("POST", options.url, options.async);
                //设置表单提交时的内容类型
                xhr.setRequestHeader("Content-Type", options.contentType);
                xhr.send(params);
            }
            // 同步
            if(options.async === false){
                callBack(xhr,options);
            }

        }
    })();
```