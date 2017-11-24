---
title: 关于JSX
date: 2017-11-22 12:12:24
tags: JSX
---

之前一直郁闷为啥react以及其他框架都会选择JSX这么个东西，网上搜的一些文章也说不到点子上，今天我自己就整理一下，这就意味我又要玩转一门新的静态编译语言。

> virtual element : 虚拟元素

> virtual DOM : 虚拟DOM


react为了方便view层的组件化，承载了html结构化页面的职责，这与其他模板语言有着相似之处，但是不同之处在于 react 是通过创建与更新虚拟元素来管理整个虚拟DOM的。

其中的虚拟元素可以理解为真实元素对应，它的构建和更新都是在内存中完成的，并不会真正渲染到DOM中去，而react创建的虚拟元素分为俩类: DOM元素 和 组件元素

#### DOM元素

```
//真实元素
<button className="btn btn-blue">
	<em>confirm</em>
</button>

//虚拟元素
{
	type:button,
	props:{
		className:'btn btn-blue',
		children:{
			type:em,
			props:{
				children:'confirm'
			}
		}
	}
}
```

#### 组件元素

```
const Button => ({color,text}){
	return {
		type:button,
		props:{
			className:color,
			children:{
				type:em,
				props:{
					children:text
				}
			}
		}
	}
}

```
当我们要生成虚拟元素的时候，就只需调用

```
Button({color:'btn btn-blue',text:'confirm'})
```

所有的DOM元素和组件元素都同理。

可以看到，编译器会把这种语法解析成json对象，以便于构建出虚拟元素，这样就可以创建虚拟DOM了。
 
所谓react组件，就是最终通过React,createElement()方法，递归渲染虚拟元素的方式构建出完全的DOM树。

在react组件中完全可以利用这方式书写html格式，只是太繁琐，所以才出现了JSX，让我们跟写html是一样的。