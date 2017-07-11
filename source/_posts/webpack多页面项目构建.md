---
title: webpack多页面项目构建
date: 2017-07-10 10:12:24
tags: webpack
---

使用 webpack 已经将近一年了，期间用它构建过4、5个项目，踩过一些坑，现在用自己的理解记录下来。

我现在教你如何一步一步搭建 webpack 开发的多页面项目。本文项目地址在 [https://github.com/blockmood/generate-pages-tutorial](https://github.com/blockmood/generate-pages-tutorial)

具体看github吧

``` bash
var path = require('path');
var ROOT = path.resolve(__dirname)

module.exports = {
	entry:{
		'page1/main':'./src/page1/main',
		'page2/main':'./src/page2/main'
	},
	output:{
		filename:'[name].js',
		path:ROOT + '/dist/'
	},
	module: {
		rules:[
			{
                test: /\.css$/,
                loader: "style-loader!css-loader"
            }
		]
	}
}
```