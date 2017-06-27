---
title: http之请求篇
date: 2017-06-21 09:10:24
tags: http
---
一个http请求，是由三部分组成的，
1、请求行
2、消息头
3、实体内容

先来看一段请求
``` bash
//表示发送的是get请求，请求的资源是1.php
GET /http/1.php HTTP/1.1
//表示客户端可以接收哪些数据类型
Accept: text/html, application/xhtml+xml, */*
//表示 我是从哪里来的（可以做防盗链，很有用，一般是连接过来才有）
Referer: http://localhost/http/1.php
//页面可以支持什么语言
Accept-Language: zh-CN
//告诉服务器我的浏览器的版本
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
//表示接收什么样的数据压缩格式
Accept-Encoding: gzip, deflate
//主机
Host: localhost
//连接形式(长连接，表示不要立马断掉我们的请求)
Connection: Keep-Alive
```
这段请求中
``` bash
GET /http/1.php HTTP/1.1 
```
称之为请求行

下面的内容
``` bash
Accept: text/html, application/xhtml+xml, */*
Accept-Language: zh-CN
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
UA-CPU: AMD64
Accept-Encoding: gzip, deflate
Host: localhost
Connection: Keep-Alive
```
称之为 消息头

最后剩下一个实体内容（也称为 一个空行）