---
title: react源码解读
date: 2018-01-09 08:10:24
tags: react
---

在`react`中，每个文件的命名风格与含义可相互解释，整体代码的结构看如下

```
--------
|---addons			//包含一系列的工具插件
|---isomorphic			//包含一系列同构方法
|---shared			//包含一些公共或者常用方法
|---test			//包含测试方法
|---core/test			//包含错误提示
|---renderers			//·react·核心部分
|--------dom
|--------------client		//包含DOM操作
|--------------server		//包含服务端渲染的实现和方法
|--------------shared		//包含文本组件，标签组件，DOM属性操作，CSS属性操作
|--------shared
|--------------event		//包含事件方法
|--------------reconciler	//称为`协调器`,包含react自定义组件，组件生命周期，setState机制，DOM diff算法等
--------

```

可以说，上诉结构中`reconciler`是`react`中最重要的部分。

在Web开发中，要将更新的数据实时的显示到UI上，就不可避免的需要操作DOM，而频繁的操作DOM则是造成性能瓶颈的主要原因之一。为此，`react`引入了`Virtual DOM`机制。毫不夸张的说`Virtual DOM`是`react`的精髓,而`reconciler`就是实现它的主要源码。

`Virtual DOM`实际上是在浏览器端用`javascript`实现的一套DOM API，它就类似一个虚拟空间，它也包括一整套的`Virtual DOM`模型，生命周期管理和维护，高性能的diff算法，以及把`Virtual DOM`绘制到真实DOM元素的Patch方法。

我们在基于`react`开发时，所有的DOM树都是通过`Virtual DOM`来实现的。`react`在`Virtual DOM`上实现了diff算法，当数据更新时，会通过diff去寻找变更的节点，并只对变化的部分进行一个替换，而不是重新渲染整个DOM树。

关于DOM diff算法比对，有俩种比较情况

> 1.节点类型不同的情况

> 2.节点类型相同的情况


### 节点类型不同的情况

在`Virtual DOM`中，每一个节点都可以当做以下所有节点的根节点，如果树形结构的根节点不相同，就不需要考虑原来的那个根节点是不是移动到其他地方去了，它会认为这个根节点已经没用了，可以扔掉，需要重新构建新的DOM树，原有树形结构上的`react`组件会经历卸载过程。

也就是说，你有可能对`Virtual DOM`只是一个更新的过程，但是却有可能引发这个属性结构上的某些组件的"装载"和"卸载"的过程。

举个列子：

在更新之前

```
<div>
	<Todos />
</div>
```

想要更新成这样

```
<span>
	<Todos />
</span>
```

在做比较的时候，一看根节点原来是`div`现在换成了`span`，类型不一样，那么这个算法就必须废弃掉之前的`div`节点，包括下面的所有子节点，然后构建`span`节点以及所有子节点。

很明显，这是一个浪费，因为`div`和`span`其实是一模一样的，它俩其实不做什么实质的功能，就是一个元素，仅仅因为类型不同就必须把`<Todos />`卸载和重新装载。

所有，我们一定要避免上面这样的情景出现，一定要避免包含组件的根节点被随意改变。


### 节点类型相同的情况

如果俩个树形结构的节点相同，`react`就会认为原来的根节点只需要更新过程，不会将其卸载。

这里分为俩种情况，一种是`DOM`元素，一种是`react`组件

#### DOM元素

假设我们原来的DOM元素类似下面情况

```
<div style={{color:red}} ckassName="welcome">
hello world
</div>
```

改变之后的元素是这样

```
<div style={{color:green}} ckassName="hello">
hello worldsss
</div>
```

可以看到，俩者之间的差距是`div`中的内容发送了变化，另外`style`和`class`也发生了改变。`react`可以对比发现这些属性内容的变化，在操作`DOM`树上节点的时候，只是去修改这些发生变化的部分，让`DOM`操作尽可能减少。

#### react组件

如果是`react`组件的话，`react`只能根据新节点的`props`去更新原来根节点的组件实例，然后引发这个组件的更新过程，也就是按以下顺序引发函数

> componentWillReceiveProps

> shouldComponentUpdate

> componentWillUpdate

> render 

> componentDidUpdate


在这个过程中，如果`shouldComponentUpdate`返回false的话，更新过程就此打住，不会再继续，所以每个组件都有必要重视`shouldComponentUpdate`，如果发现没有必要的渲染，就让他返回false