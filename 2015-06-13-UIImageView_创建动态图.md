---
layout: post
#标题
title:  UIImageView_创建动态图
#时间配置
date:   2015-06-13 21:03:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![132102338637431.png]({{ site.img_url }}BBB6FE308C833B063F3515245255DF28.png)

```objc
#pragma mark imageView创建动态图
//    创建由图片组成的数组
    NSMutableArray *array = [NSMutableArray array];
    for (int i = 1; i < 9; i ++) {
        UIImage *image = [UIImage imageNamed:[NSString stringWithFormat:@"pig－%d（被拖移）.tiff",i]];
        [array addObject:image];
        if (!image) {
            continue;
        }
    }
//    设置重复次数
    imageView.animationRepeatCount = 100000;
//    设置间隔的时间
    imageView.animationDuration = 0;
//    设置一组动态图
    imageView.animationImages = array;
//    开始动画
    [imageView startAnimating];
//    结束动画
//    [imageView stopAnimating];
    
    [self.view addSubview:imageView];
    [imageView release];
```