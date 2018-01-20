---
layout: post
#标题
title: UIImageView边角不拉伸方法
#时间配置
date:   2015-12-11 15:39:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://www.cnblogs.com/bandy/archive/2012/04/25/2469369.html" target="_blank">stretchableImageWithLeftCapWidth</a><br>
<a href="http://www.jianshu.com/p/c9cbbdaa9b02" target="_blank">iOS中拉伸图片的几种方式</a><br>
<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/图片的拉伸设置测试.zip" target="_blank">图片的拉伸设置测试.zip</a><br>


```objc
UIImage *image = [UIImage imageNamed:@"聊天后背景图"];
    
    // 设置端盖的值
    CGFloat top = 10;
    CGFloat left = 30;
    CGFloat bottom = 10;
    CGFloat right = 10;
    
    // 设置端盖的值
    UIEdgeInsets edgeInsets = UIEdgeInsetsMake(top, left, bottom, right);
    // 设置拉伸的模式
    UIImageResizingMode mode = UIImageResizingModeStretch;
    // 拉伸图片
    UIImage *newImage = [image resizableImageWithCapInsets:edgeInsets resizingMode:mode];
    self.testImageView.image  = newImage;
        self.imageViewOne.image = newImage;
```