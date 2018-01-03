---
title: JavaScript之实例化本质
date: 2018-01-02 14:42:56
tags: javascript
---

创建一个`Product`（构造函数）实例，必须使用`new`操作符，实际会经历4个步骤：

> 1.创建一个新的对象 

> 2.将构造函数的作用域赋值给新的对象（因此this就指向了这个对象）

> 3.执行构造函数中的代码 （为这个对象添加属性）

> 4.返回新对象


#### 执行步骤

> var obj = {}

> obj.__proto__ = obj.prototype

> Product.call(obj)


如果我们给`Product.prototype`的对象添加一些函数会有什么效果呢？

```
Product.prototype.toString = function() {
    return this.id;
}
```
那么当我们使用new创建一个新对象的时候，根据`__proto__`的特性，`toString`这个方法也可以做新对象的方法被访问到。