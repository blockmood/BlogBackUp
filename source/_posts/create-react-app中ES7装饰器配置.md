---
title: create-react-app中ES7装饰器配置
date: 2018-02-02 08:10:24
tags: create-react-app
---

### 第一种方式

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


### 第二种方式

.babelrc
```
{
    "presets": [
        ["env", {
            "modules": false
        }],
        "es2015",
        "stage-0",
        "stage-2",
        "react"

    ],
    "plugins": [
        "transform-decorators-legacy",
        "transform-runtime"
    ]
}
```


```
"devDependencies": {
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1"
  }

```
