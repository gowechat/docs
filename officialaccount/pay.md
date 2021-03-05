# 微信支付

## 微信支付流程

商户系统和微信支付系统主要交互：

1、商户server调用统一下单接口请求订单，api参见公共api[【统一下单API】](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_1)

2、商户server可通过[【JSAPI调起支付API】](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_7&index=6)调起微信支付，发起支付请求。

3、商户server接收支付通知，api参见公共api[【支付结果通知API】](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_7)

4、商户server查询支付结果，api参见公共api[【查询订单API】](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_2)

微信官方支付业务流成，请点击这里[微信支付](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_4)

## 统一下单
```go
import (
    "payConfig "github.com/silenceper/wechat/v2/pay/config"
    "payOrder" "github.com/silenceper/wechat/v2/pay/order"
)

wc := wechat.NewWechat()
// 这里本地内存保存access_token，也可选择redis，memcache或者自定cache
cfg := &payConfig.Config{
    AppID:     "xxxxxxxxxxxxxx", // 公众账号ID
    AppSecret: "xxxxxxxxxxxxxx", // 公众号密钥
    MchID:     "xxxxxxxxxxxxxx", // 商户号
    Key:       "xxxxxxxxxxxxxx", // 商户号API密钥
    NotifyURL: "xxxxxxxxxxxxxx", // 通知地址
}

// 获取微信支付实例
pay := wc.GetPay(cfg)

// 调用统一下单
params := &payOrder.Params{}

params.TotalFee = "10"          // 标价金额
params.CreateIP = "127.0.0.1"   // 终端IP
params.Body = "商品描述"         // 商品描述
params.OutTradeNo = "20210305" // 商户订单号
params.OpenID = "oMix35hVJKZIkOxXEliSONi6T-" // OpenID
params.TradeType = "JSAPI"     // 交易类型
```

## 统一下单参数说明
| 参数         | 是否必须 | 说明     |
|------------|------|--------|
| TotalFee   | 是    | 标价金额   |
| CreateIP   | 是    | 终端IP   |
| Body       | 是    | 商品描述   |
| OutTradeNo | 是    | 商户订单号  |
| OpenID     | 是    | OpenID |
| TradeType  | 是    | 交易类型   |