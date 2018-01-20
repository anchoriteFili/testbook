---
layout: post
#shareSDK用于微信登录
title: shareSDK用于微信登录
#时间配置
date:   2017-12-25 15:42:21 +0800
#大类配置
categories: 三方应用
#小类配置
tag: swift
---

* content
{:toc}


### 相关链接
---

- <a href="http://wiki.mob.com/快速集成/" target="_blank">http://wiki.mob.com/快速集成/</a><br>
- <a href="http://wiki.mob.com/swift调用/" target="_blank">http://wiki.mob.com/swift调用/</a><br>
- <a href="http://wiki.mob.com/适配ios-9＋系统/" target="_blank">http://wiki.mob.com/适配ios-9＋系统/</a><br>
- <a href="http://blog.csdn.net/jiaxin_1105/article/details/71124371" target="_blank">微信支付接入那些坑1-----unrecognized selector sent to instance</a><br>
- <a href="https://github.com/anchoriteFili/ShareSDKTest" target="_blank">Git仓库代码</a><br>



<img src="/styles/images/2017-12-25-shareSDKUse/1.png" width = "80%" />

### 1. 下载相关SDK
---
- [SDK下载地址](http://www.mob.com/downloadDetail/ShareSDK/ios)


```
SDK
  | —– Required（ MOB 基础公共库目录 ）
        | —– MOBFoundation.framework：基础功能框架。（必要）
  | —– ShareSDK ( ShareSDK 目录 )
        | —– ShareSDK.framework：核心静态库。（必要）
        | —– Support （ShareSDK 各组件）
              | —– Required （ 必要 ）
                   | —– ShareSDK.bundle：ShareSDK资源文件。（必要）
                   | —– ShareSDKConnector.framework：用于ShareSDK框架与外部框架连接的代理框架插件。（使用第三方SDK时必要。）
              | —– PlatformSDK 第三方平台SDK。（不需要的平台的SDK可直接移除）
              | —– PlatformConnector 对ShareSDKConnector模块架构进行优化，根据平台进行分包。（不需要的平台的库可以移除）
              | —– Optional （ 可选 ）
                    | —– ShareSDKUI.bundle：分享菜单栏和分享编辑页面资源包。（如果自定义这些UI可直接移除）
                    | —– ShareSDKExtension.framework：对ShareSDK功能的扩展框架插件。（主要提供第三方平台登录、 一键分享、截屏分享、摇一摇分享等相关功能。需要使用以上功能时必要。）
                    | —– ShareSDKUI.framework：分享菜单栏和分享编辑页面。（如果自定义这些UI可直接移除）
                    | —– ShareSDKConfigFile.bundle:用xml来初始化或者构造分享参数的资源文件。（用代码来初始化，构造分享参数可直接移除，下载的时候也是可根据自己的要求勾选下载的）
                    | —– ShareSDKConfigFile.framework：用xml来初始化，构造分享参数，使用的分享的方法库。用代码来初始化，构造分享参数可直接移除，下载的时候也是可根据自己的要求勾选下载的）
```

<img src="/styles/images/2017-12-25-shareSDKUse/3.png" width = "80%" />

### 2. 将SDK导入到项目
---
将下载的SDK文件夹拷贝到项目中->通过addFiles方法将项目导入到项目中
<img src="/styles/images/2017-12-25-shareSDKUse/2.png" width = "80%" />

### 3. 配置开发环境
---
* [快速集成链接](http://wiki.mob.com/快速集成/)

##### 1> 添加依赖库

```
libicucore.dylib
libz.dylib
libstdc++.dylib
JavaScriptCore.framework

新浪微博SDK依赖库
ImageIO.framework
libsqlite3.dylib

QQ好友和QQ空间SDK依赖库
libsqlite3.dylib

微信SDK依赖库
libsqlite3.dylib

Instagram需要依赖库
AssetsLibrary.framework
Photos.framework

美拍需要依赖库
AssetsLibrary.framework

```

##### 2> 在plist中配置环境(只包括微信)

```xml
<key>LSApplicationQueriesSchemes</key>
    <array>
        <string>wechat</string>
        <string>weixin</string>
    </array>
    <key>qq.com</key>
    <dict>
        <key>NSExceptionAllowsInsecureHTTPLoads</key>
        <true/>
        <key>NSExceptionRequiresForwardSecrecy</key>
        <false/>
        <key>NSIncludesSubdomains</key>
        <true/>
    </dict>
    <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads</key>
        <true/>
        <key>NSExceptionDomains</key>
        <dict>
            <key>jpush.cn</key>
            <dict>
                <key>NSExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSIncludesSubdomains</key>
                <true/>
            </dict>
        </dict>
    </dict>
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleTypeRole</key>
            <string>Editor</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>wx40683bbaae3afdd4</string>
            </array>
        </dict>
    </array>
    <key>MOBAppKey</key>
    <string>a6cf409fc610</string>
    <key>MOBAppSecret</key>
    <string>60dbc04e6b16d2983056ae5be9b942f5</string>
```

##### 3> 设置Other Linker Flags 添加 -Objc -all_load

<img src="/styles/images/2017-12-25-shareSDKUse/1.png" width = "80%" />


> -Objc：加了这个参数后，链接器就会把静态库中所有的Objective-C类和分类都加载到最后的可执行文件中
>
> -all_load：会让链接器把所有找到的目标文件都加载到可执行文件中，但是千万不要随便使用这个参数！假如你使用了不止一个静态库文件，然后又使用了这个参数，那么你很有可能会遇到ld: duplicate symbol错误，因为不同的库文件里面可能会有相同的目标文件，所以建议在遇到-ObjC失效的情况下使用-force_load参数。
>
> -force_load：所做的事情跟-all_load其实是一样的，但是-force_load需要指定要进行全部加载的库文件的路径，这样的话，你就只是完全加载了一个库文件，不影响其余库文件的按需加载


##### 4> 在Bridging_Header中添加头文件

```objc
#import <ShareSDK/ShareSDK.h>
#import <ShareSDKUI/ShareSDK+SSUI.h>
#import <ShareSDKConnector/ShareSDKConnector.h>

//腾讯开放平台（对应QQ和QQ空间）SDK头文件
#import <TencentOpenAPI/TencentOAuth.h>
#import <TencentOpenAPI/QQApiInterface.h>

//微信SDK头文件
#import "WXApi.h"
// 三方登录头文件
#import <ShareSDKExtension/SSEThirdPartyLoginHelper.h>
```

##### 5> 在AppDelegate中配置相关环境

```swift
/************************ shareSDK begin******************************/
    func setUpShareSDK() {
        ShareSDK.registerActivePlatforms(
            [
                SSDKPlatformType.typeWechat.rawValue,
                SSDKPlatformType.typeQQ.rawValue
            ],
            onImport: {(platform : SSDKPlatformType) -> Void in
                switch platform {
                case SSDKPlatformType.typeWechat:
                    ShareSDKConnector.connectWeChat(WXApi.classForCoder())
                case SSDKPlatformType.typeQQ:
                    ShareSDKConnector.connectQQ(QQApiInterface.classForCoder(), tencentOAuthClass: TencentOAuth.classForCoder())
                default:
                    break
                }
        },
            onConfiguration: {(platform : SSDKPlatformType , appInfo : NSMutableDictionary?) -> Void in
                switch platform {
                case SSDKPlatformType.typeWechat:
                    //设置微信应用信息
                    appInfo?.ssdkSetupWeChat(byAppId: "wx40683bbaae3afdd4",
                                             appSecret: "e26439a5d0cc2ed60ee37a93daa85755")
                case SSDKPlatformType.typeQQ:
                    //设置QQ应用信息
                    appInfo?.ssdkSetupQQ(byAppId: "100371282",
                                         appKey: "aed9b0303e3ed1e27bae87c33761161d",
                                         authType: SSDKAuthTypeWeb)
                default:
                    break
                }
        })
    }
    /************************ shareSDK end******************************/
```

##### 6> 调取获取信息方法

```swift
ShareSDK.getUserInfo(.typeWechat) { (state: SSDKResponseState, user: SSDKUser?, error: Error?) in
    if let u = user {
       // 如果有相关信息
       print("user ************* \(u)")
    }
}
```





