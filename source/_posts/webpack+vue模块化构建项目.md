---
title: webpack+vue模块化构建项目
date: 2017-06-02 14:12:24
tags: vue
---
### 开始(确认已经安装node环境和npm包管理工具)
拓展：npm install 在安装 npm 包时，有两种命令参数可以把它们的信息写入 package.json 文件，一个是npm install --save ，另一个是 npm install --save-dev，他们表面上的区别是--save 会把依赖包名称添加到 package.json 文件 dependencies 键下，--save-dev 则添加到 package.json 文件 devDependencies 键下，--save-dev 是你开发时候依赖的东西，--save 是你发布之后还依赖的东西。
``` bash
1、新建项目文件名为demo
2、npm init -y 初始化项目
3、npm install --save vue 默认安装最新版vue
4、npm install --save-dev webpack webpack-dev-server //安装webpack，webpack-dev-server（一个小型服务器）
5、npm install --save-dev babel-core babel-loader babel-preset-es2015 //安装babel的作用是将es6的语法编译成浏览器认识的语法es5
6、npm install --save-dev vue-loader vue-template-compiler //用来解析vue的组件，.vue后缀的文件
7、npm install --save-dev css-loader style-loader   //用来解析css
8、npm install --save-dev url-loader file-loader    //用于打包文件和图片
9、npm install --save-dev sass-loader node-sass     //用于编译sass

以下等等(省略)
```
### 项目目录结构
``` bash
demo
-src
--assets
---img
---sass
---style
--components
---...
--views //这里放详情页面
---...
--App.vue
--main.js
-index.html
-package.json
-webpack.config.js
```

### package.json
``` bash
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
	"dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
	"build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "vue": "^2.3.3"
  },
  "devDependencies": {
    "babel-core": "^6.24.1",
    "babel-loader": "^7.0.0",
    "babel-preset-es2015": "^6.24.1",
    "cross-env": "^5.0.0",
    "css-loader": "^0.28.4",
    "file-loader": "^0.11.1",
    "style-loader": "^0.18.1",
    "url-loader": "^0.5.8",
    "vue-loader": "^12.2.1",
    "vue-template-compiler": "^2.3.3",
    "webpack": "^2.6.1",
    "webpack-dev-server": "^2.4.5"
  }
}
```

### webpack.config.js
``` bash
var path = require('path')
var webpack = require('webpack')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname, './dist'),
        publicPath: '/dist/',
        filename: 'build.js'
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader',
                options: {
                    loaders: {
                    }
                }
            },
            {
                test: /\.js$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.(png|jpg|gif|svg)$/,
                loader: 'file-loader',
                options: {
                    name: '[name].[ext]?[hash]'
                }
            }
            ,
            {
                test: /\.css$/,
                loader: "style-loader!css-loader"
            }
        ]
    },
    resolve: {
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    },
    devServer: {
        historyApiFallback: true,
        noInfo: true,
        inline: true
    },
    performance: {
        hints: false
    },
    devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
    module.exports.devtool = '#source-map'
    // http://vue-loader.vuejs.org/en/workflow/production.html
    module.exports.plugins = (module.exports.plugins || []).concat([
        new webpack.DefinePlugin({
            'process.env': {
                NODE_ENV: '"production"'
            }
        }),
        new webpack.optimize.UglifyJsPlugin({
            sourceMap: true,
            compress: {
                warnings: false
            }
        }),
        new webpack.LoaderOptionsPlugin({
            minimize: true
        })
    ])
}
```
这里webpack配置文件不明白的，可以看 [webpack入门](http://www.jianshu.com/p/42e11515c10f)，至于更多的配置，可百度。
### 项目跑起来
``` bash
1、webpack

2、webpack-dev-server
```
### 转换npm命令
``` bash
npm install cross-env --save-dev
```
package.json文件配置添加：
``` bash
"dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
"build": "webpack"
```