---
title: JavaScript深入之执行上下文
date: 2017-07-04 9:12:56
tags: javascript
---
在上篇[JavaScript深入之执行上下文栈](https://blockmood.github.io/2017/07/01/JavaScript%E6%B7%B1%E5%85%A5%E4%B9%8B%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88/)中讲到，当 JavaScript 代码执行一段可执行代码(executable code)时，会创建对应的执行上下文(execution context)。
对于每个执行上下文，都有三个重要属性：

- 变量对象(Variable object，VO)
- 作用域链(Scope chain)
- this

### 思考题
``` bash
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();
```

``` bash
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
checkscope()();
```
这俩段代码到底哪里不一样呢，然后就在[JavaScript深入之执行上下文栈](https://blockmood.github.io/2017/07/01/JavaScript%E6%B7%B1%E5%85%A5%E4%B9%8B%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88/)中，讲到了两者的区别在于执行上下文栈的变化不一样，然而，如果是这样笼统的回答，依然显得不够详细，本篇就会详细的解析执行上下文栈和执行上下文的具体变化过程。

### 具体执行分析
我们分析第一段代码：
``` bash
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();
```

#### 执行过程如下：
- 1.执行全局代码，创建全局执行上下文，全局上下文被压入执行上下文栈
``` bash
 ECStack = [
     globalContext
 ];
```
- 2.全局上下文初始化
``` bash
globalContext = {
    VO: [global, scope, checkscope],
    Scope: [globalContext.VO],
    this: globalContext.VO
}
```
- 2.初始化的同时，checkscope 函数被创建，保存作用域链到函数的内部属性[[scope]]
``` bash
checkscope.[[scope]] = [
    globalContext.VO
  ];
```
- 3.执行 checkscope 函数，创建 checkscope 函数执行上下文，checkscope 函数执行上下文被压入执行上下文栈
``` bash
ECStack = [
    checkscopeContext,
    globalContext
   ];
```
- 4.checkscope 函数执行上下文初始化：
	- 复制函数 [[scope]] 属性创建作用域链，
	- 用 arguments 创建活动对象，
	- 初始化活动对象，即加入形参、函数声明、变量声明，
	- 将活动对象压入 checkscope 作用域链顶端。
同时 f 函数被创建，保存作用域链到 f 函数的内部属性[[scope]]
``` bash
checkscopeContext = {
        AO: {
            arguments: {
                length: 0
            },
            scope: undefined,
            f: reference to function f(){}
        },
        Scope: [AO, globalContext.VO],
        this: undefined
    }
```
- 5.执行 f 函数，创建 f 函数执行上下文，f 函数执行上下文被压入执行上下文栈
``` bash
 ECStack = [
        fContext,
        checkscopeContext,
        globalContext
    ];
```
- 6.f 函数执行上下文初始化, 以下跟第 4 步相同：
``` bash
fContext = {
        AO: {
            arguments: {
                length: 0
            }
        },
        Scope: [AO, checkscopeContext.AO, globalContext.VO],
        this: undefined
    }
```
- 7.f 函数执行，沿着作用域链查找 scope 值，返回 scope 值
- 8.f 函数执行完毕，f 函数上下文从执行上下文栈中弹出
``` bash
ECStack = [
        checkscopeContext,
        globalContext
    ];
```
- 9.checkscope 函数执行完毕，checkscope 执行上下文从执行上下文栈中弹出
``` bash
    ECStack = [
        globalContext
    ];
```

第二段代码的执行过程很以上同理。
``` bash
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
checkscope()();
```