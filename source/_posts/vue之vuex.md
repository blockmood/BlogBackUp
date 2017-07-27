---
title: vue之vuex
date: 2017-07-26 14:12:24
tags: vue
---

### 前言
在vue里，组件之间的作用域是独立的，父组件跟子组件之间的通讯可以通过prop属性来传参，但是在兄弟组件之间通讯就比较麻烦了。比如A组件要告诉一件事给B组件，那么A就要先告诉他们的爸组件，然后爸组件再告诉B。当组件比较多，要互相通讯的事情很多的话，爸组件要管他们那么多事，很累的。vuex正是为了解决这个问题，让多个子组件之间可以方便的通讯。
待办事项中的一个事件，它可能拥有几个状态，未完成、已完成、已取消或被删除等。这个事件需要在这多种状态之间切换，那么使用vuex来管理也是非常方便的。

来看一下vuex怎么完成状态管理的：
![vuex](/img/vuex.png)

以上的图分为以下几步：
- 1.所有的组件都是调用actions
- 2.分发mutation去修改state
- 3.然后state又通过getter更新到各个组件中


### 模块化
``` bash
|-store/                   // 存放vuex代码
|   |-eventModule          // 事件模块
|   |   |-actions.js
|   |   |-getters.js
|   |   |-index.js
|   |   |-mutations.js
|   |   |-state.js
|   |-themeModule           // 主题颜色模块
|   |   |-actions.js
|   |   |-getters.js
|   |   |-index.js
|   |   |-mutations.js
|   |   |-state.js
|   |-index.js              // vuex的核心，创建一个store
```
可以看到，每个模块拥有自己的state、mutation、action、getter，这样子我们就可以把我们的项目根据功能划分为多个模块去使用vuex了，而且后期维护也不会一脸懵逼。

### 状态管理

接下来，我们来看看vuex完成状态管理的一个流程。
举个栗子：一个待办事项，勾选之后，会在未完成列表里移除，并在已完成的列表里出现。这个过程，是这个待办事项的状态发生了改变。勾选的时候，是执行了一个方法，那我们就先写这个方法。在 event_list.vue 文件里新建一个moveToDone方法。
``` bash
methods: {
    moveToDone(id){ //移至已完成
        this.$store.dispatch('eventdone', id);
    }
}
```
在 moveToDone 方法中通过 store.dispatch 方法触发 action, 接下来我们在 eventModule/actions.js 中来注册这个 action, 接受一个 id 的参数。
``` bash
export default {
    eventdone = ({ commit }, param) =>{
        commit('EVENTDONE',{id: param});
    }
}
```
action 通过调用 store.commit 提交载荷(也就是{id: param}这个对象)到名为'EVENTDONE'的 mutation，那我们再来注册这个 mutation
``` bash
export default {
    EVENTDONE(states,obj){
        for (let i = 0; i < states.event.length; i++) {
            if (states.event[i].id === obj.id) {
                states.event[i].type = 2;
                states.event[i].time = getDate();
                var item = states.event[i];
                states.event.splice(i, 1);          // 把该事件在数组中删除
                break;
            }
        }
        states.event.unshift(item);                 // 把该事件存到数组的第一个元素
        local.set(states);                          // 将整个状态存到本地
    }
}
```
通过 mutation 去修改 state, state里我们存放了一个 event 属性
``` bash
export default {
    event: []
};
```
在组件中要获得这个 state 里的 event, 那就需要写个getters
``` bash
export default {
    getDone(states){
        return states.event.filter(function (d) {
            if (d.type === 2) {                 // type == 2表示已完成
                return d;                       // 返回已完成的事件
            }
        });
    }
};
```
然后每个module里都有一个index.js文件，把自己的state、mutation、action、getters都集合起来，就是一个module
``` bash
import * as func from '../function';
import actions from './actions.js';
import mutations from './mutations.js';
import state from './state.js';
import getters from './getters.js';

export default {
    state,
    getters,
    actions,
    mutations
}
```
在 store/index.js 里创建一个 store 对象来存放这个module
``` bash
import Vue from 'vue';
import Vuex from 'vuex';
import event from './eventModule';
Vue.use(Vuex);
export default new Vuex.Store({
    modules: {
        event
    }
});
```

最后在 event_list.vue 组件上，我们通过计算属性 computed 来获取到这个从未完成的状态改变到已完成的状态，我们要用到 store 这个对象里的getters
``` bash
computed: {
    getDone(){
        return this.$store.getters.getDone;
    }
}
```

[源码地址](https://github.com/lin-xin/notepad/)