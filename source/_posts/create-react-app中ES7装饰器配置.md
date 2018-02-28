---
title: create-react-app中ES7装饰器配置
date: 2018-02-02 08:10:24
tags: create-react-app
---

```
npm install --save-dev babel-plugin-transform-decorators-legacy
```

然后在

```
node_modules/babel-preset-react-app/index.js plugins
```

中添加

```
require.resolve('babel-plugin-transform-decorators-legacy')
```


详情： [https://segmentfault.com/q/1010000010491983](https://segmentfault.com/q/1010000010491983)