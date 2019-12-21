---
title: skynet_mq_init
categories:
- skynet
- mq
tags:
- mq
date: 2019-12-21
---


# 函数体
```C
void 
skynet_mq_init(int n) {
	struct global_queue *q = malloc(sizeof(*q));
	memset(q,0,sizeof(*q));
	int cap = 2;
	while (cap < n) {
		cap *=2;
	}
	
	q->cap = cap;
	q->queue = malloc(cap * sizeof(struct skynet_message*));
	Q=q;
}
```
# 结构
```C
struct message_queue {
	uint32_t handle;
	int cap;
	int head;
	int tail;
	int lock;
	struct skynet_message *queue;
};

struct global_queue {
	int cap;
	int head;
	int tail;
	int lock;
	struct message_queue ** queue;
};

struct message_remote_queue {
	int cap;
	int head;
	int tail;
	int lock;
	struct skynet_remote_message *queue;
};

static struct global_queue *Q = NULL;
```