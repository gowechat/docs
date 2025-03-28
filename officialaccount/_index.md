---
title: 公众号
weight: 1
type: docs
URL: "/wechat/officialaccount"
sidebar:
  open: true
---
# 公众号

开发前必读：[微信公众官方文档](https://developers.weixin.qq.com/doc/offiaccount/Getting_Started/Overview.html)

## 获取微信公众号操作对象
```go
wc := wechat.NewWechat()
//设置全局cache，也可以单独为每个操作实例设置
redisOpts := &cache.RedisOpts{
    Host:       "127.0.0.1:6379",
}
redisCache := cache.NewRedis(redisOpts)
wc.SetCache(redisCache)
cfg := &offConfig.Config{
    AppID:     "xxx",
    AppSecret: "xxx",
    Token:     "xxx",
    //EncodingAESKey: "xxxx",
    //Cache: redisCache, //也可以单独设置
}
officialAccount := wc.GetOfficialAccount(cfg)
//TODO 使用 `officialAccount` 操作公众号相关接口
```