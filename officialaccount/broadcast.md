# 消息群发

## 获取群发操作实例

```go
oa := wc.GetOfficialAccount(cfg)
bd:=oa.GetBroadcast()
//TODO bd.SendText
```

## 发送对象
- 发送方法的第一个参数为`broadcast.User`对象，为nil则表示发送给所有人
- `broadcast.User{TagID:1}` : 根据tagID发送
- `broadcast.User{OpenID:[]string{"openid-1","openid-2"}}`` ：根据openid发送

## 群发消息类型

### 发送文本消息

```go
bd.SendText(user *User, content string)
```

### 发送图文
```go
bd.SendNews(user *User, mediaID string,ignoreReprint bool)
```

### 发送图片

```go
bd.SendImage(user *User, images *Image)
```

### 发送语音

```go
bd.SendVoice(user *User, mediaID string)
```

### 发送视频

```go
bd.SendVideo(user *User, mediaID string,title,description string)
```

