---
title: 协助解除流程
categories: 
- mhtx
- assist
date: 2019-12-05
---

解除流程1：当发起者退出场景时，协助任务直接失效，所有参与当前协助的玩家结束当前协助。同时其他玩家也不能再接取当前协助任务。

```plantuml
@startuml
title: 解除流程1 发起者退出场景
Agent1 -> Map : 发起退出场景请求
Map -> Agent2 : 通知其他参与协助玩家 退出场景 清除协助信息
Map -> Assist : 通知协助服务 删除协助
Assist -> Gang : 通知部族协助变化
Gang -> Agent2 : 部族推送变化给其他玩家

@enduml
```

解除流程2：当发起者攻击当前协助指定boss之外的其他boss时，自动结束当前协助，同时所有参与协助的玩家结束当前协助，其他玩家不能再接取当前协助。
```plantuml
@startuml
title: 解除流程2 发起者攻击其他boss
Agent2 -> Map : 攻击其他boss
Map -> Assist : 通知协助服务 删除该玩家协助信息
Map -> Agent2 : 退出场景 清除该协助信息
Assist -> Gang : 通知部族协助变化
Gang -> Agent2 : 部族推送变化给其他玩家
@enduml
```

解除流程3： 发起者接取了其他玩家发布的协助任务时，结束当前协助，同时所有参与协助的玩家结束当前协助，其他玩家不能接取。
```plantuml
@startuml
title: 解除流程3 发起者参与其他协助
Agent1 -> Assist : 接受协助 删除自身协助
Assist -> Gang : 通知部族协助信息变化
Gang -> Agent2 : 通知玩家协助信息变化 推送协助信息
Agent2 -> Map : 同步协助信息变化 退出场景
@enduml
```

解除流程4： 参与协助玩家退出场景，自动结束自身当前协助，其他玩家可以继续接取和完成协助。
```plantuml
@startuml
title: 解除流程4 参与协助者退出场景
Agent2 -> Map : 发起退出场景请求
Map -> Assist : 通知协助服务 删除该玩家协助信息
Map -> Agent2 : 退出场景 清除该协助信息
Assist -> Gang : 通知部族协助变化
Gang -> Agent2 : 部族推送变化给其他玩家
@enduml
```

解除流程5： 参与协助玩家 主动放弃协助，结束当前协助，其他玩家可以继续接取和完成协助。
```plantuml
@startuml
title: 解除流程5 参与协助者主动放弃协助
Agent2 -> Assist : 参与者发起放弃请求
Assist -> Gang : 同步协助信息
Assist -> Agent2 : 放弃成功清除协助信息
Agent2 -> Map : 同步协助信息 退出场景
Gang -> Agent3 : 推送协助信息给其他玩家
@enduml
```

切图判断逻辑
```plantuml
@startuml

:进入场景;
if (场景Id为协助目标场景Id) then (true)
	:不做处理;
	stop
else(no)
	if (触发状态攻击) then (yes)
		:处理伤害波动上下限;
	else(no)
	endif
endif
if (触发特殊攻击) then (yes)
	:触发特殊攻击 特殊攻击进入冷却;
	:影响特殊伤害倍率/特殊伤害加深/特殊伤害减免/特殊固定增伤/特殊固定减伤取值;
else(no)
endif
:计算最终伤害;
stop
@enduml
```