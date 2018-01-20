---
layout: post
#标题
title:  webview加载含有中文的url显示空白
#时间配置
date:   2017-06-20 15:39:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


```objc
// 在类目中添加这个方法
- (NSString *)URLEncodeString {
    NSCharacterSet *set = [NSCharacterSet URLQueryAllowedCharacterSet];
    NSString *encodedString = [self stringByAddingPercentEncodingWithAllowedCharacters:set];
    return encodedString;
}
```