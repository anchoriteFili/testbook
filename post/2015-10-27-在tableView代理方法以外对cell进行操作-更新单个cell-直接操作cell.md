---
layout: post
#标题
title:  在tableView代理方法以外对cell进行操作-更新单个cell-直接操作cell
#时间配置
date:   2015-10-27 16:08:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


```objc
#pragma mark 更新单个cell的方法
NSIndexPath *indexPath = [NSIndexPath indexPathForRow:row inSection:0];
[self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];

#pragma mark 直接获取对应位置的cell,对cell进行操作
    NSIndexPath *indexPath = [NSIndexPath indexPathForRow:12 inSection:0];
    ImageAttachmentCell *cell = (ImageAttachmentCell *)[self.tableView cellForRowAtIndexPath:indexPath];

```