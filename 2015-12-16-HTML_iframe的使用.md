---
layout: post
#标题
title: HTML_iframe的使用
#时间配置
date:   2015-12-16 17:44:12 +0800
#大类配置
categories: 知识
#小类配置
tag: html
---

* content
{:toc}


> frame是一个内联的标签，可以进行各种承载关系

##### 1. 创建4个HTML文件framea、frameb、framec、和frame

![752372-20151216172424287-1665104160.png]({{ site.img_url }}B9D5DC36E48EDD0AA68FE3235A0919D1.png)

##### 2. 编写代码让c承载b、b承载a、frame承载c，形成一个层级关系

```html
<body bgcolor="#dc143c">

    FrameC
    <iframe src="frameb.html" width="600px" height="600px"></iframe>
</body>

<body bgcolor="#a52a2a">

    FrameB
    <iframe src="framea.html" width="400px" height="400px"></iframe>
</body>

    FrameA
    <a href="https://www.baidu.com" target="_parent">百度一下</a>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<!--内联-->
<a href="http://www.cnblogs.com/AnchoriteFiliGod/diary/2015/05/26/4531000.html" target="_blank">微博</a>
<br/>
<iframe src="framec.html" frameborder="0" width="800px" height="800px"></iframe>

<body>
</body>
</html>
```
![752372-20151216173202506-1577749789.png]({{ site.img_url }}815392FC11F7B4EA66950BF0FE256523.png)

**target参数的作用:**
> 当为_self的时候，在自己的框中显示

![752372-20151216173900052-867433328.png]({{ site.img_url }}4F66831AAD34330811DB1929838AAFA7.png)

![752372-20151216174041599-1661436611.png]({{ site.img_url }}27BF632EEF841FB80A49D502A54B030C.png)

**当为_blank时，直接跳转到新页面**

**当为_parent时，跳转到上级页面中，在a中点击在b中显示**

![752372-20151216174520131-461179451.png]({{ site.img_url }}4EE9DA38CA3B36ADA22B0C0FB070E4E8.png)

> 当为_top的时候，则直接在最高级父类中进行显示，即直接最高级父类显示，即frame中显示（全屏显示）