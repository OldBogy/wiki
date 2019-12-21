---
title: skynet_harbor_init
categories:
- skynet
- harbor
tags:
- harbor
date: 2019-12-21
---

## 函数声明：
```C
void skynet_harbor_init(const char * master, const char *local, int harbor);

```
## 参数解释：
```C 
//"tcp://127.0.0.1:2012"
const char * master

//"tcp://127.0.0.1:2525"
const char * local

//
int harbor
```
## 函数实现：
```C
void 
skynet_harbor_init(const char * master, const char *local, int harbor) {
	if (harbor <=0 || harbor>255 || strlen(local) > 512) {
		fprintf(stderr,"Invalid harbor id\n");
		exit(1);
	}
		//初始化一个zmq环境上下文
	void *context = zmq_init(1);
	//创建套接字 ZMQ_REQ
	void *request = zmq_socket(context, ZMQ_REQ);
	//连接在master这个终点上
	int r = zmq_connect(request, master);
	if (r<0) {
		fprintf(stderr, "Can't connect to master: %s\n",master);
		exit(1);
	}
	//创建一个ZMQ_PULL的套接字
	void *harbor_socket = zmq_socket(context, ZMQ_PULL);
	//绑定在local上用来接收消息
	r = zmq_bind(harbor_socket, local);
	if (r<0) {
		fprintf(stderr, "Can't bind to local : %s\n",local);
		exit(1);
	}

	register_harbor(request, local, harbor);
	printf("Start harbor on : %s\n",local);

	struct harbor * h = malloc(sizeof(*h));
	memset(h, 0, sizeof(*h));
	h->zmq_context = context;
	h->zmq_master_request = request;
	h->zmq_local = harbor_socket;
	h->map = _hash_new();
	h->harbor = harbor;
	h->queue = skynet_remotemq_create();
	h->zmq_queue_notice = zmq_socket(context, ZMQ_PULL);
	r = zmq_bind(h->zmq_queue_notice, "inproc://notice");
	assert(r==0);

	Z = h;
}
```
