---
title: react组件该如何写
date: 2017-10-10 15:10:24
tags: redux
---

书中写了好多种react组件的书写方式，我这里大概总结了俩种，工作中也就围绕这俩种展开来的。

### 函数式

函数式的编写方法一般是当前组件基本没有任何业务逻辑可言，仅仅就只是用来做展示的功能，大概看如下组件

```
import React from 'react'

const TodoItem = ({text,completed,onToggle,onRemove}) =>{
    return (
        <li
            style={{textDecoration:completed ? 'line-through' : 'none'}}
        >
            <input type="checkbox" checked={completed} readOnly onClick={onToggle}/>
            {text}
            <button onClick={onRemove}>x</button>
        </li>
    )
}

export default TodoItem
```
那么这个组件完全只是用来展示当前的列表，用不到任何业务逻辑，有几个简单的业务逻辑也可以直接传递给父组件，让父组件代替实现。

### es6模块式

这种方式的话比较通用，上面的组件功能当然能实现，但是肯定是会有一些多余的代码，比如constructor构造等等，但是如果项目中通用这种方式的话，起码能保证项目不会出错，目前分析的话有的组件业务逻辑较多，推荐用这种方式，大概看下面的组件：

```
import React from 'react'
import {connect} from 'react-redux'
import {addTodo} from '../actions.js'

class AddTodo extends React.Component{
    constructor(){
        super(...arguments)

        this.add = this.add.bind(this)
        this.refInput = this.refInput.bind(this)
    }

    add(){
        if(!this.input.value.trim()){
            return
        }
        this.props.onAdd(this.input.value)
        this.input.value = ''
    }


    refInput(node){
        this.input = node
    }

    render(){
        return (
            <div>
                <input ref={this.refInput}/>
                <button onClick={this.add}>add</button>
            </div>
        )
    }
}

const mapDispatchToProps = (dispatch) => {
    return {
        onAdd:(text)=>{
            dispatch(addTodo(text))
        }
    }
}

export default connect(null,mapDispatchToProps)(AddTodo)
```

上面的组件有用户的点击 输入等等业务逻辑，比函数式更通用，代码更清晰。