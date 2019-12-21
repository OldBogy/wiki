---
title: skynet_module_instance_create
categories:
- skynet
- module
tags:
- module
date: 2019-12-21
---

# 函数体
```C
void * 
skynet_module_instance_create(struct skynet_module *m) {
	if (m->create) {
		return m->create();
	} else {
		return (void *)(intptr_t)(~0);
	}
}
```
# 随记
```C
/*
* 调用模块的create函数
*/
void * skynet_module_instance_create(struct skynet_module *m)
```