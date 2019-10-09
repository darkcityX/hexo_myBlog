---
title: webpack之路(四)之dev-server
date: 2019-09-29 16:07:49
cover: /images/webpack/webpack_fengmian01.jpg
tags: webpack
categories: webpack
---

#### 一、dev-server

##### 1.什么是dev-server？
    用于开启开发服务器,只用于开发阶段。生产环境可用其它的常见的进行替代。

 - 提供网络环境————cookie等 
 - 热更新

##### 2.如何安装dev-server？
    npm i webpack webpack-cli webpack-dev-server
    
 - webpack: webpack核心，dev-server只有webpack才能跑起来
 - webpack-cli: dev-server中的命令需要通过webpack-cli调用，实质是是命令行的工具
 - webpack-dev-server: dev-server核心库

##### 3.webpack.json中添加命令
    "start": "webpack-dev-server"  // 启动命令： npm run start

其本质是：依据webpack-cli去找webpack-dev-server执行启动。而webapck.json会自动执行node_modules中的依赖，所以会忽略掉webpack-cli。这是正是webpack-dev-server在命令行中输入启动报错的原因。

注意1：在启动了热更新后，编译后的文件只存在于内存当中，而不会去打包成本地文件，其默认路径为打包文件的根目录下，文件名直接为filename中定义的文件名。所以在index.html中引用的文件就修改成/filename(filename中所定义的文件名).js。否则热更新配置完成后，webpack中看着会执行编译，但在页面中却不会进行更新展示。

注意2：启动热更新后，其只针对源文件，如果webpack.config.js或index.html文件发生变化，则需要重新启动，才能正常运行。

---
    ！！！此时，为了兼容生产环境和编译环境的操作，我们就需要进行两套配置的操作了。
---

#### 二、webpack双配置

##### 1.以函数返回值形式对两套配置进行整改
```javascript
const path = require("path");

/**
 * 以函数形式对外输出
 * env:  环境配置
 * argv: 所有的选项，也包括默认选项
 */
module.exports = function( env , argv ){
    console.log( env );
}
```
    此时，webpack的编译是可以携带参数的。在命令行中输入 webpack --env.development  /  webpack --env.production,可以分别接收到生产环境和编译环境的参数，所以我们可以直接对webpack.json中的命令进行修改。

##### 2.修改webpack.json中的快捷启动命令，使其携带默认参数
```JSON
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --env.production", // 注意，build中env.production是默认的，所以可以忽略不写。
    "start": "webpack-dev-server --env.development"
},
```

##### 3.配套形成及其缺陷

```javascript
const path = require("path");

/**
 * 以函数形式对外输出
 * env:  环境配置
 * argv: 所有的选项，也包括默认选项
 */
module.exports = function( env , argv ){
    // 如果不携带env参数，则默认为undefined
    env = env || {development: true};
    console.log( env );

    if( env.production ){
        console.log( "进入生产环境" );
        return{// 返回生产环境的配置

        }
    }else{
        console.log( "进入编译环境" ); 
        return{// 返回编译环境的配置

        }
    }
}
```
提示：如果两套配置写在同一个文件中比较难以维护。所以可以分开写到两个文件之中。 

##### 4.分文件形式对配置代码进行整改

创建config文件夹，在其目录下分别创建webpack.development.js文件及webpack.production.js文件.

###### 1). package.json文件配置

start命令中的--open是指打开默认浏览器

```JSON
{
    "name": "webpack02",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "build": "webpack --env.production",  
        "start": "webpack-dev-server --env.development --open"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "@babel/core": "^7.6.3",
        "@babel/preset-env": "^7.6.3",
        "autoprefixer": "^9.6.4",
        "babel-loader": "^8.0.6",
        "css-loader": "^3.2.0",
        "file-loader": "^4.2.0",
        "less": "^3.10.3",
        "less-loader": "^5.0.0",
        "postcss-loader": "^3.0.0",
        "style-loader": "^1.0.0",
        "url-loader": "^2.2.0",
        "webpack": "^4.41.0",
        "webpack-cli": "^3.3.9",
        "webpack-dev-server": "^3.8.2"
    },
    "browserslist": [
        "> 1%",
        "last 5 version"
    ]
}

```

###### 2). webpack.config.js文件配置

```javascript
/**
 * 以函数形式对外输出
 * env:  环境配置
 * argv: 所有的选项，也包括默认选项
 */
module.exports = function( env , argv ){
    // 如果不携带env参数，则默认为undefined
    env = env || {development: true};

    return {
        // mode
        ...env.production ? require("./config/webpack.production") : require("./config/webpack.development"),
        
        // 入口配置
        entry: "./src/js/index.js",

        // loader配置
        module: {
            rules: [
                // css文件处理
                {
                    test: /\.css$/i,
                    use: [
                        "style-loader",
                        "css-loader",
                        {
                            loader: "postcss-loader",
                            options: {
                                plugins: [
                                    require("autoprefixer")
                                ]
                            }
                        }
                    ]
                },
                // less文件处理
                {
                    test: /\.less$/i,
                    use: [
                        "style-loader",
                        "css-loader",
                        {
                            loader: "postcss-loader",
                            options: {
                                plugins: [
                                    require("autoprefixer")
                                ]
                            }
                        },
                        "less-loader"
                    ]
                },
                // 图片文件处理
                {
                    test: /\.(jpg|jpeg|png|git)$/i,
                    use: {
                        loader: "url-loader",
                        options: {
                            outputPath: 'imgs/',
                            // publicPath: "dist/imgs", 
                            limit: 4*1024
                        }
                    }
                },
                // js文件es6语法兼容处理
                {
                    test: /\.js|jsx$/i,
                    use: {
                        loader: 'babel-loader',
                        options: {
                            presets: ['@babel/preset-env']
                        }
                    }
                }

            ]
        },
        "devtool": "source-map"
    }
}
```

###### 3). config文件下的webpack.development.js配置

```javascript
// 编译环境文件相关配置
module.exports = {
    mode: 'development',
    // 在编译环境中不会生成实体文件，只会存储在内存之中，所以不需要path
    output: {
        filename: 'bundle.js'
    }
}
```
###### 4). config文件下的webpack.production.js配置

```javascript
// 生产环境相关配置
const path= require("path");

module.exports = {
    mode: "production",
    output: {
        path: path.resolve(__dirname,"../build"),
        filename: 'bundle.min.js'
    },
}
```

    !!!注意：此时就剩index.html中的引用主文件的引用问题及打包后的文件目录中没有index.html的问题了。


##### 5.动态生成html模板插件html-webpack-plugin

    npm i html-webpack-plugin -D

作用：根据某类模板创建新的html文件。
注意：在模板的index.html中就不需要再进行bundle.js文件的引用了，html-webpack-plugin会根据webpack打包自动生成js引用。

###### 1).config文件下的webpack.development.js配置

```javascript
// 编译环境文件相关配置
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
    mode: 'development',
    // 在编译环境中不会生成实体文件，只会存储在内存之中，所以不需要path
    output: {
        filename: 'bundle.js'
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: path.resolve(__dirname,"../index.html")
        })
    ]
}
```

###### 2). config文件下的webpack.production.js配置

```javascript
// 生产环境相关配置
const path= require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: "production",
    output: {
        path: path.resolve(__dirname,"../build"),
        filename: 'bundle.min.js'
    },
    plugins: [
        new HtmlWebpackPlugin({
            // 执行文件为webpack.config.js文件，需要更换，使用绝对路径更好点
            template: path.resolve(__dirname,'../index.html')
        })
    ]
}
```