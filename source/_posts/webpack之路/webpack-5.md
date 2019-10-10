---
title: webpack之路(五)之代码质量管理-eslint
date: 2019-10-10 09:15:02
cover: /images/webpack/webpack_fengmian01.jpg
tags: webpack
categories: webpack
---
### 1.作用

代码风格的统一。

### 2.依赖下载

    npm i eslint eslint-loader -D

 - eslint: 核心
 - eslint-loader: 桥梁，让webpack可以认识eslint。

### 3.webpack.json.js中的eslint配置

```javascript

const path = require("path");
module.exports = {
    mode: 'development',
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname,"./bundle"),
        filename: "bundle.js"
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/i,
                loader: 'eslint-loader',
                exclude: /node_modules/,
                options: {
                    
                }
            }
        ]
    }
}

```

执行webpack运行：
![webpack思维导图一览图](/images/webpack/eslint01.jpg)

### 4.eslint代码规范规则配置(两种方法)

#### 1).执行node_modules中得eslint依赖包中的bin/eslint.js文件

如果在当前项目目录下，执行 

    node node_modules/eslint/bin/eslint.js

根据项目需求进行配置：

![webpack思维导图一览图](/images/webpack/eslint02.png)

```javascript
module.exports = {
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": "eslint:recommended",
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "rules": {
        
    }
};
```
eslint官网rules配置：https://eslint.bootcss.com/docs/rules/

#### 2).在webpack.json中进行命令行配置

注意：使用node直接执行eslint.js加路径是因为node不会自动去找eslint.js文件，而这就可以直接配置到webpack.json，webpack会根据依赖包直接去找到该文件。

```JSON
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --env.production",
    "start": "webpack-dev-server --env.development --open",
    "eslint_init": "eslint --init"
}
```
执行命令：

    npm run eslint_init

