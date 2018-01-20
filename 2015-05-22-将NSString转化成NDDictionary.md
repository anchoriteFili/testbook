---
layout: post
#标题
title:  将NSString转化成NDDictionary
#时间配置
date:   2015-05-22 17:01:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
//     将所有的元素加入到字典中
    NSDictionary *dic1 = @{@"book": book,@"author": author,@"category": category,@"picurl": picurl,@"result": self.result};
    
//     直接生成NSString形式的json形式
    NSString *sandStr = [dic1 JSONString];
//     NSLog(@"%@",sandStr);
//     寻找文件存放的路径
    NSString *namePath = [[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject] stringByAppendingString:[NSString stringWithFormat:@"%@.xml",book]];
    NSLog(@"%@",namePath);
//     将文件放入到沙盒中
    [sandStr writeToFile:namePath atomically:YES encoding:NSUTF8StringEncoding error:nil];
//     将文件从沙盒中取出
    NSString *strOut = [NSString stringWithContentsOfFile:namePath encoding:NSUTF8StringEncoding error:nil];
//     NSLog(@"%@",strOut);
//     将NSString格式的json直接转化成字典形式
    NSDictionary *dicOut = [strOut objectFromJSONStringWithParseOptions:JKParseOptionLooseUnicode];
//     经测试，可以使用
    NSString *nameOut = [dicOut objectForKey:@"book"];
    NSLog(@"%@",nameOut);
```