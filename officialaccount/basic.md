---
title: 基础接口 - 微信SDK开发文档
linkTitle: 基础接口
weight: 3
type: docs
URL: "/wechat/officialaccount/basic.html"
---

微信公众号操作基础接口


## 获取`access_token`
> 在每个操作实例对象中也有`GetAccessToken`方法
```go
ak,err:=officialAccount.GetAccessToken()
```
### 替换获取`access_token`的方法
默认在sdk内部是ak获取之后是存放在cache中，如果你有其他系统以及存放了ak的话，只需要实现如下interface就可以了：

```go
//AccessTokenHandle AccessToken 接口
type AccessTokenHandle interface {
	GetAccessToken() (accessToken string, err error)
}
```

然后通过wechat对象，进行设置:

```go
officialAccount.SetAccessTokenHandle=customAccessTokenHandle
```

## 获取微信服务器 IP (或IP段)
如果公众号基于安全等考虑，需要获知微信服务器的IP地址列表，以便进行相关限制，可以通过该接口获得微信服务器IP地址列表或者IP网段信息。


### 获取微信callback IP地址

```go
ipList, err:=officialAccount.GetBasic().GetCallbackIP()
```

### 获取微信API接口 IP地址

```go
ipList, err:=officialAccount.GetBasic().GetAPIDomainIP()
```

## 清理接口调用频次
> 此接口官方有每月调用限制，不可随意调用

```go
err:=officialAccount.GetBasic().ClearQuota()
```