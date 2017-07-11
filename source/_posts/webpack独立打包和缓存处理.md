---
title: webpack独立打包和缓存处理
date: 2017-07-10 11:12:24
tags: webpack
---

### 独立打包
``` bash
var webpack = require('webpack');
var path = require('path');
 
module.exports = {
 entry: {
  main: './app/index.js',
  vendor: ['jquery']
 },
 output: {
  filename: '[name].js',
  path: path.resolve(__dirname, 'dist')
 },
 plugins:[
  new webpack.optimize.CommonsChunkPlugin({
   name: 'vendor'
  }),
 ]
}
```
上方我们将原本的单入口文件改成了多入口文件，并加入了vendor属性。vendor属性用于配置打包第三方类库，写入数组的类库名将统一打包到一个文件里。
同时我们将输出的filename用[name]变量来自动生成文件名，最后我们添加了一个CommonsChunkPlugin的插件，用于提取vendor。
配置完成后我们运行webpack命令：
``` bash
Hash: ee1daf95c1986768927a
Version: webpack 2.3.2
Time: 573ms
  Asset  Size Chunks     Chunk Names
  main.js 340 bytes  0 [emitted]   main
vendor.js  274 kB  1 [emitted] [big] vendor
 [0] ./~/jquery/dist/jquery.js 267 kB {1} [built]
 [1] ./app/hello.js 53 bytes {0} [built]
 [2] ./app/index.js 114 bytes {0} [built]
 [3] multi jquery 28 bytes {1} [built]
```
最终发现我们成功将jQuery打包到了vendor.js中，实现了独立打包，但是问题又来了：每次打包后生成的文件名都是一样的，浏览器可能缓存上一次的结果而无法加载最新数据。

### 添加hash
为了解决上述问题，我们需要为打包后的文件名添加hash值，这样每次修改后打包的文件hash值将改变，修改配置文件如下：
``` bash
module.exports = {
 ...
  output: {
   filename: '[name].[chunkHash:5].js',
   path: path.resolve(__dirname, 'dist')
  },
 ...
}
```
上方我们在输出文件名中增加了[chunkHash:5]变量，表示打包后的文件中加入保留5位的hash值。我们再次运行打包命令：

``` bash
Hash: c7d1295f2f9a27c412d2
Version: webpack 2.3.2
Time: 603ms
   Asset  Size Chunks     Chunk Names
 main.2a7ad.js 337 bytes  0 [emitted]   main
vendor.49eb4.js  274 kB  1 [emitted] [big] vendor
 [0] ./~/jquery/dist/jquery.js 267 kB {1} [built]
 [1] ./app/hello.js 50 bytes {0} [built]
 [2] ./app/index.js 114 bytes {0} [built]
 [3] multi jquery 28 bytes {1} [built]
```
上方我们发现打包后的文件成功加上了hash值，这样每次修改文件后hash值也会跟着变，就不怕浏览器缓存了，但是当我们尝试去修改一个js文件后再次打包，问题又来了：vendor.js的hash值也变了，我们并没有修改jQuery的源码。

### 修改vendor配置
上述问题产生的原因是因为CommonsChunkPlugin插件是用于提取公共代码的，上方我们只是提取了vendor作为公共代码。为了继续解决上述问题，其实方法很简单，我们需要修改CommonsChunkPlugin的配置，如下：
``` bash
module.exports = {
 ...
  plugins:[
   new webpack.optimize.CommonsChunkPlugin({
    names: ['vendor', 'manifest']
   }),
  ]
 ...
}
```
如此我们修改一下hello.js中的代码，发现vendor的hash值并未改变，并且多了一个manifest.js的小文件。manifest.js为webpack的启动文件代码，它会直接影响到hash值，用mainfest单独抽出来了，这样vendor的hash就不会变了。

### 生成index.html
通过以上对webpack配置文件的一系列修改，我们成功实现了webpack的独立打包与缓存处理，但是还差最后一步。
因为我们最终打包后生成的文件名中带有hash值，每次都是会变的，所以我们不能像目前这样在index.html中写死路径。
``` bash
...
<body>
 <script src="./dist/main.js"></script>
 <script src="./dist/vendor.js"></script>
 <script src="./dist/manifest.js"></script>
</body>
...
```
以上写法是不对的，因为缺少了可变的hash值，因此我们希望每次打包后index.html中的路径也会自动加上hash值，解决方法如下：
``` bash
var HtmlWebpackPlugin = require('html-webpack-plugin');
 
module.exports = {
 ...
  plugins:[
   ...
   new HtmlWebpackPlugin({
    title: 'demo',
    template: 'index.html' // 模板路径
   }),
   ...
  ]
 ...
}
```
上方我们引入了html-webpack-plugin这一个插件，该插件可以帮助我们根据模板生成html文件。在plugins设置中，title配置了生成html中的title部分，template为模板html的路径地址。
我们需要下载html-webpack-plugin：
``` bash
npm install html-webpack-plugin --save-dev
```
安装和配置完毕后，运行打包命令：webpack
``` bash
Hash: 0c4b91e206579b31544d
Version: webpack 2.3.2
Time: 856ms
   Asset  Size Chunks     Chunk Names
 vendor.e1868.js  268 kB  0 [emitted] [big] vendor
 main.44412.js 337 bytes  1 [emitted]   main
manifest.ed186.js 5.81 kB  2 [emitted]   manifest
  index.html 292 bytes   [emitted]
 [0] ./~/jquery/dist/jquery.js 267 kB {0} [built]
 [1] ./app/hello.js 50 bytes {1} [built]
 [2] ./app/index.js 114 bytes {1} [built]
 [3] multi jquery 28 bytes {0} [built]
```
我们发现在dist目录下生成了一个index.html文件，打开该文件后代码如下：

``` bash
<!DOCTYPE html>
<html>
<head>
 <meta charset="UTF-8">
 <title>demo</title>
</head>
<body>
<script type="text/javascript" src="manifest.ed186.js"></script>
<script type="text/javascript" src="vendor.e1868.js"></script>
<script type="text/javascript" src="main.44412.js"></script>
</body>
</html>
```
至此我们实现了每次打包后index.html中的路径也会自动加上hash值的功能，因此dist目录下的index.html即为以后的首页文件，最后我们在浏览器中打开该文件成功显示：
