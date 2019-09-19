---
title: webpack之路(二)之基础相关
date: 2019-09-05 15:24:45
cover: /images/webpack/webpack_fengmian01.jpg
tags: webpack
categories: webpack
---

### 一、webpack的相关信息：
#### 1.webpack的概念：
    webpack是一个自动化开发工具,可以帮助开发人员做很多重复性的工作.

#### 2.webpack的作用：
    - 压缩代码
    - 打包：一堆文件打包到一个文件中。
    - 多种文件的编译：将多种文件编译成浏览器可以识别的代码。如less、es6中的import等...
    - 脚手架：在项目创建初始，已经帮助开发人员搭建好了项目结构.
    - 生成： 可以将开发代码生成生产版本的代码包.

#### 2.webpack的安装：

    npm i webpack-cli -g

### 二、webpack的基本使用:

#### 1、相关文件及属性讲解

1). npm init -y  

    创建package.json : 存储相关依赖

2). 手动创建 webpack.config.js   

    是webpack的核心文件，对webpack的各种工作机制进行相关配置。

```webpack
// 单文件配置：

    // 引入node模块：处理路径的。
    const path = require("path");

    modeule.exports = {
        /**
         * 1. 模式：影响webpack本身的功能。在js输出时，可以使用【 process.env.NODE_ENV 】。其值有三类：
         *      development : 开发模式。会保留最完整的调试信息，不会对文件进行压缩，方便开发。
         *      production : 生产模式。会进行压缩。
         *      none: 无。
        */
        mode: 'development'
        /**
         * 2. 入口文件： 告诉webpack从哪个文件开始编译。可以是多文件也可以是单文件。
         *      注意：在本地引入的时候，最后加  ./   这遵循的是node的处理文件的方式。
        */
        entry: './src/index.js',
        /**
         * 3. 出口文件(打包出口)。该配置为json
         *    path   打包后的存储路径。 该处webpack强制要求必须使用绝对路径
         *    filename   打包后的文件名
        */
        output: {
            path: path.resolve(__dirname,'dest'),
            filename: 'bundle.min.js'
        }
    }

```

```webpack
//  多文件配置：
const path = require("path");

moudle.exprots = {
    mode: 'development',
    entry: {
        index: './src/index',
        news: './src/news',
    }
    /**
     * 该处的 [name] 会对 entry中的键index/news进行替换，从而生成相对应的文件名。
    */
    output:{
        path: path.resolve(__dirname,"dest"),
        filename: '[name].bundle.min.js'
    }
}

```
    

