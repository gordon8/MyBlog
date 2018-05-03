---
title: webpack学习笔记
date: 2018-05-02 10:57:24
tags:
- 前端
- webpack
---

## webpack是什么？
一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。）,详见[webpack官网](https://webpack.js.org/)

## webpack作用
1. 打包（解决依赖关系，把多个文件打包成一个js文件，减少服务器压力带宽）
2. 转化（less、sass、ts） 需要loader
3. 优化（SPA盛行，前端项目复杂度高，对项目进行优化）

<!-- more -->

## webpack构成
1. 入口 entry
2. 出口 output
3. 转化器 loaders
4. 插件 plugins
5. 开发服务器 devServer
6. 环境模式 mode

## 安装webpack
1. 先确保node环境已经安装；
2. 初始化项目`npm init -y`；
3. 安装webpack: `npm install webpack-cli -g`(全局) `npm install webpack-cli --save-dev` (项目)，开发时建议安装dev；
4. 验证webpack环境已经ok? `webpack -v`

## 环境
1. 开发环境：(development) 
    编写代码的环境
    npm i webpack --save-dev
    npm i webpack --D
    npm i webpack-cli --D
2. 生产环境：(production) 项目开发完毕，部署上线
    npm i vue -save
    npm i vue -S
模式设置：`webpack --mode development`； `webpack --mode production`。
    
## weppack配置文件
`weppack.config.js`采用commonJS规范，示例：
```javascript
const path = require('path');

module.exports = {
  // 模式
  mode: 'development', // "production" | "development" | "none"
  //入口配置
  entry:{ // string | object | array
    a: './src/index.js'
  },
  //出口配置
  output:{
    path: path.resolve(__dirname, 'dist'), // string,必须绝对路径  __dirname + '/dist
    filename: './bundle.js' // string， 多入口 "[name].js"
  },
  //模块配置
  module: {
    rules: [ // 模块规则（配置 loader、解析器等选项）
      
    ]
  },
  //本地服务器
  devServer: {
    
  },
  // 插件列表
  plugins: [
    
  ]
  
}
```
运行方式：
1. 运行`webpack --config hgd/config.js`；
2. 修改`package.json`，script属性添加
    ```  
    "scripts": {
      "bulid": "webpack --config hgd.config.js"
    },
    ```，
    运行`npm run bulid`。

配置文件名可修改，例如：若修改为`hgd.config.js`；运行时应运行`webpack --config hgd/config.js`

### 多入口、多出口、html插件使用
1. 多入口配置
```javascript
  //入口配置
  entry: {
    aaa: './src/index.js',
    bbb: ['./src/index2.js', './src/index3.js']
  }
```

2. 多出口配置（配合html-webpack-plugin使用）
[html-webpack-plugin插件](https://www.npmjs.com/package/html-webpack-plugin)
安装方式`npm i --save-dev html-webpack-plugin`
```javascript
  const HtmlWebpackPlugin = require('html-webpack-plugin')
  // webpack config
  {
    //出口配置
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: '[name].bundle.js'
    },
    plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        chunks: ['aaa'], //多页面分别引入的js
        filename: 'index.html', //分别打包的文件名
        title: 'wangka', //页面标题
        template: './src/index.html' //页面模板
      }),
      new HtmlWebpackPlugin({
        chunks: ['aaa','bbb'],
        filename: 'index2.html',
        title: 'wangka2',
        template: './src/index.html'
      })
    ]
   }
```
3. 删除dist目录插件[clean-webpack-plugin](https://www.npmjs.com/package/clean-webpack-plugin)
安装方式`npm i clean-webpack-plugin --save-dev`
使用方式
```javascript
const CleanWebpackPlugin = require('clean-webpack-plugin')
 
// webpack config
{
  plugins: [
    new CleanWebpackPlugin(['dist']) //path路径
  ]
}
```

### devServer
[webpack-dev-server概述](https://www.npmjs.com/package/webpack-dev-server)
安装方式`npm install webpack-dev-server --save-dev`
使用方式
```javascript
const webpack = require('webpack');

// webpack config
{
  devServer: {
    contentBase: path.resolve(__dirname, "dist"), //设置服务器访问的基本目录
    host: 'localhost', // 服务器ip地址
    compress: true, //是否压缩
    port: 9000, //端口
    open: true, //自动打开浏览器
    hot: true //模块热加载
  },
  plugins: [
      new webpack.HotModuleReplacementPlugin() //模块热加载
  ]
}

// package.json
{
  "scripts": {
    "dev": "webpack-dev-server"
  }
}
```



