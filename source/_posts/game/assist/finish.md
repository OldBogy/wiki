---
title: 协助达成结束流程
categories: 
- mhtx
- assist
date: 2019-12-05
---

```plantuml
@startuml
title: 达成流程 boss死亡
Map -> Assist : 通知服务协助达成
Assist -> Agent1 : 通知申请者发放奖励 修改协助状态
Assist -> Agent2 : 通知参与者发放奖励 修改协助状态
Assist -> Gang : 删除协助信息通知部族服务
Gang -> Agent2 : 同步信息
@enduml
```
