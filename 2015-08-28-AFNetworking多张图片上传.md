---
layout: post
#标题
title:  AFNetworking多张图片上传
#时间配置
date:   2015-08-28 18:11:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
- (void)viewDidLoad {    
[super viewDidLoad];     // 保存每次添加的图片    
self.imageDataArray = [NSMutableArray array];   
 }

-(void)imagePickerController:(UIImagePickerController*)picker didFinishPickingMediaWithInfo:(NSDictionary *)info{    
UIImage *originalImage = [info valueForKey:UIImagePickerControllerEditedImage] ;        // 得到图片的缓存数据    
NSData * imagedata = UIImageJPEGRepresentation([originalImage imageByScalingAndCroppingForSize:CGSizeMake(originalImage.size.width, originalImage.size.height)], 0.5);    
static int index = 1;    
NSString * newImageName = [NSString stringWithFormat:@"%@%zi%@", Image_Name, index, @".jpg"];    
NSString  *jpgPath = NSHomeDirectory();    
jpgPath = [jpgPath stringByAppendingPathComponent:@"Documents"];    
jpgPath = [jpgPath stringByAppendingPathComponent:newImageName];    
[imagedata writeToFile:jpgPath atomically:YES];            
[self.imageDataArray addObject:imagedata];        
index ++ ;        
// 显示当前上传图片   
[self showUploadImage:imagedata];        
[picker dismissViewControllerAnimated:YES completion:^{    }];}

提交单张图片 ：
// 向服务器提交图片    
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];    
manager.responseSerializer = [AFHTTPResponseSerializer serializer];        
// 显示进度    
[manager POST:urlstr parameters:[self Params] constructingBodyWithBlock:^(id<AFMultipartFormData> formData)    {        
static int nindex = 1;        // 单张图片上传        
NSString * paramName = [NSString stringWithFormat:@"%@%zi", Image_Name, nindex];        
NSString * newImageName = [NSString stringWithFormat:@"%@.jpg", paramName];        
NSString * imagepath = NSHomeDirectory();        
NSString * path = [imagepath stringByAppendingPathComponent:@"Documents"];        
NSBundle * Bundle = [NSBundle bundleWithPath:path];        
NSURL    * fileURL = [Bundle URLForResource:newImageName withExtension:nil];        
[formData appendPartWithFileURL:fileURL name:paramName error:nil];    
}    success:^(AFHTTPRequestOperation *operation, id responseObject)     {             
NSString *result = [[NSString alloc] initWithData:responseObject encoding:NSUTF8StringEncoding];         
NSLog(@"完成 %@", result);             
}     failure:^(AFHTTPRequestOperation *operation, NSError *error)     {         
NSLog(@"错误 %@", error.localizedDescription);
     }];

提交多张图片：
// 向服务器提交图片    
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];    
manager.responseSerializer = [AFHTTPResponseSerializer serializer];           
 // 显示进度    
[manager POST:urlstr parameters:[self Params] constructingBodyWithBlock:^(id<AFMultipartFormData> formData)    {        
// 上传 多张图片       
 for(NSInteger i = 0; i < self.imageDataArray.count; i++)        {            
NSData * imageData = [self.imageDataArray objectAtIndex: i];            
// 上传的参数名            
NSString * Name = [NSString stringWithFormat:@"%@%zi", Image_Name, i+1];            
// 上传filename            
NSString * fileName = [NSString stringWithFormat:@"%@.jpg", Name];                        
[formData appendPartWithFileData:imageData name:Name fileName:fileName mimeType:@"image/jpeg"];
        }
    }    success:^(AFHTTPRequestOperation *operation, id responseObject)     {             
NSString *result = [[NSString alloc] initWithData:responseObject encoding:NSUTF8StringEncoding];        
 NSLog(@"完成 %@", result);
             }     failure:^(AFHTTPRequestOperation *operation, NSError *error)     {
         NSLog(@"错误 %@", error.localizedDescription);
     }];
```

```objc
#pragma mark 信息提交地方
    NSDictionary *parameters = @{};
    
    
    if (_imageData || _imageDataOne || _imageDataTwo) {
        [manager POST:api parameters:parameters constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
            
            if (_imageData) {
                [formData appendPartWithFileData:_imageData name:@"Head1" fileName:self.headImageName mimeType:@"image/jpg/file"];
            }
            
            if (_imageDataOne) {
                [formData appendPartWithFileData:_imageDataOne name:@"File1" fileName:self.attachmentImageNameOne mimeType:@"image/jpg/file"];
            }
            
            if (_imageDataTwo) {
                [formData appendPartWithFileData:_imageDataTwo name:@"File2" fileName:self.attachmentImageNameTwo mimeType:@"image/jpg/file"];
            }
            
        } success:^(AFHTTPRequestOperation *operation, id responseObject) {
            [self hideHud];
            
            int errid = [responseObject[@"ErrId"] intValue];
            
            NSLog(@"errid **************** %d",errid);
            
            if (errid != 1) {
                [Helper printUrl:api parameters:parameters];
                [self showHint:@"发送失败"];
                return;
            }
            int jobId = [responseObject[@"JobsMsgID"] intValue];
            
            [self showHint:@"发送成功"];
            [self toJobDetailForJobId:jobId];
        } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
            [self hideHud];
            TTAlertForNetError(error.code);
        }];
    } else {
        
        [manager POST:api parameters:parameters success:^(AFHTTPRequestOperation *operation, id responseObject) {
            [self hideHud];
            
            int errid = [responseObject[@"ErrId"] intValue];
            
            NSLog(@"errid **************** %d",errid);
            
            
            if (errid != 1) {
                [Helper printUrl:api parameters:parameters];
                [self showHint:@"发送失败"];
                return;
            }
            
            int jobId = [responseObject[@"JobsMsgID"] intValue];
            
            [self showHint:@"发送成功"];
            [self toJobDetailForJobId:jobId];
        } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
            [self hideHud];
            TTAlertForNetError(error.code);
        }];
    }
}
```