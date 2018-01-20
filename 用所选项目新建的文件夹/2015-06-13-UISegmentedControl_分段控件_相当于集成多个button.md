---
layout: post
#标题
title:  UISegmentedControl_分段控件_相当于集成多个button
#时间配置
date:   2015-06-13 20:43:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![132041534418753.png]({{ site.img_url }}EDDC4A2303E92AC53CF83603E0E1FD77.png)

![132036401446200.png]({{ site.img_url }}27D5FD4B797CD776EF5D0534F3E985BB.png)

```objc
#pragma mark 创建UISegmentedControl
    NSArray *items = [NSArray arrayWithObjects:@"首页",@"列表",@"收藏", nil];
    UISegmentedControl *segment = [[UISegmentedControl alloc] initWithItems:items];
    segment.frame = CGRectMake(0, 30, 320, 30);
    segment.tintColor = [UIColor cyanColor];
//    设置初始时选取的item的坐标
    segment.selectedSegmentIndex = 0;
//    在1坐标处插入我的空间item
    [segment insertSegmentWithTitle:@"我的空间" atIndex:1 animated:YES];
    [self.view addSubview:segment];
    [segment release];
```

**UISegmentedControl事件的触发**

```objc
self.brightSegment.selectedSegmentIndex = 0;
    [self.brightSegment addTarget:self action:@selector(hehe:) forControlEvents:UIControlEventValueChanged];

#pragma mark 触发事件
- (void)hehe:(UISegmentedControl *)sender {
    
    if (sender.selectedSegmentIndex == 0) {
        NSLog(@"hehehe");
    } else if (sender.selectedSegmentIndex == 1) {
        NSLog(@"hahahah");
    }
}
```