---
layout: post
#标题
title:  拖文件夹到Xcode中group和folder的区别
#时间配置
date:   2015-08-16 13:32:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://m.blog.csdn.net/blog/u010479715/43971499" target="_blank">[转]XCode添加文件夹形式</a><br>

![161324284264648.png]({{ site.img_url }}058C14EF8714D74CD5B62116818D70A2.png)


> 当使用folder时，文件夹里的代码都不会参加编译，其中Build Phases中的代码内容都不会改变

![161326333013398.png]({{ site.img_url }}5BE81271D54406DC8809798268C08B62.png)

> 当使用groups的时候

![161327473171887.png]({{ site.img_url }}318320D10C098ED39D145BEBFB15E50B.png)

> 里边的代码都参与编译，并且Build Phases中直接添加了文件夹中的代码

![161331403647756.png]({{ site.img_url }}B80F127335770C28CAF6AAA2641F762C.png)
