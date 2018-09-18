---
title: webpack编译react es6
date: 2017-08-12 10:12:24
tags: webpack
---

```
npm install babel-loader babel-core babel-preset-react babel-preset-es2015 --save-dev  
```

```
module: {  
      rules: [  
        {  
          test: /\.jsx?$/,  
          exclude: /(node_modules|bower_components)/,  
          loader: 'babel-loader', // 'babel-loader' is also a legal name to reference  
          query: {  
            presets: ['react', 'es2015']  
          }  
        }  
      ]  
     }
```