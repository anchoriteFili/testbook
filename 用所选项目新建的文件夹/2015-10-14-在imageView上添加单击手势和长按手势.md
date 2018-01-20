---
layout: post
#标题
title:  在imageView上添加单击手势和长按手势
#时间配置
date:   2015-10-14 16:57:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/imageView上手势的长按和点击手势添加.zip" target="_blank">imageView上手势的长按和点击手势添加.zip</a><br>


```objc
#import "ViewController.h"

@interface ViewController ()

@property (weak, nonatomic) IBOutlet UIImageView *imageView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
//    [self.imageView set]
    self.imageView.userInteractionEnabled = YES;
    
    
    //1、创建手势实例，并连接方法handleTapGesture,点击手势
    UITapGestureRecognizer *tapGesture=[[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(handleTapGesture:)];
    //设置手势点击数,双击：点2下
    tapGesture.numberOfTapsRequired=1;
    // imageView添加手势识别
    [self.imageView addGestureRecognizer:tapGesture];
    
    //6、长按手势
    UILongPressGestureRecognizer *longpressGesutre=[[UILongPressGestureRecognizer alloc]initWithTarget:self action:@selector(handleLongpressGesture:)];
    //长按时间为1秒
    longpressGesutre.minimumPressDuration=1;
    //所需触摸1次
    longpressGesutre.numberOfTouchesRequired=1;
    [self.imageView addGestureRecognizer:longpressGesutre];
    
}

#pragma mark 点击手势触发事件
- (void)handleTapGesture:(UITapGestureRecognizer *)sender {
    
    self.imageView.image = [UIImage imageNamed:@"图片1"];
    
}

#pragma mark 长按手势触发事件
- (void)handleLongpressGesture:(UITapGestureRecognizer *)sender {
    
    self.imageView.image = [UIImage imageNamed:@"图片2"];
    
}
```