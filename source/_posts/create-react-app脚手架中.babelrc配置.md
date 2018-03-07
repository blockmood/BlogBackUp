---
title: create-react-app中ES7装饰器配置
date: 2018-03-05 18:10:24
tags: create-react-app
---

```
cnpm install  babel-plugin-transform-decorators-legacy babel-preset-es2015 babel-preset-react babel-preset-stage-1 --save-dev
```

```
"devDependencies": {
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-preset-es2015": "^6.9.0",
    "babel-preset-react": "^6.5.0",
    "babel-preset-stage-1": "^6.5.0",
  },
```

```
{
  "presets": [
    "es2015",
    "stage-1"
  ],
  "plugins": ["transform-decorators-legacy"]
}

```

