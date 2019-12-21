---
title: skynet_server
categories: 
- skynet
- context
tags:
- skynet
- context
date: 2019-12-21
---

```C
//上下文的结构(服务的结构)
struct skynet_context {
	void * instance;
	struct skynet_module * mod;
	int handle;
	int calling;
	int ref;
	char handle_name[10];
	char result[32];
	void * cb_ud;
	skynet_cb cb;
	struct message_queue *queue;
};
```
112