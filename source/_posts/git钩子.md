---
title: git husky 及 lint-staged
date: 2021-01-18 12:11:24
tags: git
---

## 安装
```
yarn add husky lint-staged -D
```

### 项目初始化

```
npx husky install
```

### 配置
```
npx husky add .husky/pre-commit "npx --no-install lint-staged"
```
> npx --no-install lint-staged 使用 lint-staged 执行本地资源  不下载

package.json配置

```
"husky":{
  "hooks":{
    "pre-commit": "lint-staged"
  }
},
"lint-staged":{
  "components/**/*.js": [
    "prettier --write --ignore-unknown",
    "eslint components --fix"
  ],
  "components/**/*.less": [
    "prettier --write --ignore-unknown",
    "stylelint"
  ]
}
```