---
layout: post
#标题
title:  UIScrollVIew控件实现label内小说无缝轮播_小说下载器制作
#时间配置
date:   2015-06-02 09:13:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


```objc
#import "Scroll_label_ViewController.h"

#define SCROLL_W 320
#define SCROLL_H 568

@interface Scroll_label_ViewController ()<UIScrollViewDelegate>

@property (nonatomic,retain) UIScrollView *scrollView;
@property (nonatomic,retain) UIPageControl *pageControl;
@property (nonatomic,assign) int pageCount;
@property (nonatomic,retain) UILabel *label1;
@property (nonatomic,retain) UILabel *label2;
@property (nonatomic,retain) UILabel *lable3;

@end

@implementation Scroll_label_ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    
    self.scrollView = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 0, SCROLL_W, SCROLL_H)];
    self.scrollView.pagingEnabled = YES;
    self.scrollView.bounces = NO;
    self.scrollView.showsHorizontalScrollIndicator = NO;
    self.scrollView.showsVerticalScrollIndicator = NO;
    self.scrollView.delegate = self;
    self.scrollView.contentSize = CGSizeMake(SCROLL_W, SCROLL_H*3);
    
    UILabel *label1 = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, SCROLL_W, SCROLL_H)];
    self.label1 = label1;
//    label1.backgroundColor = [UIColor redColor];
    label1.text = @"第1页";
    [self.scrollView addSubview:label1];
    
    UILabel *label2 = [[UILabel alloc] initWithFrame:CGRectMake(0, SCROLL_H, SCROLL_W, SCROLL_H)];
    self.label2 = label2;
    label2.text = @"第2页";
//    label2.backgroundColor = [UIColor blueColor];
    [self.scrollView addSubview:label2];
    
    UILabel *label3 = [[UILabel alloc] initWithFrame:CGRectMake(0, SCROLL_H*2, SCROLL_W, SCROLL_H)];
    self.lable3 = label3;
    label3.text = @"第3页";
//    label3.backgroundColor = [UIColor yellowColor];
    [self.scrollView addSubview:label3];
    
    NSArray *labelArr = @[label1,label2,label3];
    for (int i = 0; i < 3; i ++) {
        [self.scrollView addSubview:[labelArr objectAtIndex:i]];
    }
    
    [self.view addSubview:self.scrollView];
    
}


- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView {
    int page = (int)(self.scrollView.contentOffset.y+SCROLL_H/2)/SCROLL_H;
    
    if (page == 2) {
        [self.scrollView setContentOffset:CGPointMake(0, SCROLL_H) animated:NO];
        self.pageCount ++;
        
        self.label1.text = [NSString stringWithFormat:@"第%d页",self.pageCount+1];
        self.label2.text = [NSString stringWithFormat:@"第%d页",self.pageCount+2];
        self.lable3.text = [NSString stringWithFormat:@"第%d页",self.pageCount+3];
    }
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end
```