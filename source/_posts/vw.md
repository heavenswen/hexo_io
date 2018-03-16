---
title: vw 视口单位自适应解决方案
date: 2018-03-01 17:23:12
tags: [CSS , postcss , webpack]
categories: [UI,插件]
---
## 简介

手机端自适应开发的新方案，vw单位能够根据屏幕占比自动适应内容，不依靠js快速响应,避免了js方案（rem，viewport操作）出现的回流想象。
>1. 当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流(reflow)。每个页面至少需要一次回流，就是在页面第一次加载的时候。在回流的时候，浏览器会使渲染树中受到影响的部分失效，并重新构造这部分渲染树，完成回流后，浏览器会重新绘制受影响的部分到屏幕中，该过程成为重绘。
>2. 当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。
>注意：回流必将引起重绘，而重绘不一定会引起回流。


### 参数

+ vw : 1vw 等于视口宽度的1%
    + 兼容 ie9+
+ vh : 1vh 等于视口高度的1%
    + 兼容 ie9+
+ vmin : 选取 vw 和 vh 中最小的那个
    + 不兼容ie
+ vmax : 选取 vw 和 vh 中最大的那个
    + 不兼容 ie

 #### Android 4.4之下和iOS8以下的版本都存有一定的问题
> vw兼容方案
> 在你的HTML文件，比如index.html中的 head 或 body引入下面的JavaScript文件：
>```
<scriptsrc="//g.alicdn.com/fdilab/lib3rd/viewport-units-buggyfill/0.6.2/??viewport-units-buggyfill.hacks.min.js,viewport-units-buggyfill.min.js"></script>

// 调用viewport-units-buggyfill的方法
<script>
window.onload = function() {
    window.viewportUnitsBuggyfill.init({
        hacks: window.viewportUnitsBuggyfillHacks
    });
}
</script>
>```

当建议依靠postcss-vw 解决方案，快速转换px单位，避免了计算的苦恼。
### 操作
依照设计稿 750 === 100vw 计算
```
.wh{
    font-size:2vw; 
}
// 750*2/100 15px

```
### 工具

#### webpack 
使用postcss-loader postcss-px2viewport 跟据设计稿生成vw单位。
```
num i postcss-loader
num i postcss-px2viewport
```
config

```
//postcss.config.js
module.exports = {
    plugins: [
        //px to vw
        // require('postcss-px2viewport')
    ],
    module: {
        loaders: [
            {
                test: /\.css$/,
                loader: "style-loader!css-loader!postcss-loader"
            }
        ]
    },
    postcss: function () {
        //px to vw 
        //css 标记
        // /*px*/的，则转换为[data-dpr="1"]、[data-dpr="2"]、[data-dpr="3"]三种不同的字体
        // /*no*/的，则不做处理，依然使用px进行布局
        // return [px2viewport({ viewportWidth: 750,viewportHeight:1334 })];
    }
}
```
##### 使用
运行webpack直接将css中px 转换成 vw 单位
```
body{
    font-size:14px;
}
```
转换
```
body{
  font-size: 1.8666667vw;      
}
```
在css中标记/*px*/则转换为[data-dpr="1"]、[data-dpr="2"]、[data-dpr="3"]三种不同的字体
```
.box{
    width:20px;/*px*/
}
```
转换
```
[data-dpr="1"] .box{
    width:10px;
}

[data-dpr="2"] .box{
    width:20px;
}

[data-dpr="3"] .box{
    width:30px;
}

```
在css 中标记了/*no*/则不转换
```
.box{
    font-size:12px;/*no*/
}
```
转换
```
.box{
    font-size:12px;
}
```



