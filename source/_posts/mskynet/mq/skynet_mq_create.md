---
title: skynet_mq_create
categories:
- skynet
- mq
tags:
- mq
date: 2019-12-21
---

# 函数体
```C
struct message_queue * 
skynet_mq_create(uint32_t handle) {
	struct message_queue *q = malloc(sizeof(*q));
	q->handle = handle;
	q->cap = DEFAULT_QUEUE_SIZE;
	q->head = 0;
	q->tail = 0;
	q->lock = 0;
	q->queue = malloc(sizeof(struct skynet_message) * q->cap);

	return q;
}
```