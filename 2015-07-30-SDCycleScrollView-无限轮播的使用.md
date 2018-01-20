---
layout: post
#标题
title:  SDCycleScrollView-无限轮播的使用
#时间配置
date:   2015-07-30 15:40:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/无限轮播器测试.zip" target="_blank">无限轮播器测试.zip</a><br>

<a href="https://github.com/gsdios/SDCycleScrollView" target="_blank">SDCycleScrollView_git地址</a><br>


##### 1.添加头文件#import "SDCycleScrollView.h"和代理<SDCycleScrollViewDelegate>

##### 2.复制粘贴下面代码即可

```objc
/**
     添加本地轮播
     */
    
//    1. 采用本地图片实现
//    1.1 创建图片数组
    NSArray *images = @[[UIImage imageNamed:@"h1.png"],
                        [UIImage imageNamed:@"h2.png"],
                        [UIImage imageNamed:@"h3.png"],
                        [UIImage imageNamed:@"h4.png"]
                        ];
//    1.2 创建图片配置的文字数组
    NSArray *titles = @[@"第一张",
                        @"第二张",
                        @"第三张",
                        @"第四张",
                        ];
//    2. 创建本地图片轮播器
//    2.1 创建轮播view
    SDCycleScrollView *cycleScrollView = [SDCycleScrollView cycleScrollViewWithFrame:CGRectMake(0, 100, self.view.bounds.size.width, 180) imagesGroup:images];
    
//    2.2 判断是否无限轮播
    cycleScrollView.infiniteLoop = YES;
//    2.3 设置轮播间隔的时间
    cycleScrollView.autoScrollTimeInterval = 1.0;
    cycleScrollView.titlesGroup = titles;
    
    cycleScrollView.delegate = self;
//    设置图片翻页动画
    cycleScrollView.pageControlStyle = SDCycleScrollViewPageContolStyleAnimated;
    [self.view addSubview:cycleScrollView];
    
    /**
     添加网络图片轮播
     */
//    1. 网络申请图片配置
//    1.1  创建图片网址数组
    NSArray *URLArray = @[
                 @"https://ss2.baidu.com/-vo3dSag_xI4khGko9WTAnF6hhy/super/whfpf%3D425%2C260%2C50/sign=a4b3d7085dee3d6d2293d48b252b5910/0e2442a7d933c89524cd5cd4d51373f0830200ea.jpg",
                 @"https://ss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/super/whfpf%3D425%2C260%2C50/sign=a41eb338dd33c895a62bcb3bb72e47c2/5fdf8db1cb134954a2192ccb524e9258d1094a1e.jpg",
                 @"http://c.hiphotos.baidu.com/image/w%3D400/sign=c2318ff84334970a4773112fa5c8d1c0/b7fd5266d0160924c1fae5ccd60735fae7cd340d.jpg"
                 ];
    
//    1.2 创建网络图片配置的文字
    NSArray *intTitles = @[@"第一张",
                           @"第二张",
                           @"第三张",
                           @"第四张",
                           ];
    
//    2. 网络图片轮播设置
//    2.1 创建网络图片轮播view  * 当imageURLStringsGroup中添加为nil时，可以在后面添加after延迟加载
    SDCycleScrollView *intSDCycleScrollView = [SDCycleScrollView cycleScrollViewWithFrame:CGRectMake(0, 300, self.view.bounds.size.width, 180) imageURLStringsGroup:nil];
    
//    2.2 判断是否无限轮播
    intSDCycleScrollView.infiniteLoop = YES;
//    2.3 设置轮播间隔时间
    intSDCycleScrollView.autoScrollTimeInterval = 0.3;
    
    intSDCycleScrollView.delegate = self;
//    2.4 添加图片配置文字
    intSDCycleScrollView.titlesGroup = intTitles;
    
//    2.5 设置点点样式
    intSDCycleScrollView.pageControlStyle = SDCycleScrollViewPageContolStyleClassic;
    
//    2.6设置pageController位置
    intSDCycleScrollView.pageControlAliment = SDCycleScrollViewPageContolAlimentRight;
    
//    2.7 自定义小圆点的颜色
    intSDCycleScrollView.dotColor = [UIColor yellowColor];
    
//    2.8 可以添加placeholderImage
    intSDCycleScrollView.placeholderImage = [UIImage imageNamed:@"h1.png"];
    
    [self.view addSubview:intSDCycleScrollView];
    
//    2.9 模拟加载延时
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        intSDCycleScrollView.imageURLStringsGroup = URLArray;
    });
```