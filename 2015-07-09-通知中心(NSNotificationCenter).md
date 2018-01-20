---
layout: post
#标题
title:  通知中心(NSNotificationCenter)
#时间配置
date:   2015-07-09 20:06:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

**`通知模式`: 一个对象能够给其他任意数量的对象广播信息。对象之间可以没有耦合关系。**

> `NSNotification(通知)`，封装了要广播的消息。
> `NSNotificationCenter(通知中心)`，管理注册接受消息对象，广播消息。
> `observer(观察者)`，需要检测广播信息的对象，即接受信息的对象。

**使用方法**

> 接收信息对象在通知中心进行注册，包括：信息名称、接受信息时的处理方法。
> 对象通过通知中心广播信息，包括：信息名称、信息内容。
> 已经注册过的对象如果不需要接受信息时，在通知中心注销。

**注册通知**

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    
#pragma mark 接收信息对象在通知中心进行注册，包括：信息名称、接受信息时的处理方法。
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(ChangeNameNotification:) name:@"ChangeNameNotification" object:nil];
    
}

- (void)dealloc {
#pragma mark 在通知中心注销
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}

- (void)ChangeNameNotification:(NSNotification *)notification {
#pragma mark 获取传送来的文件
    NSDictionary *nameDictionary = [notification userInfo];
    self.nameLabel.text = [nameDictionary objectForKey:@"name"];
    
}
```

**发起通知**

```objc
@interface ViewController ()

@property (weak, nonatomic) IBOutlet UITextField *nameTextField;

@end

@implementation ViewController


- (IBAction)notificationMethod:(UIButton *)sender {
    
#pragma mark 对象通过通知中心广播消息，包括：信息名称、信息内容。
    [[NSNotificationCenter defaultCenter] postNotificationName:@"ChangeNameNotification" object:self userInfo:@{@"name":self.nameTextField.text}];
    
    [self.navigationController popViewControllerAnimated:YES];
    
}
```