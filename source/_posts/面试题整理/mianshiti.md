---

title: 面试题整理
date: 2019-09-18 16:37:13
cover: /images/mianshiti/mianshiti_fengmian02.jpg
tags: 面试题整理
categories: 面试题整理

---

### 一、前端整体相关面试题整理

### 二、前端性能优化相关面试题整理

### 三、HTML面试题整理

### 四、css及css预处理面试题整理


#### 1、元素如何居中定位？

    垂直水品居中的办法挺多。我做的网站比较偏向移动端，所以支持性都比较好。主要使用的是translate居中定位。经测试，发现只适用与IE9及其以上版本。
```css
.centered{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```

#### 2、flex布局中如何居中定位？
    flex布局也是可以直接垂直水平居中的，需要父元素设置高度。经测试，只兼容IE10及其以上版本，代码如下：

```HTML

    <div class="box">
        <section class="inner">2222</section>
    </div>

```

```css
    .box {
        display: flex;
        justify-content: center; /* 水平居中 */
        align-items: center;     /* 垂直居中 */
        /* width: 1000px; */
        height: 600px;
        border: 1px solid red;
    }
    .inner {
        /* width: 300px;
        height: 200px; */
        background-color: red;
    }

```

#### 3、如何使用css画三角形？

```css
.triangle{
    width: 0;                   /* - 0可以去掉块级元素的宽度 - */
    border:  55px solid red;    /* - 设置边框，默认是所有边框都有值，所以此时会显示正方形 - */
    border-color: transparent;  /* - 用透明色，会使块级元素在视觉中消失 - */
    border-bottom-color: green; /* - 给bottom边框添加颜色为正三角、给top添加为下三角 - */
}
```

#### 4、如何处理单行文本溢出缩略显示、多行文本溢出缩略显示？
    单多行文本缩略显示都可以使用纯css处理(IE浏览器中通常需要js动态计算处理)，也可以使用js动态计算进行处理。我常用的方法是纯css进行处理。代码如下
```css
    .single_ thumbnail{
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }

    .more_thumbnail{
        /* 该种只适用于移动端，IE上是不支持的 */
        text-overflow: -o-ellipsis-lastline;
        overflow: hidden;
        text-overflow: ellipsis;   /* 溢出隐藏 */ 
        display: -webkit-box;    /* 盒模型 */ 
        -webkit-line-clamp: 2;   /* 设置省略行数 */ 
        -webkit-box-orient: vertical;
    }
    // or
    .more_thumbnail{
        /* 该种只适用于移动端，IE上是不支持的 */
        width: 150px;
        display: -webkit-box;  /* 该属性在IE上不支持 */
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 3;
        overflow: hidden
    }
```


### 五、第三方UI库面试题整理

### 六、js面试题整理(原生js及ES6)


#### 1、原生js中的this指向问题？(这个问题会在原生javascript进行专题研究)

    转自： https://www.cnblogs.com/pssp/p/5216085.html  
    
    this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象(这句话具体理解会有些错误)。
四条定律：  
  + 函数执行，首先看函数名前面是否有"."，有的话"."前面是谁就是谁；没有的话this就是window;
  + 自执行函数中的this永远是window
  + 给元素的某一个事件绑定方法，当事件触发的时候，执行对应的方法，方法中的this是当前的元素
  + 在构造函数模式中，类中（函数体中）出现的this.xxx = xxx;中的this是当前类的一个实例




#### 2、原生js中call/apply/bind的作用及区别？

#### 3、es6中的this指向问题？

#### 4、ES6中类的关键字super的原理？



### 七、第三方函数库(jq、zepto)面试题整理

#### 1、jquery中ajax回调中this的指向了谁？

### 八、vue框架相关面试题整理

### 九、第三方插件库相关面试题

#### 1、axios原理介绍

### 十、前后端交互相关面试题整理

#### 1、客户端token失效后应该怎么处理？


