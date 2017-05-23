---
title: vue-scroller列表上拉下拉加载更多
date: 2017-05-22 22:10:24
tags: vue
---

我个人觉得官方例子特别好,直接上案例: [链接地址](https://github.com/wangdahoo/vue-scroller-demo)   [演示地址](https://wangdahoo.github.io/vue-scroller/#/)
### demo的使用方法
``` bash
$ npm install
$ npm run dev
```
### 在项目中的使用
``` bash
$import Vue from 'vue'
$import VueScroller from 'vue-scroller'
$Vue.use(VueScroller)

```

### 源码说明
以下例子中，你只需要看我标注的函数即可,我们可以理解为钩子函数，实际项目中我们只会用到这几个函数，至于其余的代码均为官方提供。
``` bash
<template>
  <div id="app">
    <div class="header">
      <h1 class="title">Refresh & Infinite</h1>
    </div>

    <scroller style="top: 44px"
      :on-refresh="refresh"
      :on-infinite="infinite">
      <div v-for="(item, index) in items" class="row" :class="{'grey-bg': index % 2 == 0}">
        {{ item }}
      </div>
    </scroller>
  </div>
</template>
<script>
  import Vue from 'vue'
  import VueScroller from 'vue-scroller'
  Vue.use(VueScroller)
  export default {
    data() {
      return {
        items: []
      }
    },
    mounted() {		// 默认初始化加载 
    	for (var i = 1; i <= 20; i++) {
        this.items.push(i + ' - keep walking, be 2 with you.')
      }
      this.top = 1
      this.bottom = 20
    },
    methods: {      //上拉加载函数  
      refresh (done) {
        setTimeout(() => {
          var start = this.top - 1
          for (var i = start; i > start - 10; i--) {
            this.items.splice(0, 0, i + ' - keep walking, be 2 with you.')
          }
          this.top = this.top - 10
          done()
        }, 1500)
      },
      infinite (done) { //下拉加载函数 
        setTimeout(() => {
          var start = this.bottom + 1
          for (var i = start; i < start + 10; i++) {
            this.items.push(i + ' - keep walking, be 2 with you.')
          }
          this.bottom = this.bottom + 10
          done()
        }, 1500)
      }
    }
  }
</script>
```