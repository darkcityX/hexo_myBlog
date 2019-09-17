---
title: 项目中css常用代码
date: 2019-09-05 12:26:44
cover: /images/css/css_fengmian01.jpg
tags: css
categories: css
---

### 一、背景图平铺效果
#### 1、需要该效果达到一下需求：
    1).图片以背景的形式铺满整个屏幕，不留空白区域
    2).保持图像的纵横比（图片不变形）
    3).图片居中
    4).不出现滚动条
    5).多浏览器支持

#### 2、代码：
```css
.bg{
    background: url(bg.jpg) no-repeat center center fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
}
```
#### 3、弊病：
+ 兼容性较差，不兼容ie8以下的版本
+ Safari 3+
+ Chrome
+ IE 9+
+ Opera 10+ (Opera 9.5 支持background-size属性 但是不支持cover)
+ Firefox 3.6+


