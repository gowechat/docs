---
title: 小程序码
type: docs
weight: 3
URL: "/wechat/miniprogram/qrcode.html"
---
# 小程序码

## 获取操作实例
```go
mini := wc.GetMiniProgram(cfg)
qr:=mini.GetQrcode()
```

## 获取小程序二维码，适用于需要的码数量较少的业务场景
```go
CreateWXAQRCode(coderParams QRCoder) (response []byte, err error)
```
## 获取小程序码，适用于需要的码数量较少的业务场景
```go
GetWXACode(coderParams QRCoder) (response []byte, err error)
```
## 获取小程序码，适用于需要的码数量极多的业务场景
```go
GetWXACodeUnlimit(coderParams QRCoder) (response []byte, err error)
```