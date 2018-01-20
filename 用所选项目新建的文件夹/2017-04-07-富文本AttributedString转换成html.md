---
layout: post
#富文本AttributedString转换成html
title:  富文本AttributedString转换成html
#时间配置
date:   2017-04-07 17:38:50 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
NSMutableAttributedString *AttrStr = [[NSMutableAttributedString alloc] initWithString:@"排名第 4 名"];
    //改变字体的颜色
    [AttrStr addAttribute:NSForegroundColorAttributeName value:[UIColor redColor] range:NSMakeRange(3, AttrStr.length-3-1)]; //后边留一个字符不变
    //改变某个范围的字体的大小
    [AttrStr addAttribute:NSFontAttributeName value:[UIFont systemFontOfSize:17] range:NSMakeRange(3, AttrStr.length-3-1)];
    
    NSLog(@"************ %@",[self htmlStringByHtmlAttributeString:AttrStr]);
```
**引用：**<br>
[textView富文本AttributedString，转换成html](https://www.jianshu.com/p/40b592e55711)<br>
[求助IOS大牛 解决个富文本编辑问题！急急急？](https://www.zhihu.com/question/41245554)<br>

```objc
- (NSString *)htmlStringByHtmlAttributeString:(NSAttributedString *)htmlAttributeString {
    
    NSString *htmlString;
    NSDictionary *exportParams = @{NSDocumentTypeDocumentAttribute:NSHTMLTextDocumentType,NSCharacterEncodingDocumentAttribute:[NSNumber numberWithInt:NSUTF8StringEncoding]};
    
    NSData *htmlData = [htmlAttributeString dataFromRange:NSMakeRange(0, htmlAttributeString.length) documentAttributes:exportParams error:nil];
    
    htmlString = [[NSString alloc] initWithData:htmlData encoding:NSUTF8StringEncoding];
    
    return htmlString;
}
```