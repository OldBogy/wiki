title: 乱写
categories: 
- note
tags:
- code
date: 2020-01-17
---
```lua
-- 功能开放
function manager:onSystemOpen()
	local level = self.propsMgr:getLevel()
	-- 幻形检查和开启
	self.magicshapeMgr:onShapeOpen(level, self.taskMgr:getTasks(), self.taskMgr:getHistory())

	for k,v in pairs(self.managers) do
		if v and not v:isOpen() and define.servers[k] then
			local conf = config.open_server:find(define.servers[k])
			if conf then
				local flag = true
				if conf.openLv and level < conf.openLv then
					flag = false
				end
				if conf.acceptId  and (not self.taskMgr:find(conf.acceptId) and not self.taskMgr:hasHistory(conf.acceptId)) then
					flag = false
				end
				if conf.submitId and not self.taskMgr:hasHistory(conf.submitId) then
					flag = false
				end
				if flag then
					v:onOpen()
				end
			end
		end
	end
end

-- 应该拆成俩个 一个放levelUp检查 一个放taskcomplete检查吧
-- 再次也应该把onShapeOpen改成 checkShapeOpen吧。。。
self.magicshapeMgr:onShapeOpen(level, self.taskMgr:getTasks(), self.taskMgr:getHistory())

```
