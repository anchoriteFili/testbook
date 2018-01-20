---
layout: post
#标题
title:  HTML中position属性
#时间配置
date:   2015-11-24 16:19:12 +0800
#大类配置
categories: 知识
#小类配置
tag: html
---

* content
{:toc}

#### position的四个属性值：

> 1. relative (相对的)
> 
> 2. absolute (绝对的)
> 
> 3. fixed (固执的)
> 
> 4. static (静态的)


##### 1. relative (相对的)

```html
<div id="parent">

　　<div id="sub1">sub1</id>

　　<div id="sub2">sub2</id>

</div>
```

> relative属性相对比较简单，我们要搞清它是相对那个对象进行偏移的。答案是它本身的位置。在上面的代码中，
>
> sub1和sub2是同级关系，如果设定sub1一个relative属性，比如设置如下CSS代码：

```css
#sub1

{

　　position:relative;

　　padding:5px; <!-pad:填补->

　　top:5px;

　　left:5px;

}
```

> 我们可以这样理解，如果不设置relative属性，sub1的位置按照正常的文档流，它应该处于某个位置。但当设置sub1的postion为

> relative后，将根据top,right,botton,left的值按照它理应所在的位置进行偏移，relative的“相对的”意思也正体现于此。
>
> 对于此，您只需要记住，sub1如果不设置relative时，它应该在那里，一旦设置后就按照它理应在的位置进行偏移。
>
> 随后的为题是，sub2的位置又在那里呢？答案是它原来在哪里，现在就在哪里，他的位置不会因为sub1增加粒postion的属性而发生改变。
>
> 注意：relative的偏移量是基于对象的margin的左上侧的。margin-边缘

##### 2. absolute (绝对的)

> 这个属性总是有人给出误导。说当position属性设为absolute后，总是按照浏览器窗口来进行定位的，这其实是错误的。实际上，这是fixed
属性的特点。

**当sub1的position设置为absolute后，其到底以谁为对象进行偏移呢？这里分为两种情况：**

> （1）当sub1的父对象(或曾祖父，只要是父类对象)parent也设置了position属性值为absolute或者relative时，也就是说，不是默认值
的情况，此时sub1按照这个parent来进行定位。
>
> 注意，对象虽然确定好了，但是有些细节需要您的注意，那就是我们到底以parent的那个位置进行定位呢？如果parent设定了margin,border,padding等属性，这个定位将忽略padding，将会从padding开始的地方(即只从padding的左上角开始)进行定位，也就是忽略padding，当然
并不会忽略margin和border.
> 
> 接下来的问题是，sub2的位置到哪里去了呢？由于当position设置为absolute后，会导致sub1移除正常的文档流，就像它不属于parent一样，
> 
> 它漂浮了起来，在DreamWeaver中把它称为“层”，其实意思是一样的。此时sub2将获得sub1的位置，他的文档流不再基于sub1，而是直接从
parent开始。

> （2）如果sub1不存在一个有着position属性的父对象，那么那就会以body为定位对象，按照留恋其的窗口进行定位，这个比较容易理解。

##### 3. fixed (固执的)

> fixed是特殊的absolute，即fixed总是以body为定位对象的，按照浏览器的窗口进行定位，即使拖动欧诺个滚动条，他的位置也是不会改变的，与background-attachment:fixed相似,当然在Dreamweaver下似乎没有支持

##### 4. static

> position的默认值，一般不设置position属性时，会按照正常的文档流进行排列。