# 消息解密

## 获取操作实例
```go
mini := wc.GetMiniProgram(cfg)
a:=mini.GetAuth()
```

## 解密数据
```go
Decrypt(sessionKey, encryptedData, iv string) (*PlainData, error)
```

其中结果为：
```go
type PlainData struct {
	OpenID    string `json:"openId"`
	UnionID   string `json:"unionId"`
	NickName  string `json:"nickName"`
	Gender    int    `json:"gender"`
	City      string `json:"city"`
	Province  string `json:"province"`
	Country   string `json:"country"`
	AvatarURL string `json:"avatarUrl"`
	Language  string `json:"language"`
	PhoneNumber     string `json:"phoneNumber"`
	PurePhoneNumber string `json:"purePhoneNumber"`
	CountryCode     string `json:"countryCode"`
	Watermark struct {
		Timestamp int64  `json:"timestamp"`
		AppID     string `json:"appid"`
	} `json:"watermark"`
}
```
根据需要取用户信息还是手机号信息