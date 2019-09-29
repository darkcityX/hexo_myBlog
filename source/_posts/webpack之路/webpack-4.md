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


