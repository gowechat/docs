# 公众号授权流程
小程序或者公众号授权给第三方平台的流程
[官方文档](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/Authorization_Process_Technical_Description.html)

## 扫码授权
```go
// 获取公众号扫码授权页面链接
loginPageURL, err := openPlatform.GetComponentLoginPage(redirectURI, authType, "")
// ...引导用户扫码授权
// 注意: 这里微信会校验跳转到授权页的referer,必须与第三方平台后台设置的`登录授权的发起页域名`一致
// ---------
// 通过授权回调获取到的authCode换取公众号或小程序的接口调用凭据和授权信息
authInfo, err := openPlatform.QueryAuthCode(authCode)
// ...处理公众号授权后的逻辑, 存储refreshToken
```