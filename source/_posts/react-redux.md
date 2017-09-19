---
title: react-redux
date: 2017-09-13 24:10:24
tags: redux
---

### react-redux提供了俩个最主要的功能

> connect : 连接容器组件和展示组件
	
> Provider: 提供包含store的context

# Provider

在组件中导入store是非常不利于复用的，所以react提供了一个叫context的功能，能够完美解决这个问题。

因为redux应用中只有一个store，因此所有的组件如果要使用store的话，只能访问这唯一的store,很自然，希望顶层的组件来扮演这个context提供者的角色，只要顶层组件提供包含store的context，那就覆盖了整个应用的所有组件。

创建Provider.js

```
import {PropTypes,Component} from 'react'

class Provider extends Component{

    getChildContext(){				//这个函数返回的就是代表context对象
        return {
            store:this.props.store
        }
    }

    render(){
        return this.props.children	//把渲染的工作交给子组件
    }
}

Provider.childContextTypes = {
  store: PropTypes.object
};

export default Provider
```

然后再入口文件

```
<Provider store={store}>
	<CounterPanel />
</Provider>
```

底层组件使用
为了让底层组件能够访问到context，必须给context-Types的类型和Provider.childContextTypes一样的值，俩者必须一致，不然就无法访问到context

```
CounterContainer.contextTypes = {
  store: PropTypes.object
}
```

还有构造函数里面必须带上context这个参数

```
constructor(props,context){
	super(props,context)
}
```

然后我们就可以通过this.context.store来访问这个store



# connect

```
export default connect(mapStateToTypes, mapDispatchToTypes)(Counter);
```
connect是react-redux提供的一个方法，这个方法接收俩个参数，mapStateToTypes和mapDispatchToTypes，执行结果依然是一个函数，然后才可以再后面又加一个括号，这个括号里面就是展示组件。

#### 这个函数做了什么呢？
作为一个容器组件，做的事情无外乎就是俩件事情。

> 1.把store上的状态转化为内层展示组件的props

> 2.把内存展示组件中的用户动作转化为派送给store的动作。
