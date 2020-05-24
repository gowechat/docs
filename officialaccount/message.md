# 消息
>当普通微信用户向公众账号发消息时，微信服务器将POST消息的XML数据包到开发者填写的URL上。

在快速入门一节中就已经演示了如果收到消息以及对消息进行回复
在SDK中通过`SetMessageHandler`方法对消息进行接收以及处理

```go
server.SetMessageHandler(func(msg message.MixMessage) *message.Reply {
    //TODO 对接收到的消息以及处理
})
```

其中`MixMessage`中包含了一个`MsgType`字段，主要会有以下几种类型：

## 接收普通消息

```go
const (
	//MsgTypeText 表示文本消息
	MsgTypeText MsgType = "text"
	//MsgTypeImage 表示图片消息
	MsgTypeImage = "image"
	//MsgTypeVoice 表示语音消息
	MsgTypeVoice = "voice"
	//MsgTypeVideo 表示视频消息
	MsgTypeVideo = "video"
	//MsgTypeShortVideo 表示短视频消息[限接收]
	MsgTypeShortVideo = "shortvideo"
	//MsgTypeLocation 表示坐标消息[限接收]
	MsgTypeLocation = "location"
	//MsgTypeLink 表示链接消息[限接收]
	MsgTypeLink = "link"
	//MsgTypeMusic 表示音乐消息[限回复]
	MsgTypeMusic = "music"
	//MsgTypeNews 表示图文消息[限回复]
	MsgTypeNews = "news"
	//MsgTypeTransfer 表示消息消息转发到客服
	MsgTypeTransfer = "transfer_customer_service"
	//MsgTypeEvent 表示事件推送消息
	MsgTypeEvent = "event"
)
```

## 接收事件推送

如果`msg.MsgType`值为`MsgTypeEvent`则表示接收到的是一个事件推送，通过`msg.EventType`可以判断事件类型，可以为以下几种：

```go
const (
	//EventSubscribe 订阅
	EventSubscribe EventType = "subscribe"
	//EventUnsubscribe 取消订阅
	EventUnsubscribe = "unsubscribe"
	//EventScan 用户已经关注公众号，则微信会将带场景值扫描事件推送给开发者
	EventScan = "SCAN"
	//EventLocation 上报地理位置事件
	EventLocation = "LOCATION"
	//EventClick 点击菜单拉取消息时的事件推送
	EventClick = "CLICK"
	//EventView 点击菜单跳转链接时的事件推送
	EventView = "VIEW"
	//EventScancodePush 扫码推事件的事件推送
	EventScancodePush = "scancode_push"
	//EventScancodeWaitmsg 扫码推事件且弹出“消息接收中”提示框的事件推送
	EventScancodeWaitmsg = "scancode_waitmsg"
	//EventPicSysphoto 弹出系统拍照发图的事件推送
	EventPicSysphoto = "pic_sysphoto"
	//EventPicPhotoOrAlbum 弹出拍照或者相册发图的事件推送
	EventPicPhotoOrAlbum = "pic_photo_or_album"
	//EventPicWeixin 弹出微信相册发图器的事件推送
	EventPicWeixin = "pic_weixin"
	//EventLocationSelect 弹出地理位置选择器的事件推送
	EventLocationSelect = "location_select"
	//EventTemplateSendJobFinish 发送模板消息推送通知
	EventTemplateSendJobFinish = "TEMPLATESENDJOBFINISH"
	//EventWxaMediaCheck 异步校验图片/音频是否含有违法违规内容推送事件
	EventWxaMediaCheck = "wxa_media_check"
)
```

## 被动回复用户消息

当收到用户向公众号发送的消息后可以对发过来的进行一次回复：

现支持一下几种回复：`文本`、`图片`、`图文`、`语音`、`视频`、`音乐`

### 回复文本

```go
server.SetMessageHandler(func(msg message.MixMessage) *message.Reply {
    //TODO 演示回复文本
    	//回复消息：演示回复用户发送的消息
		text := message.NewText(msg.Content)
		return &message.Reply{MsgType: message.MsgTypeText, MsgData: text}
})
```

### 回复图片

```go
image := message.NewImage(mediaID)
return &message.Reply{MsgType: message.MsgTypeImage, MsgData: image}
```

其中 `mediaID` 为媒体资源 ID ,可以通过素材管理(`Material`)中的接口进行上传

### 回复图文

```go
article1 := message.NewArticle("测试图文1", "图文描述", "http://图片链接地址", "http://图文链接地址")
articles := []*message.Article{article1}
news := message.NewNews(articles)
return &message.Reply{MsgType: message.MsgTypeNews, MsgData: news}
```


### 回复语音

```go
voice := message.NewVoice(mediaID)
return &message.Reply{MsgType: message.MsgTypeVoice, MsgData: voice}
```
其中 `mediaID` 为媒体资源 ID ,可以通过素材管理(`Material`)中的接口进行上传


### 回复视频

```go
video := message.NewVideo(mediaID, "标题", "描述")
return &message.Reply{MsgType: message.MsgTypeVideo, MsgData: video}
```

其中 `mediaID` 为媒体资源 ID ,可以通过素材管理(`Material`)中的接口进行上传


### 回复音乐

```go
music := message.NewMusic("标题", "描述", "音乐链接", "高质量音乐链接", "缩略图的媒体id")
return &message.Reply{MsgType: message.MsgTypeMusic, MsgData: music}
```

`NewMusic`参数说明：

|参数|是否必须|描述|
|----|----|----|
|Title|否|音乐标题|
|Description|否|音乐描述|
|MusicURL|否|音乐链接|
|HQMusicUrl|否|高质量音乐链接，WIFI环境优先使用该链接播放音乐|
|ThumbMediaId|否|缩略图的媒体id，通过素材管理中的接口上传多媒体文件，得到的id|

其中不必须的参数可以填写空字符串`""`

### 多客服消息转发

请参考 [多客服消息转发](./message-transfer.md)