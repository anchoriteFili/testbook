---
layout: post
#tableView底部footerView之间空白处理方式
title:  tableView底部footerView之间空白处理方式
#时间配置
date:   2016-09-23 17:51:50 +0800
#大类配置
categories: 环境配置
#小类配置
tag: mac
---

* content
{:toc}

```objc
 // UITableViewStyleGrouped状态下，后边默认有空白，Plain没空白，向去掉空白用CGFLOAT_MIN。。用0无效。。。好坑啊
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section {
    return CGFLOAT_MIN;
}
```


只有tableView是Grouped的时候才会底部有空白，Plain不会，解决方法如下

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
{
    if (section == 最后一个section) {
        return CGFLOAT_MIN; // 此处很变态，其实返回0.5也行，但如果返回0，则实际上底部还是会有一段空白
    } else {
        return 30; // 你可能需要的其他section的高度
    }
}


```