---
title: extract-text-webpack-plugin单独打包CSS
date: 2017-07-11 13:12:24
tags: webpack
---
extract-text-webpack-plugin该插件的主要是为了抽离css样式,防止将样式打包在js中引起页面样式加载错乱的现象;首先我先来介绍下这个插件的安装方法:
``` bash
npm install extract-text-webpack-plugin --save-dev
```
首先进入项目的根目录,然后执行以上命令进行插件的安装,插件安装完成后,接下来我们要做的就是在webpack.config.js中引入该插件
``` bash
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: "css-loader"
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin("styles.css"),
  ]
}
```

该插件有三个参数意义分别如下:
``` bash
use:指需要什么样的loader去编译文件,这里由于源文件是.css所以选择css-loader
fallback:编译后用什么loader来提取css文件
publicfile:用来覆盖项目路径,生成该css文件的文件路径
```