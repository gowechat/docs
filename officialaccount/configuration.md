# 配置

通常通过如下配置就可以获取到一个`officialAccount`的操作实例了。
```go
//使用memcache保存access_token，也可选择redis或自定义cache
wc := wechat.NewWechat()
redisOpts := &cache.RedisOpts{
	Host:        "127.0.0.1:6379",
	Database:    0,
	MaxActive:   10,
	MaxIdle:     10,
	IdleTimeout: 60, //second
}
redisCache := cache.NewRedis(redisOpts)
cfg := &offConfig.Config{
	AppID:     "xxx",
	AppSecret: "xxx",
	Token:     "xxx",
	//EncodingAESKey: "xxxx",
	Cache: redisCache,
}
officialAccount := wc.GetOfficialAccount(cfg)
```

**微信公众号支持的配置，如下：**

```go
//Config config for 微信公众号
type Config struct {
	AppID          string `json:"app_id"`           //appid
	AppSecret      string `json:"app_secret"`       //appsecret
	Token          string `json:"token"`            //token
	EncodingAESKey string `json:"encoding_aes_key"` //EncodingAESKey
	Cache          cache.Cache
}
```
**配置说明：**

|  参数   | 是否必须  | 说明 |
|  ----  | ----  | ----  | 
| AppID  | 是 |微信公众号APP ID |
| AppSecret  | 是 |微信公众号App Secret |
| EncodingAESKey | 否 | 如果指定则表示开启AES加密，消息和结果都会进行解密和加密 |
| Cache | 是| 指定微信公众号用到的AccessToken保存的位置，可以指定 `Memory`,`Redis`,`memcache`或者自定义Cache|
> 参数配置请前往[微信公众号后台](https://mp.weixin.qq.com)获取

## 缓存
**作用：** 缓存`access_token`，保存在独立服务上可以保证`access_token`在有效期内，`access_token`是公众号的全局唯一接口调用凭据，公众号调用各接口时都需使用`access_token`。

> 推荐使用`redis`或者`memcache`

### Redis 

实例化如下：
```go
redisOpts := &cache.RedisOpts{
    Host:        "127.0.0.1:6379", // redis host 
    Password:    "",//redis password
    Database:    0, // redis db
    MaxActive:   10, // 连接池最大活跃连接数
    MaxIdle:     10,  //连接池最大空闲连接数
    IdleTimeout: 60, //空闲连接超时时间，单位：second
}
redisCache := cache.NewRedis(redisOpts)
```

### Memcache
实例化如下：

```go
memCache:=cache.NewMemcache("127.0.0.1:11211")
```

### Memory
进程内内存
> 不推荐使用该模式用于生产

实例化如下：

```go
memoryCache:=cache.NewMemory()
```

### 自定义Cache
接口如下：

```go
//Cache interface
type Cache interface {
	Get(key string) interface{}
	Set(key string, val interface{}, timeout time.Duration) error
	IsExist(key string) bool
	Delete(key string) error
}
```
文件位置：`/cache/cache.go`

## 日志
sdk中使用`github.com/sirupsen/logrus`来进行日志的输出。
默认的日志参数为：

```go
	// 日志输出类型，Text文本
	log.SetFormatter(&log.TextFormatter{})

	// 将日志直接输出到stdout
	log.SetOutput(os.Stdout)

	// 输出日志为Debug 模式
	log.SetLevel(log.DebugLevel)
```
你可以在自己的项目中自定义logrus的输出配置来覆盖sdk中默认的行为
参考: [logrus配置文档](http://github.com/sirupsen/logrus)

## 跳过接口验证
微信公众号后台在填写接口配置信息时会进行接口的校验以确保接口是否可以正常相应。
如果想要在sdk中关闭该设置，可以通过如下方法：

```go
officialAccount := wc.GetOfficialAccount(cfg)
// 传入request和responseWriter
server := officialAccount.GetServer(req, rw)

//关闭接口验证，则validate结果则一直返回true
server.SkipValidate(true)
```
