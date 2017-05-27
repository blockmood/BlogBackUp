---
title: vue-lazyload图片懒加载
date: 2017-05-24 24:10:24
tags: vue
---
支持1.0和2.0
### 使用案例
``` bash
$ npm install vue-lazyload --save-dev
```

``` bash
//主文件 main.js  
import Vue from 'vue'
import App from './App.vue'

// for Vue 1.0
import VueLazyload from 'vue-lazyload'

// for Vue 2.0
import VueLazyload from 'vue-lazyload/vue-lazyload-next'

Vue.use(VueLazyload, {
  error: 'dist/error.png',
  loading: 'dist/loading.gif',
  try: 3 // default 1
})

new Vue({
  el: 'body',
  components: {
    App
  }
})
```

``` bash
//组件中
<script>
export default {
  data () {
    return {
      list: [
        'your_images_url', 
        'your_images_url', 
        'your_images_url'
      ]
    }
  }
}
</script>
<template>
  <div class="img-list">
    <ul id="container">
      <li v-for="img in list">
        <img v-lazy="img">
      </li>
      <!-- 
      for custom container
      <li v-for="img in list">
        <img v-lazy.container="img">
      </li> 

      for background-image
      <li v-for="img in list">
        <div v-lazy:background-image="img" class="bg-box"></div>
      </li>
      -->
    </ul>
  </div>
</template>
```