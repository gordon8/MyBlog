---
title: webpack学习笔记
date: 2018-05-06 22:37:24
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

## 多入口、多出口、html插件使用
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
使用方式
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

## devServer
[webpack-dev-server概述](https://www.npmjs.com/package/webpack-dev-server)
安装方式`npm install webpack-dev-server --save-dev`
### 使用方式
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

## loaders作用，处理css文件及压缩方式
loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件。

### 使用方式
* 配置（推荐）：在`webpack.config.js`文件中指定`loader`。
* 内联：在每个`import`语句中显式指定`loader`。
* CLI：在`shell`命令中指定它们。

### 配置[Configuration]
`module.rules`允许你在 webpack 配置中指定多个 loader。
使用方式
```javascript
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
```

### 内联
在 import 语句中指定 loader。使用 ! 将资源中的 loader 分开。通过前置所有规则及使用 !，可以对应覆盖到配置中的任意 loader。
使用方式`import Styles from 'style-loader!css-loader?modules!./styles.css';`

### CLI
使用方式`webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'`

### 处理css文件
使用[style-loader](https://www.npmjs.com/package/style-loader)、[css-loader](https://www.npmjs.com/package/css-loader)处理css文件
安装方式 `cnpm i style-loader css-loader -D`
使用方式
file.js
```javascript
import css from 'file.css';
```
webpack.config.js
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ] //使用顺序，从右到左
      }
    ]
  }
}
```

### 压缩方式
1. webpack4.x `webpack --mode production`
2. 之前版本使用[uglifyjs-webpack-plugin](https://www.npmjs.com/package/uglifyjs-webpack-plugin)
    安装方式 `cnpm i uglifyjs-webpack-plugin -D`
    使用方式
    ```javascript
    const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
     
    module.exports = {
      plugins: [
        new UglifyJsPlugin()
      ]
    }
    ```

## 处理图片，分离css
### 处理图片
使用[url-loader](https://www.npmjs.com/package/url-loader)、[file-loader](https://www.npmjs.com/package/file-loader)处理图片
安装方式 `cnpm i url-loader file-loader -D`
使用方式
file.js
```javascript
import img from './image.png';
```
webpack.config.js
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use:[{
          loader: 'url-loader',
          options: {
            limit: 5000,
            outputPath: 'images' //图片打包路径
          }
        }]
      }
    ]
  }
}
```
### 分离css
使用[extract-text-webpack-plugin](https://www.npmjs.com/package/extract-text-webpack-plugin)分离css
安装方式 
```shell
# for webpack 4
npm install --save-dev extract-text-webpack-plugin@next
# for webpack 3 
npm install --save-dev extract-text-webpack-plugin
# for webpack 2 
npm install --save-dev extract-text-webpack-plugin@2.1.2
# for webpack 1 
npm install --save-dev extract-text-webpack-plugin@1.0.1
```
使用方式
```javascript
const ExtractTextPlugin = require("extract-text-webpack-plugin");
 
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback : 'style-loader',
          use: 'css-loader',
          publicPath: '../' //解决css背景图路径问题
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin("css/index.css"),
  ]
}
```

## less、sass处理、前缀、消除冗余css
### less处理
使用[less](https://www.npmjs.com/package/less)、[less-loader](https://www.npmjs.com/package/less-loader)处理less
安装方式`npm install --save-dev less-loader less`
使用方式
```javascript
const ExtractTextPlugin = require("extract-text-webpack-plugin");
 
const extractLess = new ExtractTextPlugin({
    filename: "[name].[contenthash].css",
    disable: process.env.NODE_ENV === "development"
});
 
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.less$/,
            use: extractLess.extract({
                use: [{
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                }],
                // use style-loader in development
                fallback: "style-loader"
            })
        }]
    },
    plugins: [
        extractLess
    ]
};
```
### sass处理
使用[node-sass](https://www.npmjs.com/package/node-sass)、[sass-loader](https://www.npmjs.com/package/sass-loader)处理sass
安装方式`npm install sass-loader node-sass --save-dev`
使用方式
```javascript
const ExtractTextPlugin = require("extract-text-webpack-plugin");
 
const extractSass = new ExtractTextPlugin({
    filename: "[name].[contenthash].css",
    disable: process.env.NODE_ENV === "development"
});
 
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.scss$/,
            use: extractSass.extract({
                use: ['css-loader', 'sass-loader'],
                // use style-loader in development
                fallback: "style-loader",
                publicPath: '../' //解决css背景图路径问题
            })
        }]
    },
    plugins: [
        extractSass
    ]
};
```

### css前缀
使用[postcss-loader](https://www.npmjs.com/package/postcss-loader)、[autoprefixer](https://www.npmjs.com/package/autoprefixer)为css添加前缀
安装方式`npm install postcss-loader autoprefixer --save-dev`
使用方式
webpack.config.js
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader', 'postcss-loader']
      }
    ]
  }
}
```
postcss.config.js
```javascript
module.exports={
  plugins: [
    require('autoprefixer')
  ]
}
```

### 消除冗余css
使用[purifycss-webpack](https://www.npmjs.com/package/purifycss-webpack)、[purify-css](https://www.npmjs.com/package/purify-css)、[glob](https://www.npmjs.com/package/glob)去除冗余css
安装方式`npm i -D purifycss-webpack purify-css glob`
使用方式
webpack.config.js
```javascript
const path = require('path');
const glob = require('glob');
const ExtractTextPlugin = require('extract-text-webpack-plugin');
const PurifyCSSPlugin = require('purifycss-webpack');
 
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        loader: ExtractTextPlugin.extract({
          fallbackLoader: 'style-loader',
          loader: 'css-loader'
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin('[name].[contenthash].css'),
    // Make sure this is after ExtractTextPlugin!
    new PurifyCSSPlugin({
      // Give paths to parse for rules. These should be absolute!
      paths: glob.sync(path.join(__dirname, 'src/*.html')),
    })
  ]
};
```

## SourceMap、babel配置
### 配置[Devtool](https://doc.webpack-china.org/configuration/devtool)进行SourceMap调试
使用方式
webpack.config.js
```javascript
module.exports = {
  devtool: 'eval-source-map',
}
```

### 配置babel进行转译
安装方式`cnpm i babel-core babel-loader babel-preset-env -D`
使用方式
webpack.config.js
```javascript
module.exports = {
  module: {
    rules: [
      {
        test:/\.js$/,
        use: ['babel-loader'],
        exclude: /node_modules/
      }
    ]
  }
}
```
.babelrc
```javascript
{
  "presets": [
    "env"
  ]
}
```

## json配置、模块化配置、静态资源
### json配置
webpack2.0以前使用[json-loader](https://www.npmjs.com/package/json-loader)，之后版本默认配置
使用方式`const json = require('./file.json');`

### 模块化配置
与node中使用方式一致
导出：`module.exports = xxx`
引入：`require('./xxx');`

### 静态资源输出
使用[copy-webpack-plugin](https://www.npmjs.com/package/copy-webpack-plugin)进行静态资源输出
安装方式`npm i -D copy-webpack-plugin`
使用方式
```javascript
const CopyWebpackPlugin = require('copy-webpack-plugin');
module.exports = {
  plugins: [
    new CopyWebpackPlugin([{
      from:__dirname+'/src/assets',
      to:'./public'
    }])
  ]
}
```

## 优雅使用第三方库、提取js文件
### 优雅使用第三方库
以jquery为例
安装方式`cnpm install jquery -S`
使用方式
1. 通过import引入，无论是否使用都会打包进去（不推荐）
```javascript
import $ from 'jquery';

$('selector').css({});
```
2. ProvidePlugin引入，只有用到时才打包（推荐）
```javascript
const webpack = require('webpack');
module.exports = {
  plugins: [
    new webpack.ProvidePlugin([{
      $: 'jquery'
    }])
  ]
}
```

### 提取js文件
从webpack4开始`CommonsChunkPlugin`被移除，由`optimization.splitChunks`进行js文件提取。
使用方式
```javascript
module.exports = {
  optimization: {
    runtimeChunk: true,
    splitChunks: {
      cacheGroups: {
        commons: {
          test: /[\\/]node_modules[\\/]/,
          name: "jquery",
          chunks: "all"
        }
      }
    }
  }
}
```