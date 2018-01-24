---
title: Redux异步操作
date: 2018-01-23 20:10:24
tags: redux
---

Redux单向数据流是同步操作，驱动Redux的是action,action触发后，被同步的分配给reducer函数，reducer是纯函数，纯函数不产生任何副作用，自然是执行完毕之后同步返回，返回之后的数据被同步的更新store的数据，更新状态之后会被同步的执行监听状态的函数，从来引发视图的更新。

这个过程redux一路马不停蹄的去同步执行，根本没有执行异步操作的时机，那么应该在哪里插入访问服务器的异步操作呢？

### redux-thunk 中间件

当我们想让Redux去异步的处理一个操作的时候，同样也需要派发一个action，毕竟redux单向数据流就是action驱动的，以前action返回的是一个对象，而现在action返回的是一个函数，如果返回的是对象的话，那么action就会一路被发送到reducer中，那如果返回的是函数的话，那么发送到reducer中，reducer也做不了什么事情，这个时候redux-thunk就有作用了。

redux-thunk的工作就是检查action是不是一个函数，如果是函数，就去执行，并把dispatch和getState作为参数传递给这个函数，如果不是，就会被发送到reducer。

看个例子

```
const increment = () => ({
    type:ActionTypes.INCREMENT
})
```

假设上面是一个组件，如果我们想让这个组件1秒之后加1，就需要一个新的“异步action”

```
const increment = () => (
    return (dispatch) => (
        setTimeout(()=>{
            dispatch(increment())
        },1000)
    )
)
```

这就是异步的一个原理，可以看出，异步action最终还是要产生同步的action同步的派发才能对redux产生影响

异步的方式有很多种，有时间再写其他几种方式。