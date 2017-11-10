---
title: create-react-app项目添加less/scss配置
date: 2017-11-09 15:10:24
tags: react
---

使用create-react-app 创建的项目默认不支持less scss，以下增加less scss配置的步骤

### 暴露配置文件

```
npm run eject
```

### 安装less-loader 和 less

```
npm install less-loader less --save-dev
```

#### 修改webpack配置

修改 webpack.config.dev.js 和 webpack.config-prod.js 配置文件

```
{
  test: /\.(css|less)$/,
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
      },
    },
    {
      loader: require.resolve('postcss-loader'),
      options: {
        // Necessary for external CSS imports to work
        // https://github.com/facebookincubator/create-react-app/issues/2677
        ident: 'postcss',
        plugins: () => [
          require('postcss-flexbugs-fixes'),
          autoprefixer({
            browsers: [
              '>1%',
              'last 4 versions',
              'Firefox ESR',
              'not ie < 9', // React doesn't support IE8 anyway
            ],
            flexbox: 'no-2009',
          }),
        ],
      },
    },
    {
      loader: require.resolve('less-loader') // compiles Less to CSS
    }
  ],
},
```


### 安装scss-loader 和 scss

```
cnpm install sass-loader node-sass --save-dev
```

#### 修改webpack配置

修改 webpack.config.dev.js 和 webpack.config-prod.js 配置文件

```
{
    loader: require.resolve('file-loader'),
    // Exclude `js` files to keep "css" loader working as it injects
    // it's runtime that would otherwise processed through "file" loader.
    // Also exclude `html` and `json` extensions so they get processed
    // by webpacks internal loaders.
    exclude: [/\.js$/, /\.html$/, /\.json$/,/\.scss$/],
    options: {
         name: 'static/media/[name].[hash:8].[ext]',
    },
},
{
    test: /\.scss$/,
    loaders: ['style-loader', 'css-loader', 'sass-loader'],
}
```

到此，配置完毕，记得两个文件都要改。