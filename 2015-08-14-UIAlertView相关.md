---
layout: post
#标题
title:  UIAlertView相关
#时间配置
date:   2015-08-14 13:44:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://blog.csdn.net/enuola/article/details/7900346" target="_blank">UIALertView的基本用法与UIAlertViewDelegate对对话框的事件处理方法</a><br>

```objc
//创建UIAlertView <UIAlertViewDelegate> //添加代理

//初始化AlertView
    UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"AlertViewTest"
                                                   message:@"message"
                                                  delegate:self
                                         cancelButtonTitle:@"Cancel"
                                         otherButtonTitles:@"OtherBtn",nil];
    //设置标题与信息，通常在使用frame初始化AlertView时使用
    alert.title = @"AlertViewTitle";
    alert.message = @"AlertViewMessage";
    
    //这个属性继承自UIView，当一个视图中有多个AlertView时，可以用这个属性来区分
    alert.tag = 0;
    //只读属性，看AlertView是否可见
    NSLog(@"%d",alert.visible);
    //通过给定标题添加按钮
    [alert addButtonWithTitle:@"addButton"];
    //按钮总数
    NSLog(@"number Of Buttons :%d",alert.numberOfButtons);
    //获取指定索引的按钮标题
    NSLog(@"buttonTitleAtIndex1:%@",[alert buttonTitleAtIndex:1]);
    NSLog(@"buttonTitleAtIndex2:%@",[alert buttonTitleAtIndex:2]);
    //获取取消按钮的索引
    NSLog(@"cancelButtonIndex:%d",alert.cancelButtonIndex);
    //获取第一个其他按钮的索引
    NSLog(@"firstOtherButtonIndex:%d",alert.firstOtherButtonIndex);
    //显示AlertView
    [alert show];
    [alert release];


#pragma marks -- UIAlertViewDelegate代理方法 --
//根据被点击按钮的索引处理点击事件
-(void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex
{
    NSLog(@"clickButtonAtIndex:%d",buttonIndex);
}

//AlertView已经消失时执行的事件
-(void)alertView:(UIAlertView *)alertView didDismissWithButtonIndex:(NSInteger)buttonIndex
{
    NSLog(@"didDismissWithButtonIndex");
}

//ALertView即将消失时的事件
-(void)alertView:(UIAlertView *)alertView willDismissWithButtonIndex:(NSInteger)buttonIndex
{
    NSLog(@"willDismissWithButtonIndex");
}

//AlertView的取消按钮的事件
-(void)alertViewCancel:(UIAlertView *)alertView
{
    NSLog(@"alertViewCancel");
}

//AlertView已经显示时的事件
-(void)didPresentAlertView:(UIAlertView *)alertView
{
    NSLog(@"didPresentAlertView");
}

//AlertView即将显示时
-(void)willPresentAlertView:(UIAlertView *)alertView
{
    NSLog(@"willPresentAlertView");
}
```