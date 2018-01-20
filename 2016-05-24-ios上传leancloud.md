---
layout: post
#标题
title:  ios上传leancloud
#时间配置
date:   2016-05-24 13:36:22 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}


<a href="http://pan.baidu.com/s/1qXUbSIC" target="_blank">转换工具</a><br> **密码: `9hzw`**

##### 1. 创建txt文件

> 终端命令touch 转换文件.txt

##### 2. 引入leancloud

`pod 'AVOSCloud'`

##### 3. 相关代码

```objc
//如果使用美国站点，请加上这行代码 [AVOSCloud useAVCloudUS]; AppDelegate
    [AVOSCloud setApplicationId:@"MoNLf6HtzRCMxmpiffWYbAdI" clientKey:@"9giqNOqlKi4ttTSJSswdfdXc"];

#import "ViewController.h"

@interface ViewController ()

@property (nonatomic,strong) NSMutableArray *contentArr; //全局数组

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    
#pragma mark 获取文件
    NSString * namePath = [[NSBundle mainBundle] pathForResource:@"转码文件" ofType:@"txt"];
    NSString * arrayStr = [NSString stringWithContentsOfFile:[NSString stringWithFormat:@"%@",namePath] encoding:NSUTF8StringEncoding error:NULL];
    
    NSDictionary *dic = [self dictionaryWithJsonString:arrayStr];
    self.contentArr = dic[@"arr"];
    
}

#pragma mark 字符串转化成字典
- (NSDictionary *)dictionaryWithJsonString:(NSString *)jsonString {
    
    if (jsonString == nil) {
        return nil;
    }
    
    NSData *jsonData = [jsonString dataUsingEncoding:NSUTF8StringEncoding];
    NSError *err;
    NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:jsonData
                                                        options:NSJSONReadingMutableContainers
                                                          error:&err];
    if(err) {
        
        NSLog(@"json解析失败：%@",err);
        return nil;
    }
    return dic;
}

#pragma mark 上传按钮点击事件
- (IBAction)shangChuanClick:(UIButton *)sender {
    
    for (NSDictionary *dic in self.contentArr) {
        
        AVObject *todo = [AVObject objectWithClassName:@"xiaoShuo"];
        [todo setObject:dic[@"title"] forKey:@"title"];
        [todo setObject:dic[@"url"] forKey:@"url"];
        [todo setObject:dic[@"volume"] forKey:@"volume"];// 只要添加这一行代码，服务端就会自动添加这个字段
        [todo saveInBackgroundWithBlock:^(BOOL succeeded, NSError *error) {
            if (succeeded) {
                // 存储成功
                NSLog(@"保存成功");
                
            } else {
                // 失败的话，请检查网络环境以及 SDK 配置是否正确
                NSLog(@"保存失败");
            }
        }];
    }
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```