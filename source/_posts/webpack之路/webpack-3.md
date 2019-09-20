---
title: webpack之路(三)之loader
date: 2019-09-20 00:29:34
cover: /images/webpack/webpack_fengmian01.jpg
tags: webpack
categories: webpack
---

### 一、loader介绍

    默认的webpack只能处理js及json相关格式的文件，loader的出现可以让webpack处理两者以外的文件，其原理是将各种格式的文件打包进入js文件中。

### 二、相关loader 介绍

#### 1、style-loader、css-loader
    npm style-loader css-loader -D
- css-loader: 用于读取和解析css文件,在编译时不报错，将其解析成js文件。
- style-loader: 用于将样式代码输出到一个style标签中。

```webpack
    const path = require("path");

    moudle.exports = {
        mode: "development"
        entry: "./src/js/index.js",
        output: {
            path: path.resolve(__dirname,"dest"),
            fildname: 'bundle.min.js'
        },
        /* - module: 模块 - */
        moudle: {
            rules: [
                {
                    test: /\.css$/i, 
                    use: ['style-loader','css-loader']  // 从右往左执行
                }
            ]
        }
    }
```


