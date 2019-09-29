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

#### 2、postcss-loader、autoprefixer
    npm i autoprefixer postcss-loader

postcss-loader: 让webpack解析、处理css文件的loader。专门用来给浏览器加前缀的loader。
autoprefixer: 内置了标准的前缀标准表，自动根据浏览器支持情况、判断是否需要补全前缀(>5%原则)。

    注意：autoprefixer不是loader，是插件————用于增强webpack的功能，而非直接参与文件处理

##### 1).使用postcss-loader的webpack配置：
```javascript
    const path = require("path");

    module.exports = {
        mode: "development",
        entry: "./src/js/index.js",
        output: {
            path: path.resolve(__dirname,"dest"),
            filename: 'bundle.min.js'
        },
        /* - module: 模块 - */
        module: {
            rules: [
                {
                    test: /\.css$/i, 
                    use: ['style-loader','css-loader','postcss-loader']  // 从右往左执行
                }
            ]
        }
    }
```
##### 2).autoprefixer引用配置

2.1)、写法1：在项目中创建postcss.config.js文件：
```javascript
    module.exports = {
        plugins:[
            require("autoprefixer")   
        ]
    }
```

2.2)、写法2：在webpack.config.js文件中直接进行配置:
```javascript
    const path = require("path");

    module.exports = {
        mode: "development",
        entry: "./src/js/index.js",
        output: {
            path: path.resolve(__dirname,"dest"),
            filename: 'bundle.min.js'
        },
        /* - module: 模块 - */
        module: {
            rules: [
                {
                    test: /\.css$/i, 
                    use: [
                        'style-loader',
                        'css-loader',
                        {
                            loader: 'postcss-loader',
                            options: {
                                plugins:[
                                    require("autoprefixer")   
                                ]
                            }
                        }
                    ] 
                }
            ]
        }
    }
```
    使用 npx autoprefiexer --info  可以查看浏览器的支持情况 
##### 3).autoprefixer参数相关配置：
3.1)、写法1：配置添加前缀参数文件
    创建新的文件 .browwserslistrc  文件。如果进行配置了，在调用postcss-loader时，会自动寻找该文件，进行配置打包。
```text
    last 5 version  // 支持浏览器向下的5个版本
    > 1%            // 用户量大于 1%
```

3.2)、写法2：在package.json中的browserslist对象中直接进行配置
```json
{
  "name": "webpack_demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "autoprefixer": "^9.6.1",
    "css-loader": "^3.2.0",
    "jquery": "^3.4.1",
    "postcss-loader": "^3.0.0",
    "style-loader": "^1.0.0"
  },
  "browserslist":[
      "last 5 version",
      "> 1%"
  ]

}

```


#### 3、file-loader、url-loader

##### 3.1). file-loader

    npm i file-loader -D

---

    file-loader: 将文件(eg:图片文件、字体文件...)也编译到js种(包括直接引用、也包括css引用)

```javascript

const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname,'dest'),
        filename: 'bundle.min.js'
    },
    module: {
        rules: [
            {
                test: /\.(img|png|image|gif)$/i,
                use: {
                    loader: 'file-loader',
                    options: {
                        outputPath: 'imgs/',        // 相当于打包后的相对路径
                        publicPath: 'dest/imgs/'    // 输出到css中的路径
                    }
                }                
            }
        ]
    }
}

```

##### 3.2). url-loader

    npm i url-loader -D

---

    注意：功能和作用和file-loader类似，但多了options中的参数设置，可以对文件大小等进行参数设置处理。在使用url-loader时，也需要引入file-loader，因为url-loader是基于file-loader的。

```javascript
const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname,'dest'),
        filename: 'bundle.min.js'
    },
    module: {
        rules: [
            {
                test: /\.(img|png|image|gif)$/i,
                use: {
                    loader: 'url-loader',
                    options: {
                        outputPath: 'imgs/',        // 相当于打包后的相对路径
                        publicPath: 'dest/imgs/',   // 输出到css中的路径
                        limit: 8*1024               // 大于8*1024的依旧使用file-loader在打包时存储为图片、小于的以base64进行处理
                    }
                }                
            },
            // 处理字体图标的文件: 与上面的是可以合并的
            {
                test: /\.(eot|svg|ttf|woff|woff2)/i,
                use: {
                    loader: 'url-loader',
                    options: {
                        output: 'fonts/',
                        publicPath: 'dest/fonts/',
                        limit: 4*1024
                    }
                }
            }
        ]
    }
}

```

#### 4、less-loader
    npm i less-loader less -D
---
  less-loader: 让webpack可以访问less的功能，并不具备解析less的功能。
  
  less: less文件的核心文件、可以解析less语法。 

``` javascript

const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname,'dest'),
        filename: 'bundle.min.js'
    },
    module: {
        rules: [
            {
                test: /\.less$/i,
                use: [
                    'style-loader',
                    'css-loader',
                    {
                        loader: 'postcss-loader',
                        options: {
                            plugins: [
                                require("autoprefixer")
                            ]
                        }
                    },
                    'less-loader'
                ]
            } 
        ]
    }
}

```

#### 5、babel-loader

  npm i babel-loader @babel/core @babel/preset-env -D

---
babel-loader：

@babel/core: 

@babel/preset-env:

```javascript

const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname,'dest'),
        filename: 'bundle.min.js'
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/i,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            }
        ]
    }
}

```
注意：当文件报错后，调试起来比较麻烦，看到的是编译后的代码,这就引申出了sourcemap。

### 三、开启sourcemap
    开启调试工具。现在浏览器都自带的一个解析.map的文件的调试工具
 - 打开后会自动生成一个.map的文件
 - 浏览器在解析文件时，会优先寻找.map的文件进行解析

webpack.config.js
```javascript

const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname,'dest'),
        filename: 'bundle.min.js'
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/i,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            }
        ]
    },
    // 开启调试工具
    devtool:'source-map'
}

```

![webpack思维导图一览图](/images/webpack/webpack_babel1.png)
---
![webpack思维导图一览图](/images/webpack/webpack_babel2.jpg)


