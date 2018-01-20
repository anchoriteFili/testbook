---
layout: post
#标题
title:  UIProgressView基础应用
#时间配置
date:   2015-06-18 17:05:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


```objc
#pragma mark 创建progressView
    UIProgressView *progressView = [[UIProgressView alloc] initWithProgressViewStyle:UIProgressViewStyleDefault];
    
    self.progressView = progressView;
//    设置progressView的frame
    progressView.frame = CGRectMake(30, 100, 300, 30);
//    初始化进度条数值
    progressView.progress = 0.0f;
//    设置跑过的颜色
    progressView.progressTintColor = [UIColor redColor];
//    设置没跑进度条的颜色
    progressView.trackTintColor = [UIColor greenColor];
//    设置进度条百分比
    [progressView setProgress:0.9 animated:YES];
//    设置进度条的中心点
    progressView.center = CGPointMake(100, 100);
    
//    动态改变进度条数值，别忘后边加.0
    for (int i = 0; i < 10000; i ++) {
        float m = (i/10000.0);
        [self.progressView setProgress:m animated:NO];
    }
    
    [self.view addSubview:progressView];
```