---
title: 一、MVC与MVVM概念
date: 2019-10-11 10:37:10
cover: /images/web/web01.jpg
tags: 前端概念
categories: 前端概念
---
### 1.MVC模式和MVVM模式的进程

mvc出现的较早，当时的前端并不怎么成熟，很多业务逻辑都是由后端实现的，所以前端并没有真正意义上的MVC模式。随着大前端技术的不断发展与成熟，越来越多的服务端代码开始向浏览器中转移，这就产生了成千上万行的javascript代码，它们连接了各式各样HTML和CSS文件，但由此而来，也产生相应的问题————缺乏正规的组织形式。随之而来。也多了越来越多的javascript框架。其中比较出名的就是JQuery，但这类库并没有实现对业务逻辑的分成，所以维护性及扩展性极差。当浏览器兼容性不在成为前端阻碍的时候。前端项目的体量也越来越大，项目的可维护性、扩展性、安全性成为主要原因。也因此而来MVVM模式一类的框架诞生。如vue/React/angule。

----

### 2.MVC模式(Model-View-Controller：模型-视图-控制器)简介
 - Model，模型。 指的是后端传递给前端的数据。
 - View ，视图。 指的是前端根据数据所展示的前端页面。
 - Controller，控制器。 指的是页面的业务逻辑。

总结：使用MVC的目的在于将M与V进行分离，即数据与页面样式的分离。MVC是单向通信，也就是View和Model必须通过Controller来承上启下。

----

### 3.MVVM模式(Model-View-ViewModel:模型-视图-视图模型)简介
 - Model，模型。指的是后端传递给前端的数据。
 - View ，视图。指的是前端根据数据所展示的前端页面。
 - ViewModel，视图模型。 其是MVVM模式的核心，是连接View和Model的桥梁。其有两个方向的作用：
&nbsp;&nbsp;&nbsp;&nbsp;1).将模型转化为视图。即将后端传递的数据转化为前端所需要展示的页面。实现的方式是数据绑定。
&nbsp;&nbsp;&nbsp;&nbsp;2).将视图转换成模型。即将所看到的页面转换为后端的数据。实现的方式是DOM事件监听。

这两个方向都实现，称之为双向数据绑定。

总结：在MVVM的模式下，视图和模型是不能直接通信的。它们通过ViewModel来通信，ViewModel通常要实现一个observer(观察者)的角色。当数据发生变化，ViewModel能够监听到数据的变化，然后通知到相应的是同做自动更新。而当用户操作视图，ViewModel也能监听到视图的变化，然后通知数据做改动。这实际上就实现了数据的双向绑定，并且MVVM中的View和ViewModel是可以相互通信的。

### 4.MVC模式与MVVM模式的区别
两者的区别并不在于VM完全替代了C，ViewModel存在的目的在于抽离Controller中展现的业务逻辑部分，而不是替代Controller，其它部分的视图操作业务还是放在Controller中实现的，也就是说MVVM实现的是业务逻辑组件的重用。




