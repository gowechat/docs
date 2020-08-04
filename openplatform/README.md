# 微信开放平台

状态：beta

[官方文档](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/Third_party_platform_appid.html)

## 快速入门

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
    if msg.InfoType == message.InfoTypeVerifyTicket {
        componentVerifyTicket, err := openPlatform.SetComponentAccessToken(msg.ComponentVerifyTicket)
        if err != nil {
            log.Println(err)
            return nil
        }
        //debug 
        fmt.Println(componentVerifyTicket)
        rw.Write([]byte("success"))
        return nil
    }
    //handle other message
    //
    
    
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

## 代第三方公众号 - 发起网页授权

```go
//第三方公众号appid
appid := ""
officialAccount := openPlatform.GetOfficialAccount(appid)
oauth := officialAccount.PlatformOauth()
//重定向到微信oauth授权登录
oauth.Redirect(rw, req, callback, "snsapi_userinfo", "", appid)
```

## 代第三方公众号 - 通过网页授权的code 换取access_token
```go
officialAccount := openPlatform.GetOfficialAccount(appid)
componentAccessToken, err := openPlatform.GetComponentAccessToken()
if err != nil {
    fmt.Println(err)
}
accessToken, err := officialAccount.PlatformOauth().GetUserAccessToken(code, appid, componentAccessToken)
if err != nil {
    fmt.Println(err)
}
fmt.Println(accessToken)
// 通过accessToken获取用户信息请参考微信公众号的业务
```

## 维护AuthrToken
AuthrToken是平台代第三方公众号调用微信接口的凭据，通过第三方公众号授权给平台时得到的refreshToken来获取

```go
func CheckAuthrToken(appid, refreshToken string) {
    // 获取authrToken
    token, err := openPlatform.GetAuthrAccessToken(appid)
    if err != nil {
        fmt.Println(err)
    }
    if token == "" {
        openPlatform.RefreshAuthrToken(appid, refreshToken)
    }
}
```

## 代第三方公众号 - 调用微信接口（以发送微信模板消息为例）
平台代第三方公众号调用微信接口，需要在调用前确保AuthrToken有效，其余操作与公众号一致。  
```go
import "github.com/silenceper/wechat/v2/officialaccount/message"

// 在代第三方公众号调用微信接口的时候，需要确保AuthrToken有效
// 这里的appid是第三方公众号的appid
CheckAuthrToken(appid, refreshToken)
msg := &message.TemplateMessage{
    ToUser:     openid,
    TemplateID: templateID,
    URL:        url,
    Data:       data,
}
officialAccount := openPlatform.GetOfficialAccount(appid)
template := message.NewTemplate(officialAccount.GetContext())
msgID, err := template.Send(msg)
if err != nil {
    fmt.Println(err)
}
fmt.Println(msgID)
```
> 微信的部分接口（如：获取jsconfig信息）区分了第三方平台调用和公众号直接调用的地址，在文档下方单独进行说明。
> 
## 代第三方公众号 - 获取jsconfig信息
```go
CheckAuthrToken(appid, refreshToken)
jsConfig, err := openPlatform.GetOfficialAccount(appid).PlatformJs().GetConfig(uri, appid)
if err != nil {
    fmt.Println(err)
}
fmt.Println(jsConfig)
```