---
layout: post
#标题
title:  NSUserDefaults快速创建
#时间配置
date:   2015-05-08 10:25:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

**创建User类**

```objc
#import <Foundation/Foundation.h>

@interface User : NSObject<NSCoding>

@property (nonatomic,copy) NSString *userName;
@property (nonatomic,copy) NSString *pwd;

@end

#import "User.h"

@implementation User

//将传过来的user数据归档到aCoder中
- (void)encodeWithCoder:(NSCoder *)aCoder {
    
    [aCoder encodeObject:self.userName forKey:@"name"];
    [aCoder encodeObject:self.pwd forKey:@"pwd"];
    
}

//将存有user数据的aDecoder根据Key反归档
- (id)initWithCoder:(NSCoder *)aDecoder {
    
    self = [super init];
    if (self) {
        self.userName = [aDecoder decodeObjectForKey:@"name"];
        self.pwd = [aDecoder decodeObjectForKey:@"pwd"];
    }
    
    return self;
}

- (void)dealloc {
    [_userName release];
    [_pwd release];
    [super dealloc];
}

@end
```

```objc
#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidAppear:(BOOL)animated {
    NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];
    NSString *str = [userDefault objectForKey:@"str"];
    NSLog(@"%@",str);
    
    NSData *userData = [userDefault objectForKey:@"user"];
    User *user = [NSKeyedUnarchiver unarchiveObjectWithData:userData];
    NSLog(@"%@ %@",user.userName,user.pwd);
    
}

- (void)viewDidLoad
{
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];
    NSString *path = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
    NSLog(@"%@",path);
    
//    NSUserDefaults 储存用户偏好设置的类
    
    User *user = [[User alloc] init];
    user.userName = @"zhangsan";
    user.pwd = @"123423";
    
    [NSKeyedArchiver archiveRootObject:user toFile:@"path"];
    NSData *userData = [NSKeyedArchiver archivedDataWithRootObject:user];
    [userDefault setObject:userData forKey:@"user"];
    [userDefault synchronize];
    
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```