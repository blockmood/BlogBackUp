---
title: vue之全面理解MVVM模式
date: 2017-05-23 14:12:24
tags: vue
---
之前一直对MVVM这个模式有点模糊，虽然也能做出项目，但是总感觉欠缺点什么东西，所以今天特意回来看了一下vue的这个MVVM，其实理解了还是蛮简单的，可能我一直遵循的是MVC和MCP的这种思想，所有这种思想转变的还是挺难受的。

### 关于MVVM
vue集中在MVVM模式上的视图模型(ViewModel)层，并且通过双向数据绑定连接视图(View)和模型(Model)，实际的DOM操作和输出格式被抽象出来成指令和过滤器，看一个最简单的例子。

``` bash
// 这是我们的 View
<div id="example-1">
  Hello {{ name }}!
</div>
```

``` bash
// 这是我们的 Model
var exampleData = {
  name: 'Vue.js'
}

```

``` bash
// 创建一个 Vue 实例或 "ViewModel"
// 它连接 上面的View 与 Model
var exampleVM = new Vue({
  el: '#example-1',
  data: exampleData
})
```

这里的ViewModel连接着View和Model，因为我们只要修改ViewModel中的data数据，上面的Model和View会自动响应数据的变化。