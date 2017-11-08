---
title: react以函数为子组件
date: 2017-10-26 15:10:24
tags: react
---

### 代理方式的高阶组件
react高阶组件不是唯一可用于提高组件代码重用的方法，高阶组件扩展现有组件功能的方式主要是通过props，以代理方式的高阶组件为例子，新产生的组件和原有的组件说到底是父子关系的组件，而俩个组件之间的通信自然是props,看下面的代码

```
import React from 'react';

const addNewProps = (WrappedComponent, newProps) => {
  return class WrappingComponent extends React.Component {
    render() {
      return <WrappedComponent {...this.props} {...newProps} />
    }
  }
}

export default addNewProps;
```

如果要使用这个组件，作为参数的组件必须要能够接受名为 newProps 的prop，不然应用这个组件就完全没有意义，这就是代理组件的一个缺点，对原有组件的props有了固化的要求。

### 以函数为子组件

以函数为子组件就是为了克服这种局限性的，在这种模式下，实现代码重用的不是一个函数，而是一个真正的react组件，这样的组件有一个特点，就是要求必须有子组件的存在，而且这个子组件必须是一个函数。在实例的生命周期中，this.props.children()引用的就是子组件，render函数会直接把this.props.children当做函数来调用，得到的结果就可以作为render函数返回结果的一部分。

```
import React from 'react';

const loggedinUser = 'mock user';

class AddUserProp extends React.Component {
  render() {
    const user = loggedinUser;
    return this.props.children(user)
  }
}

AddUserProp.propTypes = {
  children: React.PropTypes.func.isRequired
}

export default AddUserProp;
```

使用这个组件的灵活之处在于他没有被对增强的组件有任何的props的要求，只是传递过去一个参数，至于使用的话

例如：我们想让一个组件吧user显示出来

```
<AddUserProp>
	{
		(user)=> <div>{user}</div>
	}
</AddUserProp>
```

又例如：我们想吧user传递给另外一个Foo组件

```
<AddUserProp>
	{
		(user)=> <Foo user={user} />
	}
</AddUserProp>
```

可以看到，使用函数为子组件非常灵活。