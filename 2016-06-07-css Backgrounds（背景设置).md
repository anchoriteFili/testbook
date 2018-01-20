---
layout: post
#css Backgrounds（背景设置) 
title:  css Backgrounds（背景设置) 
#时间配置
date:   2016-06-07 23:14:22 +0800
#大类配置
categories: 知识
#小类配置
tag: css
---

* content
{:toc}

**相关链接：**

* <a href="http://www.runoob.com/css/css-background.html" target="_blank">css Backgrounds（背景设置)</a><br>
* <a href="http://www.runoob.com/cssref/css3-pr-background.html" target="_blank">CSS background 属性</a><br>





```css
body {
    /*
    background-image:url("http://c.hiphotos.baidu.com/image/pic/item/b3b7d0a20cf431ad799ccaf94f36acaf2fdd98a0.jpg");
    background-repeat:no-repeat; 
    background-position:right top; 
    margin-right:200px; 
    */
    /*为了简写这些属性的代码，我们可以将这些属性合并在同一个属性中*/
    /*顺序：background-color background-image background-repeat 
    background-attachment background-position*/
    background:#063 url(89%88.png) no-repeat left top;
    margin:200px;
    /*
    background: 简写属性，作用是将背景属性设置在一个声明中。
    background-attachment: 背景图像是否固定或者随着页面的其余部分滚动。
    background-color: 设置元素的背景颜色
    background-image: 把图像设置为背景。
    background-position: 设置背景图像的起始位置。
    background-repeat: 设置背景图像是否及如何重复
    repeat: 背景图像将向垂直和水平方向重复。这是默认
    repeat-x: 只用水平位置会重复背景图像。
    repeat-y: 只用垂直位置会重复背景图像。
    no-repeat: 图片不会重复。
    inherit: 指定background-repea属性设置应该从父元素继承。
    */
}
```