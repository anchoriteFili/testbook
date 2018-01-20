---
layout: post
#标题
title:  button上左对齐方法_快速创建多个同类型的button
#时间配置
date:   2015-09-14 12:46:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft; 
```


**快速创建多个同规格的button**

```objc
#pragma mark 下面的多个button的创建
- (void)buttonWithBtnName:(NSString *)btnName frameWithX:(CGFloat)x Y:(CGFloat)y Width:(CGFloat)width andHeight:(CGFloat)height {
    
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    button.frame = CGRectMake(WIDTHADP*x, HEIGHTADP*y, width, height);
    [button setTitle:btnName forState:UIControlStateNormal];
    [button setTitleColor:RGB_COLOR(255, 255, 255) forState:UIControlStateNormal];
    [button setTitleColor:RGB_COLOR(112, 182, 35) forState:UIControlStateHighlighted];
    button.titleLabel.font = [UIFont systemFontOfSize:13.0];
    button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft; //左对齐
    [button addTarget:self action:@selector(buttonsClick:) forControlEvents:UIControlEventTouchUpInside];
    [self.tableHeaderView addSubview:button];
}

#pragma mark 所有button点击事件
- (void)buttonsClick:(UIButton *)sender {
    
    if ([sender.titleLabel.text isEqualToString:@"众筹介绍"]) {
        NSLog(@"众筹介绍");
    } else if ([sender.titleLabel.text isEqualToString:@"众筹参与者"]) {
        NSLog(@"众筹参与者");
    } else if ([sender.titleLabel.text isEqualToString:@"众筹项目"]) {
        NSLog(@"众筹项目");
    } else if ([sender.titleLabel.text isEqualToString:@"项目众筹条件"]) {
        NSLog(@"项目众筹条件");
    } else if ([sender.titleLabel.text isEqualToString:@"领投人"]) {
        NSLog(@"领投人");
    } else if ([sender.titleLabel.text isEqualToString:@"流程"]) {
        NSLog(@"流程");
    } else if ([sender.titleLabel.text isEqualToString:@"投资金额支付及服务费"]) {
        NSLog(@"投资金额支付及服务费");
    } else if ([sender.titleLabel.text isEqualToString:@"出品绿灯会"]) {
        NSLog(@"出品绿灯会");
    }
}
```