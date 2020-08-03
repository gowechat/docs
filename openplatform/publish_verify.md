# 全网发布校验
微信第三方平台进行全网发布的时候，会有一个全网发布接入检测的过程。  
[官方文档](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/Post_Application_on_the_Entire_Network/releases_instructions.html)

```go
wc := wechat.NewWechat()
memory := cache.NewMemory()
cfg := &openplatform.Config{
    AppID:     "xxx",
    AppSecret: "xxx",
    Token:     "xxx",
    EncodingAESKey: "xxx",
    Cache: memory,
}

//授权的第三方公众号的appID
appID := "xxx"
// 下面文档中提到的openPlatform都是这个变量
openPlatform := wc.GetOpenPlatform(cfg)
officialAccount := openPlatform.GetOfficialAccount(appID)

// 传入request和responseWriter
server := officialAccount.GetServer(req, rw)
//设置接收消息的处理方法
server.SetMessageHandler(func(msg message.MixMessage) *message.Reply {
    switch msg.InfoType {
    case message.InfoTypeVerifyTicket:
        // 在这里处理推送的VerifyTicket
        // 测试验证票据推送流程
        rw.Write([]byte("success"))
    case message.InfoTypeAuthorized:
        // 微信会推送测试号的query_auth_code过来，需要在这里获取到测试号的AuthrToken
        // 参照开放平台的`维护AuthrToken`小节
    }
    switch msg.MsgType {
    case message.MsgTypeText:
        if msg.Content == "TESTCOMPONENT_MSG_TYPE_TEXT" {
            // 测试公众号处理用户消息
            return &message.Reply{
                MsgType: message.MsgTypeText,
                MsgData: message.NewText("TESTCOMPONENT_MSG_TYPE_TEXT_callback"),
            }
        }
        // 测试公众号使用客服消息接口处理用户消息
        if strings.HasPrefix(msg.Content, "QUERY_AUTH_CODE") {
            // 立即回复空串
            rw.Write([]byte(""))
            var data = strings.Split(msg.Content, ":")
            if len(data) == 2 {
                // 调用客服接口回复消息
                customerMsg := message.NewCustomerTextMessage(string(msg.FromUserName), fmt.Sprintf("%s_from_api", data[1]))
                CheckAuthrToken(appid, refreshToken)
                officialAccount := openPlatform.GetOfficialAccount(appid)
                msgManager := message.NewMessageManager(officialAccount.GetContext())
                msgManager.Send(msg)
            }
        }
    }    
    return nil
})

//处理消息接收以及回复
err := server.Serve()
if err != nil {
    fmt.Println(err)
    return
}
//发送回复的消息
server.Send()
```
