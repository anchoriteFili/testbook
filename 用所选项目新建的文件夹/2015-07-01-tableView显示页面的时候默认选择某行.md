---
layout: post
#标题
title:  tableView显示页面的时候默认选择某行
#时间配置
date:   2015-07-01 20:10:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


```objc
- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:YES];
    NSIndexPath *indexpath = [NSIndexPath indexPathForRow:9 inSection:0];
    
    [self.tableView scrollToRowAtIndexPath:indexpath atScrollPosition:UITableViewScrollPositionTop animated:YES]; 
    
    [self.tableView selectRowAtIndexPath:indexpath animated:YES scrollPosition:UITableViewScrollPositionMiddle]; 
}
```