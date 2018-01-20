---
layout: post
#标题
title:  UIWebView快速创建
#时间配置
date:   2015-05-12 20:12:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![122007574703679.png]({{ site.img_url }}F7BDD7D91D515CC010582849923D2B2A.png)

**UIWebView禁止Bounce回弹**

```objc
[(UIScrollView *)[[webview subviews] objectAtIndex:0] setBounces:NO]; 
```

**参考代码**

```objc
//    创建UIWebView
    UIWebView *webView = [[UIWebView alloc] initWithFrame:CGRectMake(0, 0, 320, 568)];
//    设置申请项
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://www.baidu.com"]];
    
   /*
    @property(nonatomic) BOOL scalesPageToFit
    一个布尔值，用于决定webpage的尺寸是否适合屏幕和使
    用者能不能改变webpage的尺寸
    如果是yes，尺寸可以改变，用户可以放大或缩小页面
    如果是NO，用户是不能改变页面大小的
    */
    webView.scalesPageToFit = YES;
    
    webView.delegate = self;
    
    self.webView = webView;
    
//    将申请的网络载入到webView中
    [webView loadRequest:request];
    [self.view addSubview:webView];
    [webView release];

//页面的后退
- (void)backBtnClicked:(UIBarButtonItem *)btn
{
    if (_webView.canGoBack) {
        [_webView goBack];
    }
}

//页面的前进
- (void)forwardBtnClicked:(UIBarButtonItem *)btn
{
    if (_webView.canGoForward) {
        [_webView goForward];
    }
}

    //方法：加载本机html
    
    //获得包中的资源路径
    NSString * resourcePath = [[NSBundle mainBundle] resourcePath];
    //获得html文件的路径
    NSString * filePath = [resourcePath stringByAppendingPathComponent:@"baidu.html"];
    //将html文件内容读取成字符串
    NSString * htmlString = [[NSString alloc] initWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil];
    //加载
    [webView loadHTMLString:htmlString baseURL:[NSURL fileURLWithPath:[[NSBundle mainBundle] bundlePath]]];

//与JS交互
    NSMutableString * sb = [[NSMutableString alloc] init];
    //拼接一段HTML代码
    [sb appendString:@"<html>"];
    [sb appendString:@"<head>"];
    [sb appendString:@"<title>欢迎您</title>"];
    [sb appendString:@"</head>"];
    [sb appendString:@"<body>"];
    [sb appendString:@"<h2>欢迎您访问<a herf=\"http://www.baidu.com\">"];
    [sb appendString:@"百度</a></h2>"];
    //HTML代码中支持JavaScript脚本
    [sb appendString:@"<script language='javascript'>"];
    [sb appendString:@"alert('欢迎使用UIWebView.....');</script>"];
    [sb appendString:@"</body>"];
    [sb appendString:@"</html>"];
    //加载并显示HTML代码
    [webView loadHTMLString:sb baseURL:[NSURL URLWithString:@"http://www.baidu.com"]];
```