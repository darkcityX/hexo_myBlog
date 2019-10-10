---
title: webpack之路(六)之代码单元测试-jest
date: 2019-10-10 12:28:40
cover: /images/webpack/webpack_fengmian01.jpg
tags: webpack
categories: webpack
---

### 1.作用

### 2.依赖下载

    npm i jest jest-webpack -D

 - jest : 核心库
 - jest-webpack : 

### 3.在webpack.json中添加jest命令

```json
    "scripts": {
        "jest" : "jest-webpack"
    }
```

执行npm run jest ，会报错，这是因为使用测试单元需要测试用例。

### 4.使用jest进行单元测试

#### 1).需要测试的单元

```javascript
// 需要测试的单元 index.js
export function fab(n){
    if( n==1 || n==2 ){
        return 1;
    }else{
        return fab(n-1)+fab(n-2);
    }
}
```
#### 2).写测试用例

在当前目录下新创建tests文件夹(当执行npm run jest时，会默认去找tests文件)，在文件夹中创建针对当前需要测试的单元新创建相关名称的测试用例（index.test.js）

```javascript

```


## 未完成，待续。
