---
title: react之shouldComponentUpdate性能优化详解
date: 2018-01-06 08:10:24
tags: react
---

在`react`中最重要的俩个生命周期方法，一个是`render()`，它决定的是组件该渲染什么，而另一个`shouldComponentUpdate`则决定组件什么时候不需要重新渲染。

`react`组件类的父类`Component`提供了`shouldComponentUpdate`的默认实现，这个默认实现就是返回一个`true`，也就说，每次更新的时候都需要调用所有的生命周期函数，也包括`render()`方法，然后根据`render()`返回的结果计算`Virtual DOM`。

当然，默认方法提供了一种最保险的办法，毕竟`react`不知道组件的内部实现，就算全部重新渲染一遍浪费一点，但至少保证不会出错。

但是要达到更好的性能，就有必要定义我们自己的`shouldComponentUpdate`的方法，让它在需要更新的时候才返回`true`，这样节省了大量的计算资源。


假设对于一个组件而言，影响组件渲染的`prop`只有`completed`和`text`,所以只要确保这俩个prop没有改变，`shouldComponentUpdate`就可以返回`false`

```
shouldComponentUpdate(nextProps,nextState){
	return (nextProps.completed !== this.props.completed) ||
	(nextState.text !== this.props.text)
}
```

根据自己组件内部不同状态，实现的方法也不同，但是大体意思都一致。

如果每一个组件都要重新实现`shouldComponentUpdate`，从代码的脚本是一件非常麻烦的事情，但是如果我们使用了`react-redux`库，一切都变的简单了。

### react-redux、shouldComponentUpdate 默认实现

`react-redux`默认实现了`shouldComponentUpdate`，实现的逻辑是比对传递给展示组件的`props`和上一次的`props`,因为展示组件完全是一个无状态组件，组件渲染什么完全由`props`决定,如果`props`没有变化，那就可以认为组件渲染没有变化。

相比`react`的`shouldComponentUpdate`的实现，`react-redux`的默认实现当前比较牛逼一些，但是在对比`props`方面，用的依然是浅比较，也就是`===`,若果`prop`的值是数字或者是字符串，只要值相同，都会返回`true`，但是如果是对象形式，浅比较只看这俩个对象的引用地址是不是相同，如果不是，哪怕这俩个对象的内容一致，都会返回`false`

```
<Foo style={{color:red}}/>
```

像上面这样的方法，Foo组件利用`react-redux`提供的`shouldComponentUpdate`默认实现，每一次渲染都会认为这个prop发生变化，因为每次都会产生一个新的对象。

既然这样，那为什么不用深比较呢？

但其实这是一个比较正确的决定，因为对象到底有多少层无法预料，如果一直使用递归所有层次进行深层次的比较的话，无疑消耗的性能可能大。


这时，有一个库，`Immutable`,可以帮我们实现简单而又不需要消耗太多性能的深比较。

### Immutable.js

在`javascript`中对象一般是可变的，因为都是使用的引用赋值，新的对象简单引用了原始对象，假设改变了新的对象，原始对象也会随着改变。

`Immutable Data`就是一旦创建，就不能再改变的数据，对`Immutable`的对象进行 修改 添加 或者删除，都会返回一个新的`Immutable`对象，`Immutable`的实现原理就是持久化数据结构，也就是旧数据在改变的情况下，要保证旧数据可用并且不改变。为了避免深拷贝把所有节点都复制一遍带来的性能问题，`Immutable`使用了结构共享，就是如果对象树中的一个节点发生变化，只修这个节点和受这个节点所影响的父节点，其他节点则进行共享。

前面我们说了可在`shouldComponentUpdate`中使用浅比较来比较`prop`，但是有一定的局限性，但是深比较的话又比较损耗性能，这时就可以在`shouldComponentUpdate`中使用`Immutable`来实现深层比较。

`Immutable`提供了简洁，高效的判断数据变化方法，只要`===`和`is`简便的API就可以知道是否要执行`render()，`这个操作几乎零成本，所以可以极大的提高性能。


[Immutable API](https://github.com/lucaong/immutable)