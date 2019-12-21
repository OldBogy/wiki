---
title: C创建LUA虚拟机
categories:
- skynet
- 杂记
tags:
- C
- 虚拟机
date: 2019-12-21
---

```C
//创建虚拟机函数 luaL_newstate() 返回 lua_State类型的指针
struct lua_State *L = luaL_newstate();

//把所有标准类库加载到指定的虚拟机函数 luaL_openlibs(L)
luaL_openlibs(L);

//销毁指定lua虚拟机的所有对象 函数 lua_close(L)
lua_close(L);

//加载文件 luaL_dofile(L, filename) luaL_loadfile(L, filename)
// dofile 加载并且执行了 有返回值 0 没有错误  1 有错误 
int err = luaL_dofile(L, filename);
//loadfile 加载文件 没有返回值
luaL_loadfile(L, filename);

//修改环境变量 setenv("name", value, overwrite)
/*通过此函数并不能添加或修改 shell 进程的环境变量，或者说通过setenv函数设置的环境变量只在本进程，
而且是本次执行中有效。如果在某一次运行程序时执行了setenv函数，进程终止后再次运行该程序，上次的设置是无效的，
上次设置的环境变量是不能读到的。*/
setenv("LUA_PATH", path, 1);

// lua_getglobal(L, key) 获取key 对应的元素，压入栈中
lua_getglobal(L, key);

// lua_tointeger(L, index) 
// lua_tostring(L, index)
// 从lua堆栈中取出指定位置的元素 -1 永远表示栈顶 1 永远表示栈底
int n = lua_tointeger(L, -1);
const char * str = lua_tostring(L, -1);

//lua_pop(L, num); 从lua堆栈中弹出num个元素
lua_pop(L, 1);

```

关于lua堆栈 参见：https://www.cnblogs.com/sevenyuan/p/4511808.html
堆栈索引的方式可是是正数也可以是负数，区别是：正数索引1永远表示栈底，负数索引-1永远表示栈顶。