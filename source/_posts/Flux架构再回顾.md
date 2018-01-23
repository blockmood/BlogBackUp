---
title: Flux
date: 2018-01-21 24:10:24
tags: Flux
---

### Flux是什么

简单说，Flux 是一种架构思想，专门解决软件的结构问题。它跟MVC 架构是同一类东西，但是更加简单和清晰。

### 基本概念

Flux将一个应用分成4个部分

> View层

> Action(动作) 视图层触发的消息(比如mouseClick)

> Dispatcher(派发器) 用来接收到Action 执行回调函数

> Store (数据层) 用来存放应用的数据，一旦发生改变，就提示view进行更新

Flux最大的特点就是 数据是"单向流动"的

一般的流程

> 1.用户访问view

> 2.view发出action

> 3.Dispatcher接收到action，要求store进行相应的更新

> 4.Store更新后，向view成发送一个"change"事件

> 5.view收到"change"事件后，更新页面

上面的过程，数据总是"单向流动"的，任何相邻的部分都不会发生数据的"数据的双向流动"，这保证了流程的清晰，这也是Flux最大的优点。

在`MVC`架构中，我们无法杜绝`Model`和`View`之间的直接对话，这样如果项目很大的话，就会造成数据的逻辑混乱，但是如果是`Flux`架构的话，如果想要改变`Store`就必须触发`Action`，这就是一个规矩，说直白点,`Store`只有`get`方法,根本无法去直接`set`它，这看起来是一个限制，但是这种限制正好限制了数据之间的混乱流动。

### 缺点

Flux的缺点

> 每一个Store都需要通过`AppDispatcher.register()`方法监听`Action`的触发

> 难以通过服务器端渲染

> 再哪里发请求，如何处理异步流