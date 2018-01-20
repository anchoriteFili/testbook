---
layout: post
#标题
title:  NSTimer与ScrollView的配合使用
#时间配置
date:   2015-06-16 11:09:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


##### 创建NSTimer
---

```objc
    // 添加计时器，每1秒钟执行一次nextImage方法，是图片自动翻页
    self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(nextImage) userInfo:nil repeats:YES];
    // 将计时器添加到循环中，计时器跑起
    [[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];
    
    // 移除方法
    [self.timer invalidate];
```

##### NSTimer与ScrollView的配合使用

![161417221076424.gif]({{ site.img_url }}333242372A11ADF2CCC4A31F456B7930.gif)

**代码部分**

```objc
#import "Scroller_lunbo_ViewController.h"

@interface Scroller_lunbo_ViewController ()<UIScrollViewDelegate>

@property (retain,nonatomic) UIScrollView *scrollView;
@property (nonatomic,retain) UIPageControl *pageControl;
@property (nonatomic,retain) NSTimer *timer;

@end

@implementation Scroller_lunbo_ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
#pragma mark 创建UIScrollView
    UIScrollView *scrollView = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 0, 320, 568)];
    scrollView.delegate = self;
    self.scrollView = scrollView;
    [self.view addSubview:scrollView];
//    锁定滑动方向
    scrollView.directionalLockEnabled = NO;
//    图片的宽
    CGFloat imageW = self.scrollView.frame.size.width;
//    图片的高
    CGFloat imageH = self.scrollView.frame.size.height;
//    图片中的Y
    CGFloat imageY = 0;
//    图片个数
    NSInteger totalCount = 5;
//    添加5张图片
    
#pragma mark 向scrollView中添加5张图片
    for (int i = 0; i < totalCount; i ++) {
        UIImageView *imageView = [[UIImageView alloc] init];
        CGFloat imageX = i*imageW;
        imageView.frame = CGRectMake(imageX, imageY, imageW, imageH);
        
        NSString *name = [NSString stringWithFormat:@"pig－%d（被拖移）.tiff",i+1];
        imageView.image = [UIImage imageNamed:name];
//        隐藏指示条
        self.scrollView.showsHorizontalScrollIndicator = NO;
        [self.scrollView addSubview:imageView];
        
    }

//    2.设置scrollView的滚动范围
    CGFloat contentW = totalCount * imageW;
    
//    不允许在垂直方向上进行滚动
    self.scrollView.contentSize = CGSizeMake(contentW, 0);
//    3.设置分页
    self.scrollView.pagingEnabled = YES;
//    4.监听scrollView的滚动
    self.scrollView.delegate = self;
    
    [self addTimer];
    
#pragma mark 创建UIPageControl
    UIPageControl *pageControl = [[UIPageControl alloc] initWithFrame:CGRectMake(50, 400, 200, 50)];
    self.pageControl = pageControl;
    
    //    背景色
    pageControl.backgroundColor = [UIColor cyanColor];
    pageControl.numberOfPages = 5;
    pageControl.currentPage = 0;
    
    //    设置普通点的颜色为蓝色
    pageControl.pageIndicatorTintColor = [UIColor blueColor];
    
    //    设置正在显示的页的点的颜色为红色
    pageControl.currentPageIndicatorTintColor = [UIColor  redColor];
    
    [self.view addSubview:pageControl];
    [pageControl release];
    
}

#pragma mark scrollView滚动时调用,在滚动的时候对currentPage赋值，保持点图一一对应
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {

    //    NSLog(@"滚动中");
    //    计算页码
    //    页码 = (contentoffset.x + scrollView一半宽度)/scrollView宽度
    CGFloat scrollViewW = scrollView.frame.size.width;
    CGFloat x = scrollView.contentOffset.x;
    int page = (x + scrollViewW / 2) / scrollViewW;
    
    //    在滚动的时候进行赋值
    self.pageControl.currentPage = page;
    
}

#pragma mark 开始拖拽时调用,拖拽时移除计时器，使拖拽和计时器互不影响
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView {
//  移除计时器
    [self removeTimer];
}

#pragma mark 当拖拽停止时，添加计时器，是图片自动移动照常进行
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate {
    [self addTimer];
}

#pragma mark 创建添加计时器方法
- (void)addTimer {
//    添加计时器，每1秒钟执行一次nextImage方法，是图片自动翻页
    self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(nextImage) userInfo:nil repeats:YES];
//    将计时器添加到循环中，计时器跑起
    [[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];
}

#pragma mark 创建计时器的nextImage方法，实现图片的翻页效果
- (void)nextImage {
//    将起到传值功能的currentPage的值赋给page，得到图片当前的页面
    int page = (int)self.pageControl.currentPage;
    if (page == 4) {
        page = 0;
//        当page为0时，直接将偏移量转成0，是图片回到起始位置
        [self.scrollView setContentOffset:CGPointMake(0, 0) animated:NO];
    }else {
        page++;
//        当page!=4时，页数正常加1，并对偏移量进行赋值，实现图片翻页
        CGFloat x = page * self.scrollView.frame.size.width;
        [self.scrollView setContentOffset:CGPointMake(x, 0) animated:YES];
    }
}

#pragma mark 创建移除计时器的方法
- (void)removeTimer {
//    计时器移除
    [self.timer invalidate];
}
```