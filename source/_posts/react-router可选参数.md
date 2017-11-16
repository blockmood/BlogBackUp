---
title: react-router 4.0 以上可选参数配置
date: 2017-11-14 15:10:24
tags: react-router
---

#### 低版本
```
<Route path="/search/:category(/:keyword)" component={Search}/>
```
#### 高版本
```
<Route path="/search/:category/:keyword?" component={Search}/>
```