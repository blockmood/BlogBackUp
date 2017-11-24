---
title: iScrool All kinds of pit
date: 2017-11-21 12:12:24
tags: iScrool
---


### 关于移动端卡顿解决办法

>  touch-action: none;


初始化之前

```
document.addEventListener('touchmove', function (e) { e.preventDefault(); }, false);
```

### 关于callback事件

[参考链接](https://github.com/schovi/react-iscroll)
[中文链接](http://wiki.jikexueyuan.com/project/iscroll-5/)
[中文链接2](http://www.360doc.com/content/14/0724/11/16276861_396699901.shtml)


### 关于事件参数

> maxScrollY：表示最大需要滑动的高度

> y：表示滑动了多少高度 

其余都同理。