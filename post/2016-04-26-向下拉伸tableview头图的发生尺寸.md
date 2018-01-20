---
layout: post
#标题
title:  向下拉伸tableview头图的发生尺寸
#时间配置
date:   2016-04-26 18:28:22 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![752372-20160426182243330-667233869.png]({{ site.img_url }}8D0700240B5297CF2787A806A8310843.png)

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/tableViewheader下拉放大.zip" target="_blank">tableViewheader下拉放大.zip</a><br>

> 说明：当tableView下拉的时候，上面的图根据偏移量改变尺寸，当偏移量为负值的时候(下拉)，可以改变尺寸，当偏移量为正值的时候(上拉),不改变尺寸。


```objc
#import "ViewController.h"

@interface ViewController ()<UITableViewDelegate,UITableViewDataSource>

@property (weak, nonatomic) IBOutlet UITableView *tableView;
@property (nonatomic,retain) UIView *headerView; //头部view

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    [self.view addSubview:self.headerView];
    self.tableView.delegate = self;
    self.tableView.dataSource = self;
    
}

#pragma mark 设置每行的高度
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return 60;
}

#pragma mark 设置section的个数
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

#pragma mark 设置每个section的行数
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return 20;
}

#pragma mark cellForRow方法
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    static NSString *cellIdentifier = @"cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    if (cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
    }
    //    设置选择时的状态
    cell.selectionStyle = UITableViewCellSelectionStyleDefault;
    //    设置cell背景色为透明色
    cell.backgroundColor = [UIColor clearColor];
    
    return cell;
}

#pragma mark 添加点击cell的方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
    //点击以后直接恢复正常状态
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    
    if (scrollView.contentOffset.y<0) {
        self.headerView.frame = CGRectMake(scrollView.contentOffset.y/200*WIDTH/2, 0, (200-scrollView.contentOffset.y)/200*WIDTH, 200-scrollView.contentOffset.y);
    }
    NSLog(@"正在滑动");
}

- (UIView *)headerView {
    if (!_headerView) {
        _headerView = [[UIView alloc] init];
        
        _headerView.frame = CGRectMake(0, 0, WIDTH, 200);
        
        UIImageView *imageView = [[UIImageView alloc] init];
        imageView.image = [UIImage imageNamed:@"图片"];
        [_headerView addSubview:imageView];
        imageView.translatesAutoresizingMaskIntoConstraints = NO;
        
        NSArray *hConstranits = [NSLayoutConstraint constraintsWithVisualFormat:@"H:|-0-[imageView]-0-|" options:0 metrics:nil views:@{@"imageView":imageView}];
        NSArray *vConstranits = [NSLayoutConstraint constraintsWithVisualFormat:@"V:|-0-[imageView]-0-|" options:0 metrics:nil views:@{@"imageView":imageView}];
        
        [_headerView addConstraints:hConstranits];
        [_headerView addConstraints:vConstranits];
        
    }
    return _headerView;
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end

```