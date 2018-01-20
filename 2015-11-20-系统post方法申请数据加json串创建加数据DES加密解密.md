---
layout: post
#标题
title:  系统post方法申请数据加json串创建加数据DES加密解密
#时间配置
date:   2015-11-20 12:17:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/DES加密解密.zip" target="_blank">DES加密解密.zip</a><br>
<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/DESDemo.zip" target="_blank">DESDemo.zip</a><br>


**网络post申请**

```objc
- (void)voteButtonClick:sender {
    
    //1.创建可变字符串，初始化为数组开头括号
    NSMutableString *OptionResult = [[NSMutableString alloc] initWithString:@"["];
    
    //2.for循环拼接字符串
    for (int i = 0; i < self.modelArray.count; i ++) {
        
        GreenVoteTableViewCellModel *cellModel = [self.modelArray objectAtIndex:i];
        
        if (cellModel.CStarScore == 0) {
            TTAlert([NSString stringWithFormat:@"请选择第%d题星星",cellModel.Sequence]);
            return;
        }
        if (cellModel.CVoteOptionID == 1000) {
            TTAlert([NSString stringWithFormat:@"第%d题单选不能为空",cellModel.Sequence]);
            return;
        }
        
        //json串创建
        NSString *string;
        if ([cellModel.Comment isEqualToString:@""]) {
            
            string = [NSString stringWithFormat:@"{\"VoteThemeID\":%d,\"VoteOptionID\":%d,\"StarScore\":%d},",cellModel.VoteThemeID,cellModel.CVoteOptionID,cellModel.CStarScore];
            
        } else {
            string = [NSString stringWithFormat:@"{\"VoteThemeID\":%d,\"VoteOptionID\":%d,\"StarScore\":%d,\"Comment\":\"%@\"},",cellModel.VoteThemeID,cellModel.CVoteOptionID,cellModel.CStarScore,cellModel.Comment];
        }
        [OptionResult appendString:string];
    }
    
    // 3. 获取末尾逗号所在位置
    NSUInteger location = [OptionResult length]-1;
    NSRange range = NSMakeRange(location, 1);
    
    // 4. 将末尾逗号换成结束的]}
    [OptionResult replaceCharactersInRange:range withString:@"]"];
    /**
     出现问题：
     1. 在使用AF的post的时候，服务端接收到的数据是get的，并且接收的数据是残缺的，直接返回《在黑名单列表中》
     2. 在使用AF的get的时候，服务端能够正常的接收数据一切正常，但是get传输数据长度有限制，怕以后出问题
     3. 直接使用系统的post异步数据申请，服务端接收数据为post，能够正常传输数据，但是返回数据data为加密状态,好吧。。。总算弄好了
     */
    
#pragma mark 提交数据
    AppDelegate *delegate=(AppDelegate *)[[UIApplication sharedApplication] delegate];
    NSString *sessionId=delegate.sessionId;
    NSString *userId=[NSString stringWithFormat:@"%ld",(long)delegate.userId];

#pragma mark 系统方法post提交数据
    NSURL *url = [NSURL URLWithString:api_CrowdFundingVoteTitleSubmit];
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
    [request setHTTPMethod:@"POST"];
    
    NSString *paramStr = [NSString stringWithFormat:@"UserID=%@&SessionId=%@&CrowdFundingID=%@&OptionResult=%@",userId,sessionId,self.crowdFundingID,OptionResult];
    
    NSData *data = [paramStr dataUsingEncoding:NSUTF8StringEncoding];
    [request setHTTPBody:data];
    
    [NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse * _Nullable response, NSData * _Nullable data, NSError * _Nullable connectionError) {
        
        if (data != nil) {
            NSDictionary *responseObject = [EncryptUtil decryptWithData:data];
            int errid = [responseObject[@"ErrId"] intValue];
            if (errid != 1) {
                return;
            }
            
#pragma mark 页面的跳转
            [UIView animateWithDuration:0.5 animations:^{
                self.frame = CGRectMake(WIDTHADP*12, -601, WIDTHADP*351, HEIGHTADP*601);
            }];
            
            if (_delegate && [_delegate respondsToSelector:@selector(voteSucceed)]) {
                [_delegate voteSucceed];
            }
        }
    }];
}
```

**加密解密**


```objc
#import <Foundation/Foundation.h>
#import <CommonCrypto/CommonCryptor.h>
#import "GTMBase64.h"

@interface EncryptUtil : NSObject
/**
 使用默认密钥加密
 @param  sText 数据内容 */
+ (NSString *)encryptWithText:(NSString *)sText;
/**
 使用默认密钥解密
 @param  sText 数据内容 */
+ (NSString *)decryptWithText:(NSString *)sText;
/**
 使用自定义密钥加密
 @param  sText 数据内容
 @param  key 自定义密钥(少于24位末尾自动补零) */
+ (NSString *)encryptTextWithKey:(NSString *)sText key:(NSString *) key;
/**
 使用自定义密钥解密
 @param  sText 数据内容
 @param  key 自定义密钥(少于24位末尾自动补零) */
+ (NSString *)decryptTextWithKey:(NSString *)sText key:(NSString *) key;
/**
 使用默认秘钥解密
 @param  data 数据内容 */
+ (NSDictionary *)decryptWithData:(NSData *)data;


@end
```

**将字符串转化成字典**

```objc
NSData *jsonData = [jsonString dataUsingEncoding:NSUTF8StringEncoding];
 NSError *err;
 NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:jsonData options:NSJSONReadingMutableContainers error:&err];
```