---
title: redux-saga
date: 2018-12-16 21:10:24
tags: redux
---


## Effects

Effect 是一个 javascript 对象，里面包含描述副作用的信息，可以通过 yield 传达给 sagaMiddleware 执行

在 redux-saga 世界里，所有的 Effect 都必须被 yield 才会执行，所以有人写了 eslint-plugin-redux-saga 来检查是否每个 Effect 都被 yield。并且原则上来说，所有的 yield 后面也只能跟Effect，以保证代码的易测性。

例如：

```
yield fetch(UrlMap.fetchData);
```

应该用

```
yield call(fetch, UrlMap.fetchData)
```

从而可以使代码可测：

```
assert.deepEqual(iterator.next().value, call(fetch, UrlMap.fetchData))
```


### put

作用和 redux 中的 dispatch 相同。

```
yield put({ type: 'CLICK_BTN' });
```

### select

作用和 redux thunk 中的 getState 相同。

```
const id = yield select(state => state.id);
```

### take

等待 redux dispatch 匹配某个 pattern 的 action .(只执行一次)

### 阻塞和无阻塞

redux-saga 可以用 fork 和 call 来调用子 saga ，其中 fork 是无阻塞型调用，call 是阻塞型调用。

如果看过 saga 的论文，就知道 saga 是由许多子 saga （或者 subtransaction）组合起来的。fork Effect 和它的字面意思一样，即创建一个子 saga 。

fork
无阻塞调用

```
function* count(number) {
  let currNum = number;

  while (currNum >= 0) {
    console.log(currNum--);
    yield delay(1000);
  }
}

function countSaga* () {
  while (true) {
    const { payload: number } = yield take(BEGIN_COUNT);
    const countTaskId = yield fork(count, number);

    yield take(STOP_TASK);
    yield cancel(countTaskId);
  }
}
```

call
有阻塞地调用 saga 或者返回 promise 的函数。


```
const project = yield call(fetch, { url: UrlMap.fetchProject });
const members = yield call(fetchMembers, project.id);
```


### 组合saga middleware

```
export default function * rootSaga() {
  yield all([
    fork(watchIsLogin),
    fork(watchUsername),
    fork(watchPassword)
  ]);
}

export default function* rootSaga(){
    yield all([
        ...countSaga
    ])
}

export const countSaga = [
    fork(watchIncrementAsync),
    fork(watchCountDel)
]


```