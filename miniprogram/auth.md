---
title: 微信登录 - 微信SDK开发文档
linkTitle: 微信登录
type: docs
weight: 1
URL: "/wechat/miniprogram/auth.html"
---

## 获取操作实例
```
mini := wc.GetMiniProgram(cfg)
a:=mini.GetAuth()
```
## 根据 jscode 获取用户 session 信息
```go
Code2Session(jsCode string) (result ResCode2Session, err error)
```