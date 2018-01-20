```swift
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
```

<iframe src="https://modao.cc/app/WSAJ9P0qmzlC55qgcJiaYiIoRJjbpcU/embed" width="760" height="780" allowTransparency="false" frameborder="0"></iframe>



漂亮
























