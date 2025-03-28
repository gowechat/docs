---
title: 素材管理 - 微信SDK开发文档
linkTitle: 素材管理
type: docs
weight: 7
URL: "/wechat/officialaccount/material.html"
---

## 获取素材操作实例

```go
oa := wc.GetOfficialAccount(cfg)
m:=oa.GetMaterial()
```

## 新增永久图文素材

```go
AddNews(articles []*Article)
```

## 新增其他类型永久素材(除视频)

```go
AddMaterial(mediaType MediaType, filename string)
```

## 新增永久视频素材
```go
AddVideo(filename, title, introduction string)
```

## 删除永久素材
```go
DeleteMaterial(mediaID string)
```

## 批量获取永久素材
```go
BatchGetMaterial(permanentMaterialType PermanentMaterialType, offset, count int64) 
```

## 获取永久图文素材
```go
GetNews(id string) ([]*Article, error) 
```