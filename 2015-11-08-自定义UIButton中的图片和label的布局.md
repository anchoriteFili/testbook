---
layout: post
#标题
title:  自定义UIButton中的图片和label的布局
#时间配置
date:   2015-11-08 18:36:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

**使用方法**

```objc
IBButton *button = [[IBButton alloc] initWithFrame:CGRectMake(20, 20, self.view.frame.size.width-40, 50)];
    button.backgroundColor = [UIColor yellowColor];
    
    [button setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
    button.titleLabel.font = [UIFont systemFontOfSize:30];
    button.imageViewFrame = CGRectMake(0, 0, 50, button.frame.size.height);
    button.titleLabelFrame = CGRectMake(50, 0, button.frame.size.width-50, button.frame.size.height);
    [self.view addSubview:button];
    button.frame = CGRectMake(20, 30, self.view.frame.size.width-40, 50);

    /**
     注意：这里不是用的setBackgroundimage
     */
    [button setImage:[UIImage imageNamed:@"美女图片"] forState:UIControlStateNormal];
    
    [button setTitle:@"哈哈哈哈哈哈" forState:UIControlStateNormal];
```

**IBButton内部**

```objc
#import "IBButton.h"

@implementation IBButton

- (id)initWithFrame:(CGRect)frame
{
    const CGFloat scale = [UIScreen mainScreen].bounds.size.width / 640.0;
    
    if (self = [super initWithFrame:frame]) {
        // 设置按钮文字颜色
        [self setTitleColor:[UIColor colorWithRed:124/255.0 green:136/255.0 blue:155/255.0 alpha:1.0] forState:UIControlStateNormal];
        // 设置按钮文字居中
//        self.titleLabel.textAlignment = NSTextAlignmentCenter;
        // 让图片按照原来的宽高比显示出来
        self.imageView.contentMode = UIViewContentModeScaleAspectFit;
        // 设置按钮文字的字体
        self.titleLabel.font = [UIFont systemFontOfSize:24 * scale];
        
        self.adjustsImageWhenHighlighted = NO;
    }
    return self;
}

#pragma mark - 重写了UIButton的方法
#pragma mark 控制UILabel的位置和尺寸
// contentRect其实就是按钮的位置和尺寸
- (CGRect)titleRectForContentRect:(CGRect)contentRect
{
    return self.titleLabelFrame;
}

#pragma mark 控制UIImageView的位置和尺寸
- (CGRect)imageRectForContentRect:(CGRect)contentRect
{
    return self.imageViewFrame;
}

@end
```