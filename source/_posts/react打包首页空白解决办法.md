---
title: react打包首页空白解决办法
date: 2017-11-07 15:10:24
tags: react
---

### 解决办法

> 在package.json配置文件中加一句：　“homepage”: “./” 

>这是build之后的路径问题，改为相对路径后再次打开这个index.html文件就可以正常浏览了。