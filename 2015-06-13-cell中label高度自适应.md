---
layout: post
#标题
title:  cell中label高度自适应
#时间配置
date:   2015-06-13 14:26:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

**tableView内每行高度的自适应**

```objc
#pragma mark cell高度的自适应
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
//    计算高度
    NSString *key = [[self.contactDic allKeys] objectAtIndex:indexPath.section];
    NSArray *arr = [self.contactDic objectForKey:key];
    Person *p = [arr objectAtIndex:indexPath.row];
    NSString *hobby = p.hobby;
    
    CGRect rect = [hobby boundingRectWithSize:CGSizeMake(200, 1000)
                                      options:NSStringDrawingUsesLineFragmentOrigin
                                   attributes:@{NSFontAttributeName: [UIFont systemFontOfSize:17]}
                                      context:nil];
    NSLog(@"rect = %@",NSStringFromCGRect(rect));
    
    return 110 + rect.size.height;
}
```

**cell内label的自适应**

```objc
CGRect frame = self.hobbyLabel.frame;
    CGRect rect = [p.hobby boundingRectWithSize:CGSizeMake(200, 1000)
                                        options:NSStringDrawingUsesLineFragmentOrigin
                                     attributes:@{NSFontAttributeName: [UIFont systemFontOfSize:17]}
                                        context:nil];
    frame.size.height = rect.size.height;
    self.hobbyLabel.frame = frame;
```