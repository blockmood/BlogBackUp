---
title: webpack之sass-loader模块加载
date: 2017-06-01 14:12:24
tags: webpack
---
为了使用sass，我们需要安装sass的依赖包。
``` bash

$ npm install --save-dev sass-loader

$ npm install --save-dev node-sass  //sass-loader依赖于node-sass，所以还要安装node-sass

```
当然了，使用样式的话，css-loader和style-loader也是必须的依赖包，如果没有安装，可以类似上述的方法安装

css-loader使你能够使用类似@import 和 url(…)的方法实现 require()的功能；
style-loader将所有的计算后的样式加入页面中；
　　二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中。

### webpack配置中的配置
只列举sass配置,前提是已经装了 css-loader和style-loader 并且已经配置
``` bash
{
    test: /\.scss$/,
    use: [{
        loader: "style-loader" // creates style nodes from JS strings
    }, {
        loader: "css-loader" // translates CSS into CommonJS
    }, 
        loader: "sass-loader" // compiles Sass to CSS
    }]
}
```
### 使用方法
``` bash
import '../../css/test.scss'

require('../../css/test2.scss');
```

### 文件中使用
``` bash
<style lang="scss">

</style>
```