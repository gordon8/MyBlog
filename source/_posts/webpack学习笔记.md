---
title: webpack学习笔记
date: 2018-05-02 10:57:24
tags:
- 前端
- webpack
---

## webpack是什么？
前端打包工具（模块打包器）

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
  //入口配置
  entry:{
    a: './src/index.js'
  },
  //出口配置
  output:{
    path: path.resolve(__dirname, 'dist'), //必须绝对路径  __dirname + '/dist
    filename: './bundle.js'
  },
  //
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



