---
layout: post
#标题
title:  SVPullToRefresh上拉加载下拉刷新
#时间配置
date:   2015-10-27 10:41:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="https://github.com/samvermette/SVPullToRefresh" target="_blank">https://github.com/samvermette/SVPullToRefresh</a><br>

```objc
#import "SVPullToRefresh.h"
#import "AFNetworking.h"

@property (assign, nonatomic) int beginIdx; //和刷新有关的东西
@property (assign, nonatomic) int blogCount; //和刷新有关的东西

#pragma mark 这个是刷新的部分 #import "SVPullToRefresh.h"
    __weak typeof (self) weakself = self;
    
    [self.itemsTableView addPullToRefreshWithActionHandler:^{
        [weakself loadMyTableViewDate:weakself.beginIdx count:weakself.blogCount isInsert:NO];
    }];
    
    [self.itemsTableView addInfiniteScrollingWithActionHandler:^{
        [weakself loadMyTableViewDate:weakself.beginIdx count:weakself.blogCount isInsert:YES];
    }];

#pragma mark 刷新方法的实现
- (void)loadMyTableViewDate:(int)beginIdx count:(int)count isInsert:(bool)isInsert
{
    /**
     在里面申请数据，在申请数据返回值里调用停止动画方法
     */
    
    AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
    NSDictionary *params = @{
                             
                             };
    
    [manager POST:api_StudioOfficialDynamicBlog parameters:params success:^(AFHTTPRequestOperation *operation, id responseObject) {
        int errid = [responseObject[@"ErrId"] intValue];
        if (errid != 1) {
            return;
        }
        
        //数据处理
        int dataCount = [responseObject[@"DataCount"] intValue];
        
        for (int i = 0; i < dataCount; i++) {
            
//            TeamHomePageBlogModel *blogModel = [[TeamHomePageBlogModel alloc] init];
//            
//            [blogModel procMyblogInfo:responseObject index:i];
//            [_blogArray addObject:blogModel];
            
        }
        
        _beginIdx+= dataCount;
        [self.itemsTableView reloadData];
        if (!isInsert) {
            [self.itemsTableView.pullToRefreshView stopAnimating];
        }
        else
            [self.itemsTableView.infiniteScrollingView stopAnimating];
        
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        NSLog(@"error %@", error);
        if (!isInsert) {
            [self.itemsTableView.pullToRefreshView stopAnimating];
        }
        else
            [self.itemsTableView.infiniteScrollingView stopAnimating];
        
        [self hideHud];
        TTAlertForNetError(error.code);
    }];
}
```