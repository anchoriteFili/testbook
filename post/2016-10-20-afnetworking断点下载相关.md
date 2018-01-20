---
layout: post
#afnetworking断点下载相关
title:  afnetworking断点下载相关
#时间配置
date:   2016-10-20 14:41:50 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

### 参考资料
---

* <a href="http://www.cnblogs.com/qingche/p/5362592.html" target="_blank">iOS- 利用AFNetworking3.0+（最新AFN) - 实现文件断点下载</a><br>
* <a href="http://www.cnblogs.com/YouXianMing/p/3651462.html" target="_blank">AFNetworking 2.0使用(持续更新)</a><br>
* <a href="http://files.cnblogs.com/files/AnchoriteFiliGod/AFNetworking3.0--master.zip" target="_blank">AFNetworking3.0--master.zip</a><br>
* <a href="http://files.cnblogs.com/files/AnchoriteFiliGod/AFNetworking2.0_VC.zip" target="_blank">AFNetworking2.0_VC.zip</a><br>


### 代码相关
---

##### 1> 2.0断点下载相关

```objc
#import "ZIPViewController.h"
#import "AFNetworking.h"

@interface ZIPViewController () {
    // 下载操作
    NSURLSessionDownloadTask *_downloadTask;
}

@property (weak, nonatomic) IBOutlet UIImageView *imageView;
@property (weak, nonatomic) IBOutlet UIProgressView *progressView;

@end

@implementation ZIPViewController

- (void)downFileFromServer{
    
    NSURL *URL = [NSURL URLWithString:@"http://www.gonghuizhudi.com/file/images.zip"];
    
    NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
    
    //AFN3.0+基于封住URLSession的句柄
    AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
    
    //请求
    NSURLRequest *request = [NSURLRequest requestWithURL:URL];
    
    NSProgress *progress;
    
    // 开始下载任务
    _downloadTask = [manager downloadTaskWithRequest:request progress:&progress destination:^NSURL *(NSURL *targetPath, NSURLResponse *response)
                                              {
                                                  // 拼接一个文件夹路径
                                                  NSURL *documentsDirectoryURL = [[NSFileManager defaultManager] URLForDirectory:NSDocumentDirectory inDomain:NSUserDomainMask appropriateForURL:nil create:NO error:nil];
                                                  
                                                  // 根据网址信息拼接成一个完整的文件存储路径并返回给block
                                                  return [documentsDirectoryURL URLByAppendingPathComponent:[response suggestedFilename]];
                                                  
                                              } completionHandler:^(NSURLResponse *response, NSURL *filePath, NSError *error)
                                              {
                                                  
                                                  NSLog(@"下载结束");
                                                  // 结束后移除掉这个progress
                                                  [progress removeObserver:self
                                                                forKeyPath:@"fractionCompleted"
                                                                   context:NULL];
                                              }];
    
    // 设置这个progress的唯一标示符
    [progress setUserInfoObject:@"someThing" forKey:@"Y.X."];
    
    // 给这个progress添加监听任务
    [progress addObserver:self
               forKeyPath:@"fractionCompleted"
                  options:NSKeyValueObservingOptionNew
                  context:NULL];
    
    
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context
{
    if ([keyPath isEqualToString:@"fractionCompleted"] && [object isKindOfClass:[NSProgress class]]) {
        NSProgress *progress = (NSProgress *)object;
        NSLog(@"Progress is %f", progress.fractionCompleted);
        
        // 回到主线程，刷新progressView的显示
        dispatch_async(dispatch_get_main_queue(), ^{
            self.progressView.progress = 1.0 * progress.completedUnitCount / progress.totalUnitCount;
        });
        
        // 打印这个唯一标示符
        NSLog(@"%@", progress.userInfo);
    }
}

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
    
    [self downFileFromServer];
    
}

- (IBAction)stopDownloadBtnClick:(id)sender {
    //暂停下载
    [_downloadTask suspend];
}

- (IBAction)startDownloadBtnClick:(id)sender {
    //开始下载
    [_downloadTask resume];
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

##### 2> 3.0断点下载相关

```objc
#import "ViewController.h"
#import "AFNetworking.h"

@interface ViewController ()
{
    // 下载操作
    NSURLSessionDownloadTask *_downloadTask;
}
@end

@implementation ViewController

- (void)downFileFromServer{
    
    NSURL *URL = [NSURL URLWithString:@"http://www.gonghuizhudi.com/file/images.zip"];
    
    NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
    
    //AFN3.0+基于封住URLSession的句柄
    AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
    
    //请求
    NSURLRequest *request = [NSURLRequest requestWithURL:URL];
    
    //下载Task操作
    _downloadTask = [manager downloadTaskWithRequest:request progress:^(NSProgress * _Nonnull downloadProgress) {
        
        // @property int64_t totalUnitCount;  需要下载文件的总大小
        // @property int64_t completedUnitCount; 当前已经下载的大小
        
        // 给Progress添加监听 KVO
        NSLog(@"%f",1.0 * downloadProgress.completedUnitCount / downloadProgress.totalUnitCount);
        // 回到主队列刷新UI
        dispatch_async(dispatch_get_main_queue(), ^{
            self.progressView.progress = 1.0 * downloadProgress.completedUnitCount / downloadProgress.totalUnitCount;
        });

    } destination:^NSURL * _Nonnull(NSURL * _Nonnull targetPath, NSURLResponse * _Nonnull response) {
        
        //- block的返回值, 要求返回一个URL, 返回的这个URL就是文件的位置的路径

        NSString *cachesPath = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) lastObject];
        NSString *path = [cachesPath stringByAppendingPathComponent:response.suggestedFilename];
        return [NSURL fileURLWithPath:path];

    } completionHandler:^(NSURLResponse * _Nonnull response, NSURL * _Nullable filePath, NSError * _Nullable error) {
        // filePath就是你下载文件的位置，你可以解压，也可以直接拿来使用
        
        NSString *imgFilePath = [filePath path];// 将NSURL转成NSString
        UIImage *img = [UIImage imageWithContentsOfFile:imgFilePath];
        self.imageView.image = img;

    }];
}


- (void)viewDidLoad {
    [super viewDidLoad];
    
    //网络监控句柄
    AFNetworkReachabilityManager *manager = [AFNetworkReachabilityManager sharedManager];
    
    //要监控网络连接状态，必须要先调用单例的startMonitoring方法
    [manager startMonitoring];
    
    [manager setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
        //status:
        //AFNetworkReachabilityStatusUnknown          = -1,  未知
        //AFNetworkReachabilityStatusNotReachable     = 0,   未连接
        //AFNetworkReachabilityStatusReachableViaWWAN = 1,   3G
        //AFNetworkReachabilityStatusReachableViaWiFi = 2,   无线连接
        NSLog(@"%ld", (long)status);
    }];
    
    //准备从远程下载文件. -> 请点击下面开始按钮启动下载任务
    [self downFileFromServer];

}
- (IBAction)stopDownloadBtnClick:(id)sender {
    //暂停下载
    [_downloadTask suspend];
}
- (IBAction)startDownloadBtnClick:(id)sender {
    //开始下载
    [_downloadTask resume];
}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```