---
layout: post
#标题
title:  警告控制器UIAlertController的创建
#时间配置
date:   2015-06-13 11:38:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
#pragma mark 创建一个警告controller
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"标题" message:@"标题下提示内容" preferredStyle:UIAlertControllerStyleAlert];
    
#pragma mark 创建取消按钮
    UIAlertAction *concelAction = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:nil];
    
#pragma mark 创建取消按钮
    UIAlertAction *defaultAction = [UIAlertAction actionWithTitle:@"默认" style:UIAlertActionStyleDefault handler:nil];
    
#pragma mark 创建重置按钮destructive破坏的
    UIAlertAction *destructiveAction = [UIAlertAction actionWithTitle:@"重置" style:UIAlertActionStyleDestructive handler:nil];
    
#pragma mark 将所有的事件添加到警告controller中
    [alertController addAction:concelAction];
    [alertController addAction:defaultAction];
    [alertController addAction:destructiveAction];
    
#pragma mark 获取所有事件
    NSArray *arr = [alertController actions];
    for (UIAlertAction *aler in arr) {
        NSLog(@"%@",aler);
    }
    
#pragma mark 添加文本输入框
    [alertController addTextFieldWithConfigurationHandler:^(UITextField *textField) {
        textField.placeholder = @"登录";
    }];
    
    [alertController addTextFieldWithConfigurationHandler:^(UITextField *textField) {
        textField.placeholder = @"密码";
    }];
    
#pragma mark 添加一个确认按钮 实现按钮的点击事件
    UIAlertAction *getAction = [UIAlertAction actionWithTitle:@"确认" style:UIAlertActionStyleDefault handler:^(UIAlertAction *action) {
        UITextField *login = alertController.textFields[0];
        UITextField *password = alertController.textFields[1];
        [self.view endEditing:YES];
        NSLog(@"登录：%@ 密码：%@",login.text,password.text);
    }];
    
    [alertController addAction:getAction];
#pragma mark 模态视图出现
    [self presentViewController:alertController animated:YES completion:nil];
```