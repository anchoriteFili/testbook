---
layout: post
#标题
title: NSDate/NSDateInterval/NSDateFormatter使用
#时间配置
date:   2016-02-29 18:08:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
#pragma mark NSDate的使用
    //获取的是0时区的时间
    NSDate *nowDate = [NSDate date];
    NSLog(@"nowDate ====== %@",nowDate);
    
#pragma mark NSTimeInterval 用以表示以秒为单位的时间间隔
    /**
     可以使用- initWithTiimeIntervalSinceNow:方法传入一个NSTimeInterval参数，创建一个NSDate对象
     */

    NSDate *tomorrowDate = [[NSDate alloc] initWithTimeIntervalSinceNow:24*60*60];
    
    NSDate *yesterdayDate = [[NSDate alloc] initWithTimeIntervalSinceNow:-24*60*60];

    NSLog(@"tomorrowDate = %@",tomorrowDate);
    NSLog(@"yesterdayDate = %@",yesterdayDate);
    
    NSLog(@"timeInterval = %f",[tomorrowDate timeIntervalSinceDate:yesterdayDate]);
    
#pragma mark NSDateFormatter 
    /**
     NSDateFormatter是ios中的日期格式类，功能是实现NSDate和NSString的互转
     
     */
    
    //指定日期格式
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
//    [formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
    [formatter setDateFormat:@"yyyy年MM月dd日 HH点mm分ss秒"]; //转换下格式就变成东八区时间了

    NSString *dateString = [formatter stringFromDate:[NSDate date]];
    
    NSLog(@"dateString = %@",dateString);
```