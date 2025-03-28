---
title: JS-SDK
type: docs
weight: 8
URL: "/wechat/officialaccount/js.html"
---
# JS-SDK

## 获取js-sdk操作实例
```go
oa := wc.GetOfficialAccount(cfg)
j:=oa.GetJs()
```

## 获取js配置
```go
GetConfig(uri string) (config *Config, err error)
```

其中 `Config` 结果为：
```go
// Config 返回给用户jssdk配置信息
type Config struct {
	AppID     string `json:"app_id"`
	Timestamp int64  `json:"timestamp"`
	NonceStr  string `json:"nonce_str"`
	Signature string `json:"signature"`
}
```

## 替换`js-ticket`取值方式

默认js-ticket是存放在sdk设置的cache，如果需要自定义取值，可以实现`credential.JsTicketHandle`接口：
```go
//JsTicketHandle js ticket获取
type JsTicketHandle interface {
	//GetTicket 获取ticket
	GetTicket(accessToken string) (ticket string, err error)
}
```

然后通过`js.SetJsTicketHandle(ticketHandle credential.JsTicketHandle)`进行设置
