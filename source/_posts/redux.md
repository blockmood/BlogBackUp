---
title: redux
date: 2017-09-13 24:10:24
tags: redux
---


## 工作流程

![redux](/img/redux.jpg)

> 1.用户通过dispath发出Action

> 2.Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State 。

> 3.State 一旦有变化，Store 就会调用监听函数subscribe。
