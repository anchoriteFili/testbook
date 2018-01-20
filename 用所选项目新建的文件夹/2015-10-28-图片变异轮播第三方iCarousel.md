---
layout: post
#标题
title:  图片变异轮播第三方iCarousel
#时间配置
date:   2015-10-28 18:57:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/3d图片滚动样式.zip" target="_blank">3d图片滚动样式.zip</a><br>

![752372-20151028185729591-1677504309.png]({{ site.img_url }}423C2F65C56EF9AD64190DABAABD9661.png)

**快速载入**

```objc
#pragma mark 设置添加的总的图片数量
- (NSUInteger)numberOfItemsInCarousel:(iCarousel *)carousel
{
    return 30;
}

#pragma mark 在此处添加图片
- (UIView *)carousel:(iCarousel *)carousel viewForItemAtIndex:(NSUInteger)index
{
    UIView *view = [[[UIImageView alloc] initWithImage:[UIImage imageNamed:[NSString stringWithFormat:@"%d.jpg",index]]] autorelease];
    
#pragma mark 调整图片的显示尺寸
    view.frame = CGRectMake(70, 80, 220, 100);
    return view;
}

#pragma mark 占位图数量
- (NSUInteger)numberOfPlaceholdersInCarousel:(iCarousel *)carousel
{
    return 0;
}

#pragma mark 当前屏内显示的图片数量
- (NSUInteger)numberOfVisibleItemsInCarousel:(iCarousel *)carousel
{
    return 6;
}

#pragma mark 设置中间图片的宽度
- (CGFloat)carouselItemWidth:(iCarousel *)carousel
{
//    return ITEM_SPACING;
    return 250;
    
}
```