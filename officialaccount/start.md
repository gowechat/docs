---
title: 快速入门 - 微信SDK开发文档
linkTitle: 快速入门
type: docs
weight: 1
URL: "/wechat/officialaccount/start.html"
---

以下例子就演示了一个启动一个server，接收到用户发往公众号的消息然后做处理。
> - 测试公众号可以使用[微信公众平台接口测试平台](https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login)
> - 本地环境开发的话，可以使用 [ngrok](https://ngrok.com/)工具映射出来的公网地址，方便调试。

通过`go mod`初始化一个项目，并将`wechat sdk`下载下来：

```sh
go mod init github.com/silenceper/wechat-example
go get -v github.com/silenceper/wechat/v2
```
**包含一个文件`main.go`**
> 代码中的配置参数请更改为自己的！

```go
package main

import (
	"fmt"
	"net/http"

	wechat "github.com/silenceper/wechat/v2"
	"github.com/silenceper/wechat/v2/cache"
	offConfig "github.com/silenceper/wechat/v2/officialaccount/config"
	"github.com/silenceper/wechat/v2/officialaccount/message"
)

func serveWechat(rw http.ResponseWriter, req *http.Request) {
	wc := wechat.NewWechat()
	//这里本地内存保存access_token，也可选择redis，memcache或者自定cache
	memory := cache.NewMemory()
	cfg := &offConfig.Config{
		AppID:     "xxx",
		AppSecret: "xxx",
		Token:     "xxx",
		//EncodingAESKey: "xxxx",
		Cache: memory,
	}
	officialAccount := wc.GetOfficialAccount(cfg)

	// 传入request和responseWriter
	server := officialAccount.GetServer(req, rw)
	//设置接收消息的处理方法
	server.SetMessageHandler(func(msg *message.MixMessage) *message.Reply {
		//TODO
		//回复消息：演示回复用户发送的消息
		text := message.NewText(msg.Content)
		return &message.Reply{MsgType: message.MsgTypeText, MsgData: text}
	})

	//处理消息接收以及回复
	err := server.Serve()
	if err != nil {
		fmt.Println(err)
		return
	}
	//发送回复的消息
	server.Send()
}

func main() {
	http.HandleFunc("/", serveWechat)
	fmt.Println("wechat server listener at", ":8001")
	err := http.ListenAndServe(":8001", nil)
	if err != nil {
		fmt.Printf("start server error , err=%v", err)
	}
}
```

**运行**

```sh
# go run main.go
wechat server listen at :8001
```
