title: 结拜
categories: 
- sg
- sworn
tags:
- sg
- sworn
date: 2020-01-12
---

## 基础方法

### create
```plantuml
@startuml
client -> agent : swornCreate
agent -> sworn : create param
Assist -> Gang : 通知部族协助变化
Gang -> Agent2 : 部族推送变化给其他玩家
Agent2 -> Assist : 其他玩家接收协助
Assist -> Agent2 : 通知接受成功
Agent2 -> Map : 进入地图开启协助
@enduml
```

----
### cancel

----
### invite

----
### cancelInvite

----
### reply

----
### leave

----
### kickOut

----
### getList

----
### toast

----
### apply

----
### applyReply

----
### getBeInviteInfos

----
### getApplyInfos

----
### getSwornData

----
### replyCheck

----
### getMemberSize

----
### getMemberIdsByUid

----
### toastCheck

----