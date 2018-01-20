---
layout: post
#标题
title:  tableView中消除cell左15像素空白方法
#时间配置
date:   2015-09-28 13:59:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
//在创建tableView时用这个方法
    if ([self.itemTableView respondsToSelector:@selector(setSeparatorInset:)]) {
        [self.itemTableView setSeparatorInset:UIEdgeInsetsZero];
    }
    if ([self.itemTableView respondsToSelector:@selector(setLayoutMargins:)]) {
        [self.itemTableView setLayoutMargins:UIEdgeInsetsZero];
    }

#pragma mark 设置cell的样式 代理方法设置
- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath {
    
    if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {
        [cell setSeparatorInset:UIEdgeInsetsZero];
    }
    if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        [cell setLayoutMargins:UIEdgeInsetsZero];
    }
}
```