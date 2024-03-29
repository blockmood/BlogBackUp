---
title: Eslint配置
date: 2021-01-19 20:42:56
tags: Eslint
---

> 前提 vscode 安装了eslint .

.vscode 统一检测安装插件

extensions.json 
```
{
  "recommendations":[
    "dbaeumer.vscode-eslint",
    "stylelint.vscode-stylelint"
  ]
}
```

### vscode 文件下方显示eslint状态 检测是否安装成功

```
"eslint.alwaysShowStatus": true,
```

### 项目初始化

```
eslint --init
```
> 根据选项选择 如果是react 则需要安装 `eslint-plugin-react@latest`

### 自动修复

```
"eslint-fix": "eslint componsents/**/*.js --fix"
```

### 配置

```
module.exports = {
    'env': {
        'browser': true,
        'es2021': true
    },
    'extends': 'plugin:react/recommended',
    'parserOptions': {
        'ecmaFeatures': {
            'jsx': true
        },
        'ecmaVersion': 13,
        'sourceType': 'module'
    },
    'plugins': [
        'react'
    ],
    'rules': {
        //关闭换行符操作系统格式问题
        'linebreak-style': [
            'off',
            'unix'
        ],
        'quotes': [
            'error',
            'single'
        ],
        'semi': [
            'error',
            'always'
        ],
        // 禁止缩进错误
        'indent': 0,
        // 关闭不允许使用 no-tabs
        'no-tabs': 'off',
        'no-console': 1,
        // 设置不冲突 underscore 库
        'no-underscore-dangle':0,
        // 箭头函数直接返回的时候不需要 大括号 {}
        'arrow-body-style': [2, 'as-needed'],
        'no-alert':'error',
 
        // 设置是否可以重新改变参数的值
        'no-param-reassign': 0,
        // 允许使用 for in
        'no-restricted-syntax': 0,
        'guard-for-in': 0,
        // 不需要每次都有返回
        'consistent-return':0,
        // 允许使用 arguments
        'prefer-rest-params':0,
        // 允许返回 await
        'no-return-await':0,
        // 不必在使用前定义 函数
        'no-use-before-define': 0,
        // 允许代码后面空白
        'no-trailing-spaces': 0,
 
        // 关闭大括号内的换行符要求
        'object-curly-newline': 0,
 
        // 有一些 event 的时候，不需要 role 属性，不需要其他解释
        'jsx-a11y/no-static-element-interactions':0,
        'jsx-a11y/click-events-have-key-events':0,
        // 类成员之间空行问题
        'lines-between-class-members':0,
 
 
        // 不区分是否在 despendencies
        'import/no-extraneous-dependencies': 0,
        // 引用时候根据根目录基础
        'import/no-unresolved': 0,
 
        // 关闭解构赋值报错
        'react/destructuring-assignment': 0,   
        // 允许在 .js 和 .jsx 文件中使用  jsx
        'react/jsx-filename-extension': [1, { 'extensions': ['.js', '.jsx'] }],
        // jsx > 紧跟着属性
        'react/jsx-closing-bracket-location': [1, 'after-props'],
        // 不区分是否是 无状态组件
        'react/prefer-stateless-function': 0,
        // prop-types忽略children属性
        'react/prop-types': [1, { ignore: ['children']}],
        // 关闭禁止prop-types类型
        'react/forbid-prop-types': 0,
        // 关闭default-props检查
        'react/require-default-props':0
    },
    'settings':{
        'react':{
            'version':'999'
        }
    }
};

```