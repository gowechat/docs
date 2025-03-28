---
title: 菜单管理
type: docs
weight: 9
URL: "/wechat/officialaccount/menu.html"
---
# 菜单管理

## 获取菜单操作实例
```go
oa := wc.GetOfficialAccount(cfg)
m:=oa.GetMenu()
```

## 获取当前菜单设置
```go
GetMenu() (resMenu ResMenu, err error)
```
其中`ResMenu`结果为：
```go
//ResMenu 查询菜单的返回数据
type ResMenu struct {
	util.CommonError

	Menu struct {
		Button []Button `json:"button"`
		MenuID int64    `json:"menuid"`
	} `json:"menu"`
	Conditionalmenu []resConditionalMenu `json:"conditionalmenu"`
}
```

## 添加菜单（struct方式）
```go
SetMenu(buttons []*Button) error
```
## 添加菜单（JSON方式）
直接传入json
```go
SetMenuByJSON(jsonInfo string) error
```

## 删除菜单
```go
DeleteMenu() error
```


## 添加个性化菜单（struct方式）
```go
AddConditional(buttons []*Button, matchRule *MatchRule) error
```
## 添加个性化菜单（JSON方式）
直接传入json
```go
AddConditionalByJSON(jsonInfo string) error
```

## 测试个性化菜单匹配结果
```go
MenuTryMatch(userID string) (buttons []Button, err error)
```

## 删除个性化菜单
```go
DeleteConditional(menuID int64) error
```

## 获取自定义菜单配置接口
```go
GetCurrentSelfMenuInfo() (resSelfMenuInfo ResSelfMenuInfo, err error)
```

其中`ResSelfMenuInfo`结果为：
```go
type ResSelfMenuInfo struct {
	util.CommonError

	IsMenuOpen   int32 `json:"is_menu_open"`
	SelfMenuInfo struct {
		Button []SelfMenuButton `json:"button"`
	} `json:"selfmenu_info"`
}

```