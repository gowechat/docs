# 模板消息

## 获取模板消息实例
```go
oa := wc.GetOfficialAccount(cfg)
m:=oa.GetTemplate()
```

## 发送模板消息

```go
Send(msg *TemplateMessage) (msgID int64, err error)
```
其中 `TemplateMessage`结构为：

```go
type TemplateMessage struct {
	ToUser     string                       `json:"touser"`          // 必须, 接受者OpenID
	TemplateID string                       `json:"template_id"`     // 必须, 模版ID
	URL        string                       `json:"url,omitempty"`   // 可选, 用户点击后跳转的URL, 该URL必须处于开发者在公众平台网站中设置的域中
	Color      string                       `json:"color,omitempty"` // 可选, 整个消息的颜色, 可以不设置
	Data       map[string]*TemplateDataItem `json:"data"`            // 必须, 模板数据

	MiniProgram struct {
		AppID    string `json:"appid"`    //所需跳转到的小程序appid（该小程序appid必须与发模板消息的公众号是绑定关联关系）
		PagePath string `json:"pagepath"` //所需跳转到小程序的具体页面路径，支持带参数,（示例index?foo=bar）
	} `json:"miniprogram"` //可选,跳转至小程序地址
}
```