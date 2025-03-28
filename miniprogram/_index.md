---
title: 小程序
weight: 2
type: docs
URL: "/wechat/miniprogram/"
sidebar:
  open: true
---

# 小程序


## 获取小程序操作对象
```go
wc := wechat.NewWechat()
memory := cache.NewMemory()
cfg := &miniConfig.Config{
    AppID:     "xxx",
    AppSecret: "xxx",
    Cache: memory,
}
miniprogram := wc.GetMiniProgram(cfg)
//TODO 调用对应接口
miniprogram.GetAnalysis().GetAnalysisDailyRetain()
```