# 用户管理

## 获取用户管理操作实例
```go
oa := wc.GetOfficialAccount(cfg)
u:=oa.GetUser()
```

## 获取用户基本信息
```go
GetUserInfo(openID string) (userInfo *Info, err error)
```

## 设置用户备注名
```go
UpdateRemark(openID, remark string) (err error)
```

## 返回用户列表
```go
ListUserOpenIDs(nextOpenid ...string) (*OpenidList, error)
```

其中返回结果为：
```go
/ OpenidList 用户列表
type OpenidList struct {
	Total int `json:"total"`
	Count int `json:"count"`
	Data  struct {
		OpenIDs []string `json:"openid"`
	} `json:"data"`
	NextOpenID string `json:"next_openid"`
}
```