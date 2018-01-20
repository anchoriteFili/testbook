---
layout: post
#标题
title:  网络申请第三方AFNetwoking_网络申请
#时间配置
date:   2015-06-11 21:36:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

### 相关链接
---

* <a href="https://github.com/AFNetworking/AFNetworking" target="_blank">https://github.com/AFNetworking/AFNetworking</a><br>
* <a href="http://www.bubuko.com/infodetail-660570.html" target="_blank">AFNetworking实现 断点续传</a><br>
* <a href="http://www.cocoachina.com/ios/20151020/13831.html" target="_blank">AFNetworking 3.0迁移指南</a><br>
* <a href="http://www.jianshu.com/p/11bb0d4dc649" target="_blank">iOS开发之AFNetworking 3.0.4使用</a><br>


##### 快速创建

```objc
// get
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager]; 

[manager GET:URL parameters:nil progress:^(NSProgress * _Nonnull downloadProgress) {  

}     
 success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {  

 NSLog(@"这里打印请求成功要做的事");  

}

failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull   error) {  

NSLog(@"%@",error);  //这里打印错误信息

}];

// post 
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];


NSMutableDictionary *parameters = @{@"":@"",@"":@""};

[manager POST:URL parameters:parameters progress:^(NSProgress * _Nonnull uploadProgress) {


} success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {


} failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {

}];
```

**暂停按钮的实现**

```objc
#pragma mark 暂停按钮点击事件
- (void)stopBtnClick:(UIButton *)sender {
    NSLog(@"book = %@",self.namelabel.text);
    
    if (!self.isDownloading) {
        [sender setTitle:@"暂停" forState:UIControlStateNormal];
        self.isDownloading = YES;
        
        AFHTTPRequestOperation *operation = [[[NSOperationQueue currentQueue] operations] objectAtIndex:0];
//        只对一个操作暂停即可完成全部暂停
        [operation pause];
        
    }else {
        [sender setTitle:@"下载" forState:UIControlStateNormal];
        self.isDownloading = NO;
        
        AFHTTPRequestOperation *operation = [[[NSOperationQueue currentQueue] operations] objectAtIndex:0];
//        继续下载操作
        [operation resume];
        
    }
}
```

**对AFNetWorking进行改造才可以从网上申请text文件的JSON数据**


> self.acceptableContentTypes = [NSSet setWithObjects:@"application/json", @"text/json", @"text/javascript", nil];

> self.acceptableContentTypes = [NSSet setWithObjects:@"application/json",`@"text/html"`, @"text/json", @"text/javascript", nil];

**参数parameters的作用：在请求序列化中在网址后边加一些请求字符串或HTTP Body**

```objc
NSString *URLString = @"http://example.com";
NSDictionary *parameters = @{@"foo": @"bar",@"baz": @[@1,@2,@3]};

[[AFHTTPRequestSerializer serializer] requestWithMethod:@"GET" URLString:URLString parameters:parameters error:nil];
```