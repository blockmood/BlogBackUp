---
title: webpack编译es6之babel配置
date: 2017-07-07 13:12:24
tags: webpack
---
### 安插件
``` bash
npm install --save-dev babel-core babel-loader babel-preset-es2015
```

### .banelrc 文件配置
``` bash
{
	"presets": ["es2015"]
}
```

### webpack 配置
``` bash
{
    test: /\.js$/,
    loader: 'babel-loader',
    query: {presets: ['es2015']}
}
```