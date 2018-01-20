---
layout: post
#标题
title:  iOS NSFileHandle 基本用法介绍
#时间配置
date:   2015-06-05 19:33:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

> NSFileHandle  此类主要是对文件内容进行读取和写入操作<br>
> NSFileMange   此类主要是对文件进行的操作以及文件信息的获取

**常用处理方法**

```objc
+ (id)fileHandleForReadingAtPath:(NSString *)path  //打开一个文件准备读取     

+ (id)fileHandleForWritingAtPath:(NSString *)path  //打开一个文件准备写入   

+ (id)fileHandleForUpdatingAtPath:(NSString *)path  //打开一个文件准备更新  

-  (NSData *)availableData; //从设备或通道返回可用的数据            

-  (NSData *)readDataToEndOfFile; //从当前的节点读取到文件的末尾               

-  (NSData *)readDataOfLength:(NSUInteger)length; //从当前节点开始读取指定的长度数据                           

-  (void)writeData:(NSData *)data; //写入数据         

-  (unsigned long long)offsetInFile;  //获取当前文件的偏移量            

-  (void)seekToFileOffset:(unsigned long long)offset; //跳到指定文件的偏移量     

-  (unsigned long long)seekToEndOfFile; //跳到文件末尾        

-  (void)truncateFileAtOffset:(unsigned long long)offset; //将文件的长度设为offset字节

-  (void)closeFile;  //关闭文件
```

**向文件追加数据**

```objc
NSString *homePath  = NSHomeDirectory( );        

NSString *sourcePath = [homePath stringByAppendingPathConmpone:@"testfile.text"];                                            

NSFileHandle *fielHandle = [NSFileHandle fileHandleForUpdatingAtPath:sourcePath];                                                        

[fileHandle seekToEndOfFile];  //将节点跳到文件的末尾          

NSString *str = @"追加的数据"                   

NSData* stringData  = [str dataUsingEncoding:NSUTF8StringEncoding];          

[fileHandle writeData:stringData]; //追加写入数据       

[fileHandle closeFile];
```

**定位数据**                    

```objc
NSFileManager *fm = [NSFileManager defaultManager];              

NSString *content = @"abcdef";                      

[fm createFileAtPath:path contents:[content dataUsingEncoding:NSUTF8StringEncoding] attributes:nil];                                                   

NSFileHandle *fileHandle = [NSFileHandle fileHandleForReadingAtPath:path];      

NSUInteger length = [fileHandle availabelData] length]; 获取数据长度       

[fileHandle seekToFileOffset;length/2]; //偏移量文件的一半           

NSData *data = [fileHandle readDataToEndOfFile];                

[fileHandle closeFile];
```

**复制文件**                           

```objc
NSFileHandle *infile, *outfile; //输入文件、输出文件          

NSData *buffer; 读取的缓冲数据                    

NSFileManager *fileManager = [NSFileManager defaultManager];   

NSString *homePath = NSHomeDirectory( );              

NSString *sourcePath = [homePath stringByAppendingPathComponent:@"testfile.txt"];  //源文件路径                                          

NSString *outPath = [homePath stringByAppendingPathComponent:@"outfile.txt"]; //输出文件路径                               

BOOL sucess  = [fileManager createFileAtPath:outPath contents:nil attributes:nil];  

if (!success)          

{                                                      

return N0;                                                                                                   

}                 

infile = [NSFileHandle fileHandleForReadingAtPath:sourcePath]; //创建读取源路径文件

if (infile == nil)                          

{                                          

return NO;                      

}                           

outfile = [NSFileHandle fileHandleForReadingAtPath:outPath]; //创建并打开要输出的文件                                                                                                                

if (outfile == nil)                            

{                                                               

return NO;                                                    

}                                             

[outfile truncateFileAtOffset:0]; //将输出文件的长度设为0         

buffer = [infile readDataToEndOfFile];  //读取数据           

[outfile writeData:buffer];  //写入输入                        

[infile closeFile];        //关闭写入、输入文件               

[outfile closeFile]；
```