---
title: 常用css样式类
date: 2019-09-05 11:37:00
cover: /images/css/css_fengmian01.jpg
tags: css
categories: css
---

### 一、文本溢出处理
#### 代码：
```css
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
```

### 二、文本溢出处理
#### 代码：
```css
    text-overflow: -o-ellipsis-lastline;
    overflow: hidden;
    text-overflow: ellipsis;   // 溢出隐藏
    display: -webkit-box;    // 盒模型
    -webkit-line-clamp: 2;   // 设置省略行数
    -webkit-box-orient: vertical;
```

### 三、css绘制三角形
#### 代码：
```css
    width: 0;                   //0可以去掉块级元素的宽度
    border:  55px solid red;   //设置边框，默认是所有边框都有值，所以此时会显示正方形
    border-color: transparent; //用透明色，会使块级元素在视觉中消失
    border-bottom-color: green;//给bottom边框添加颜色为正三角、给top添加为下三角...
```

