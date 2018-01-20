---
layout: post
#微信登录简单封装
title:  微信登录简单封装
#时间配置
date:   2017-12-28 15:42:21 +0800
#大类配置
categories: document
#小类配置
tag: swift
---

* content
{:toc}

### 逻辑展示
---

<iframe src="https://modao.cc/app/WSAJ9P0qmzlC55qgcJiaYiIoRJjbpcU/embed" width="760" height="780" allowTransparency="false" frameborder="0"></iframe>

### 相关资料
---
* [iOS接入指南](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=1417694084&token=&lang=zh_CN)<br>
* [移动应用微信登录开发指南](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419317851&token=&lang=zh_CN)<br>
* [个人日记保存点](http://www.cnblogs.com/AnchoriteFiliGod/diary/2017/12/21/8080913.html)

### 接入流程
---
#### Podfile信息

```
target 'WechatLogTest' do
end
platform :ios, '8.0'
use_frameworks!
pod 'WechatOpenSDK'
```

#### plist中环境的设置


```
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
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleTypeRole</key>
            <string>Editor</string>
            <key>CFBundleURLName</key>
            <string>weixin</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>wx0cd8451d269de523</string>
            </array>
        </dict>
    </array>
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
```


#### 创建HYHttpTool.swift
```swift

import UIKit

let WXAPPID = "wx0cd8451d269de523"
let WXSECRET = ""
let USER_DEFAULT = UserDefaults.standard // 获取userDefault
let WXRefreshToken = "WXLogTool_refreshToken"
let WXAccessToken = "WXLogTool_accessToken"
let WXOpenID = "WXLogTool_openid"
let WXUnionid = "WXLogTool_unionid"


class WXLogTool: NSObject, WXApiDelegate {
    
    /**
     * 创建单例，使用实例：let ClassName = HYHttpTool.shared
     */
    static let shared = WXLogTool.init()
    private override init() {}
    
    /// 注册appid
    func registerApp() {
        WXApi.registerApp(WXAPPID)
    }
    
    /// 收到一个来自微信的请求，第三方应用程序处理完后调用sendResp向微信发送结果
    ///
    /// - Parameter req: 具体请求内容，是自动释放的
    func onReq(_ req: BaseReq!) {
        
        if req.isKind(of: GetMessageFromWXReq.self) {
            // 微信请求APP提供内容，需要app提供内容后使用senRsp返回
            print("微信请求App提供内容，App要调用sendResp:GetMessageFromWXResp返回给微信")
        } else if req.isKind(of: ShowMessageFromWXReq.self) {
            
            let tmp: ShowMessageFromWXReq = req as! ShowMessageFromWXReq
            
            // 显示微信传过来的内容
            let msg: WXMediaMessage = tmp.message
            let obj: WXAppExtendObject = msg.mediaObject as! WXAppExtendObject
            
            print("标题：\(msg.title) 内容：\(msg.description) 附带信息：\(obj.extInfo) 缩略图：\(msg.thumbData.count)")
        } else if req.isKind(of: LaunchFromWXReq.self) {
            // 从微信启动app
            print("从微信启动app")
        }
    }
    
    /// 发送一个sendReq后，收到微信的回应
    ///
    /// - Parameter resp: 具体的回应内容，是自动释放的
    func onResp(_ resp: BaseResp!) {
        
        switch resp.errCode {
        case 0: // 用户同意
            // 调用相关方法
            let aresp: SendAuthResp = resp as! SendAuthResp
            print("code *********** \(aresp.code)")
            weChatCallBackWithCode(code: aresp.code)
            break
        case -4: // 用户拒绝授权
            break
        case -2: // 用户取消
            break
        default:
            break
        }
        
    }
    
    
    /** https://api.weixin.qq.com/sns/oauth2/access_token
     *  链接返回数据
     access_token : 接口调用凭证
     *  expires_in : access_token接口调用凭证超时时间，单位（秒）
     *  refresh_token : 用户刷新access_token
     *  openid : 授权用户唯一标识
     *  scope : 用户授权的作用域，使用逗号（,）分隔
     *  unionid : 当且仅当该移动应用已获得该用户的userinfo授权时，才会出现该字段
     */
    func weChatCallBackWithCode(code: String) {
        
        let urlString = "https://api.weixin.qq.com/sns/oauth2/access_token"
        print("urlString ************** \(urlString)")
        
        HYHttpTool.post(url: urlString, param: ["appid":WXAPPID,"secret":WXSECRET,"code":code,"grant_type":"authorization_code",]) { [weak self] (response, result) in
            print("code 获取 ************ \(String(describing: result.value))")
            
            let dic: NSDictionary = result.value as! NSDictionary
            if let errmsg = dic.value(forKey: "errmsg") {
                print("errmsg ************** \(errmsg)")
                return
            }
            
            USER_DEFAULT.set(dic.value(forKey: "refresh_token") as! String, forKey: WXRefreshToken)
            USER_DEFAULT.set(dic.value(forKey: "access_token") as! String, forKey: WXAccessToken)
            USER_DEFAULT.set(dic.value(forKey: "openid") as! String, forKey: WXOpenID)
            

            self?.getUserInfoWithAccessToken(accessToken:  dic.value(forKey: "access_token") as! String, openId: dic.value(forKey: "openid") as! String)
        }
        
    }
    
    /// 获取微信用户信息方法
    ///
    /// - Parameters:
    ///   - accessToken: 接口调用凭证
    ///   - openId: 授权用户唯一标识
    func getUserInfoWithAccessToken(accessToken: String, openId: String) {
        
        let urlString = "https://api.weixin.qq.com/sns/userinfo"
        
        HYHttpTool.post(url: urlString, param: ["access_token":accessToken,"openid":openId]) { (response, result) in
            print("获取微信用户信息方法 ************ \(String(describing: result.value))")
            let dic: NSDictionary = result.value as! NSDictionary
            
            if let errmsg = dic.value(forKey: "errmsg") {
                print("errmsg ************** \(errmsg)")
                return
            }
        }
    }
    
    
    /// 使用refresh_token刷新access_token
    ///
    /// - Parameter refreshToken: 用户刷新access_token
    func refreshAccessToken(refreshToken: String) {
        
        let urlString = "https://api.weixin.qq.com/sns/oauth2/refresh_token"
        
        HYHttpTool.post(url: urlString, param: ["appid":WXAPPID,"grant_type":"refresh_token","refresh_token":refreshToken]) { [weak self] (response, result) in
            print("使用refresh_token刷新access_token ************ \(String(describing: result.value))")
            let dic: NSDictionary = result.value as! NSDictionary
            
            if let errmsg = dic.value(forKey: "errmsg") {
                // 如果刷新失败，表示过期，可进行再次申请授权
                self?.sendAuthRequest()
                print("errmsg ************** \(errmsg)")
            } else {
                // 如果刷新成功，则根据refreshToken获取获取相关信息
                
                USER_DEFAULT.set(dic.value(forKey: "refresh_token") as! String, forKey: WXRefreshToken)
                USER_DEFAULT.set(dic.value(forKey: "access_token") as! String, forKey: WXAccessToken)
                USER_DEFAULT.set(dic.value(forKey: "openid") as! String, forKey: WXOpenID)
                
                self?.getUserInfoWithAccessToken(accessToken:  dic.value(forKey: "access_token")  as! String, openId: dic.value(forKey: "openid") as! String)
            }
        }
    }
    
    
    /// 判断本地的 access_token 是否可用
    ///
    /// - Parameters:
    ///   - access_token: 调用接口凭证
    ///   - openId: 普通用户标识，对该公众帐号唯一
    ///   - complete: 返回处理结果
    func accessTokenUsable(access_token: String, openId: String, complete: @escaping(Bool) -> Void) {
        
        let urlString = "https://api.weixin.qq.com/sns/auth"
        
        HYHttpTool.post(url: urlString, param: ["access_token":access_token,"openid":openId]) { (response, result) in
            print("判断本地的 access_token 是否可用 ************ \(String(describing: result.value))")
            let dic: NSDictionary = result.value as! NSDictionary
            
            if let errmsg = dic.value(forKey: "errmsg") {
                print("errmsg ************** \(errmsg)")
                
                if errmsg as! String == "ok" {
                    complete(true)
                } else {
                    complete(false)
                }
            }
        }
    }
    
    /// 登录点击方法
    func weChatLogin() {
        
        if WXApi.isWXAppInstalled() {
            print("****** 安装了微信APP *********")
        } else {
            print("****** 未安装微信 *********")
            return
        }
        
        if (USER_DEFAULT.object(forKey: WXAccessToken) != nil) {

            accessTokenUsable(access_token: USER_DEFAULT.object(forKey: WXAccessToken) as! String, openId: USER_DEFAULT.object(forKey: WXOpenID) as! String, complete: { [weak self] (isOK) in

                if isOK {
                    print("isOK === yes")
                    // 如果还可以用，则直接去申请用户信息
                    self?.getUserInfoWithAccessToken(accessToken:  USER_DEFAULT.object(forKey: WXAccessToken) as! String, openId: USER_DEFAULT.object(forKey: WXOpenID) as! String)
                } else {
                    print("isOK === no")
                    // 如果不可以用，则调取refreshAccessToken
                    // 如果存在则使用refresh_token刷新access_token
                    self?.refreshAccessToken(refreshToken: USER_DEFAULT.object(forKey: WXRefreshToken) as! String)
                }

            })
        } else {
            // 如果不存在则直接授权
            sendAuthRequest()
        }
    }
    
    /// 授权方法，直接跳转到微信进行授权
    func sendAuthRequest() {
        let req: SendAuthReq = SendAuthReq()
        req.scope = "snsapi_userinfo"
        req.state = "WechatLogTest"
        WXApi.send(req)
    }

}
```

#### Appdelegate中设置相关环境
```swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.

        // 注册信息
        WXLogTool.shared.registerApp()
        return true
    }

    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        return WXApi.handleOpen(url, delegate: WXLogTool.shared) // 设置代理
    }

```
