---
title: skynet_module_init
categories:
- skynet
- module
tags:
- module
date: 2019-12-21
---

# 函数体
```C
void 
skynet_module_init(const char *path) {
	struct modules *m = malloc(sizeof(*m));
	m->count = 0;
	m->path = strdup(path);
	m->lock = 0;

	M = m;
}
```
# 结构
```C
struct modules {
	int count;
	int lock;
	const char * path;
	struct skynet_module m[MAX_MODULE_TYPE];
};

static struct modules * M = NULL;

#define MAX_MODULE_TYPE 32

```