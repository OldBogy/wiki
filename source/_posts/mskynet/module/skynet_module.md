---
title: skynet_module
categories:
- skynet
- module
tags:
- module
date: 2019-12-21
---

```C
void skynet_module_init(const char *path) {
	struct modules *m = malloc(sizeof(*m));
	m->count = 0;
	m->path = strdup(path);
	m->lock = 0;

	M = m;
}
```