---
title: 利用reselect提高数据获取性能
date: 2017-10-12 15:10:24
tags: react
---

首先安装对应的包
```
	npm install --save reselect
```
原理：
只要相关状态没有改变，那就直接使用上一次的缓存结果

```
import {createSelector} from 'reselect';
import {FilterTypes} from '../constants.js';

const getFilter = (state) => state.filter;
const getTodos = (state) => state.todos;

export const selectVisibleTodos = createSelector(
  [getFilter, getTodos],
  (filter, todos) => {
    switch (filter) {
      case FilterTypes.ALL:
        return todos;
      case FilterTypes.COMPLETED:
        return todos.filter(item => item.completed);
      case FilterTypes.UNCOMPLETED:
        return todos.filter(item => !item.completed);
      default:
        throw new Error('unsupported filter');
    }
  }
);
```
reselect 提供了createSelector函数选择器，这是高阶函数，也就是接收函数作为参数来产生一个新函数的函数。
createSelector第一个参数是一个函数数组，每个元素代表了选择器需要做的映射计算（这里原理是当前的状态和之前的状态做比较，如果相同，就把数据缓存起来，就不会执行第二个函数，如果不同，才会执行第二个函数做重新计算），例子里提供了俩个函数getFilter和getTodos，对应代码
```
const getFilter = (state) => state.filter;
const getTodos = (state) => state.todos;
```
createSelector第二个参数代表的是一个计算过程，参数为第一个参数的输出结果。

Redux要求每个reducer不能修改state的状态，如果要返回一个新的状态，就必须返回一个新的对象。如此一来，Redux Store状态树上的某一个节点如果没有变化，那么这个节点下的数据就肯定没有变化，应用在reselect中，也就是第一个参数的原理，直接缓存运算结果。
