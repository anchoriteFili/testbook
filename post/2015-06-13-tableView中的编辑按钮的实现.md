---
layout: post
#标题
title:  tableView中的编辑按钮的实现
#时间配置
date:   2015-06-13 14:51:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

**删除cell的实现**

```objc
#pragma mark 点击编辑按钮，进入编辑状态
- (void)leftItemClicked:(UIBarButtonItem *)item
{
    [self.tableView setEditing:YES animated:YES];
}

#pragma mark 设置行能否进行编辑
-(BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
{
    return YES;
    
}
#pragma mark 设置编辑状态，此处为删除状态
- (UITableViewCellEditingStyle)tableView:(UITableView *)tableView editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return UITableViewCellEditingStyleDelete;
}

#pragma mark -- commit the edit style
-(void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{
//    如果编辑方式为删除
    if (editingStyle == UITableViewCellEditingStyleDelete) {
        
        //删除数据和cell
     NSString * key = [[self.contactDic allKeys ] objectAtIndex:indexPath.section];
        NSMutableArray * arr = [self.contactDic objectForKey:key];
        
        [arr removeObjectAtIndex:indexPath.row];
        
        [self.tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationAutomatic];
    }else if (editingStyle == UITableViewCellEditingStyleInsert) {
        //        如果是插入编辑方式
        // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
    }
}

#pragma mark ------------------
#pragma mark 点击退出编辑状态
- (void)rightItemClicked:(UIBarButtonItem *)item
{
    [self.tableView setEditing:NO animated:YES];
}
```

**移动cell的实现**

```objc
#pragma mark 判断是否能够进入移动状态
-(BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath
{
    return YES;
}

#pragma mark 判断同一section能够进行移动，如果不是同一section则返回原位置
- (NSIndexPath *)tableView:(UITableView *)tableView targetIndexPathForMoveFromRowAtIndexPath:(NSIndexPath *)sourceIndexPath toProposedIndexPath:(NSIndexPath *)proposedDestinationIndexPath
{
    if (sourceIndexPath.section == proposedDestinationIndexPath.section) {
        return proposedDestinationIndexPath;
    }
    return sourceIndexPath;
}

#pragma mark 实现从起始indexPath移动到destinationIndexPath,移动的实现
- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)sourceIndexPath toIndexPath:(NSIndexPath *)destinationIndexPath
{
    NSString * key = [[self.contactDic allKeys] objectAtIndex:sourceIndexPath.section];
    Person * p =[[ self.contactDic objectForKey:key] objectAtIndex:sourceIndexPath.row];
    [p retain];
    [[ self.contactDic objectForKey:key] removeObject:p];
    
    NSString * newKey = [[self.contactDic allKeys] objectAtIndex:destinationIndexPath.section];
    NSMutableArray * newArray  = [self.contactDic objectForKey:newKey];
    [newArray insertObject:p atIndex:destinationIndexPath.row];
    [p release];
    
}
```