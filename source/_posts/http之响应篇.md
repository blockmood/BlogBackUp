---
title: http之响应篇
date: 2017-06-21 15:10:24
tags: http
---
一个http响应代表服务器给浏览器回送的数据，同时告诉浏览器该如何处理数据。
### 基本结构
1.状态行
2.消息头信息
3.实体信息

先看一段响应信息
``` bash
//200 ok 表示客户端已经请求成功
HTTP/1.1 200 OK
//告诉浏览器请求页面的时间
Date: Wed, 21 Jun 2017 05:54:22 GMT
//告诉浏览器服务器的情况
Server: Apache/2.4.23 (Win32) OpenSSL/1.0.2j mod_fcgid/2.3.9
//文档类型
Content-Type: text/html; charset=UTF-8
//缓存
Cache-controll:private
//目标间隔时间和重定向跳转地址
Refresh: 3;url=http://localhost/http/2.php


//下面3个共同来控制该页面是否需要被缓存（为什么呢？ 因为浏览器种类有很多种）
Expires:-1
Cache-controll:no-cache
Pragma:no-cache
```
### 状态码
	100 ~ 199  表示成功接收请求，要求客户端继续提交下次请求才能完成整个处理过程
	200 ~ 299  表示成功接收请求并已完成整个过程，常用200
	300 ~ 399  为完成请求，客户需进一步细化请求，例如，请求的资源已经移到一个新的网址，常用304 302
	400 ~ 499  客户端请求有错误，常用404
	500 ~ 599  服务器内部出现错误，常用500

304码一般都是缓存机制。


### PHP禁用缓存
如何通过http响应控制页面缓存，在默认情况下，浏览器会缓存页面。
``` bash
header("Expires:-1");
header("Cache-controll:no-cache");
header("Pragma:no-cache");
```