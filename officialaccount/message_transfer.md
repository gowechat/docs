---
title: 多客服消息转发
type: docs
weight: 5
URL: "/wechat/officialaccount/message_transfer.html"
---
# 多客服消息转发

```go
transferCustomer := message.NewTransferCustomer("")
return &message.Reply{MsgType: message.MsgTypeTransfer, MsgData: transferCustomer}
```

**其中`NewTransferCustomer`如果参数可选，表示指定客服账号**