---
title: skynet_mq
categories:
- skynet
- mq
tags:
- mq
date: 2019-12-21
---

消息队列

```C

消息队列结构
struct message_queue {
	int cap;
	int head;
	int tail;
	int lock;
	struct skynet_message *queue;
};

cap为队列的最大长度
初始化消息队列 
void skynet_mq_init(int cap)

消息弹出
int skynet_mq_pop(struct skynet_message *message)

消息压入
void skynet_mq_push(struct skynet_message *message)

```