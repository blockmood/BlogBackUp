---
title: redux等插件配置
date: 2017-09-22 10:10:24
tags: redux
---

## 插件列表

> React Devtools 可以检验react组件的树形结构

> Redux Devtools 可以检视Redux数据流

> React Perf 可以发现React组件渲染性能问题


### 代码配置
```
import {createStore, combineReducers, applyMiddleware, compose} from 'redux';

import {reducer as todoReducer} from './todos';
import {reducer as filterReducer} from './filter';

import Perf from 'react-addons-perf'

const win = window;
win.Perf = Perf

const reducer = combineReducers({
  todos: todoReducer,
  filter: filterReducer
});

const middlewares = [];
if (process.env.NODE_ENV !== 'production') {
  middlewares.push(require('redux-immutable-state-invariant')());
}

const storeEnhancers = compose(
  applyMiddleware(...middlewares),
  (win && win.devToolsExtension) ? win.devToolsExtension() : (f) => f,
);

export default createStore(reducer, {}, storeEnhancers);

```
