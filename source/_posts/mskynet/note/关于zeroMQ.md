---
title: 一点点关于ZeroMQ
categories:
- skynet
- 杂记
tags:
- zeroMQ
date: 2019-12-21
---


### 一点点关于ZeroMQ


```C
// 指定这个ZMQ环境上下文中进行I/O操作的线程池的大小
// 执行成功返回一个不透明的context句柄。否则返回NULL并且设置errno为下列指定的值。
//void * zmq_init(int io_threads)
void *context = zmq_init(1);

//void *zmq_socket (void *context, int type);
//context 上下文
//type： ZMQ_REQ REQ-REP模式是阻塞式的，也就是说必须要client先发送一条消息给server，
//然后server才可以返回一个response给client
//type： ZMQ_PULL 消息接收器
void *request = zmq_socket(context, ZMQ_REQ);

//struct harbor
struct harbor {
	//上下文
	void * zmq_context;
	//阻塞式的消息请求 ZMQ_REQ
	void * zmq_master_request;
	//消息接收器 ZMQ_PULL
	void * zmq_local;
	//消息通知接收器 ZMQ_PULL
	void * zmq_queue_notice;

	int notice_event;
	struct hashmap *map;
	struct remote remote[REMOTE_MAX];
	struct message_remote_queue *queue;
	int harbor;
	int lock;
};

/*zmq_msg_init_size()函数会分配任何被请求的资源来存储一个size大小字节的消息，
并且初始化msg参数指定的消息，用来表示新分配到的消息。*/
int zmq_msg_init_size (zmq_msg_t *msg, size_t size);
/*
* zmq_msg_data() 函数会返回msg参数指定的消息内容的指针。
* 永远不要直接对zmq_msg_t对象进行直接操作，而是要使用zmq_msg函数族进行操作。
*/
void *zmq_msg_data (zmq_msg_t *msg);
/*
*在一个socket上发送一个消息帧
*zmq_send()函数会根据buf参数指定的内存缓冲区和len参数指定的缓冲区数据长度创建一个消息，并将消息添加到消息
*队列中。
*/
int zmq_send (void *socket, void *buf, size_t len, int flags);

/*
*释放一个ZMQ消息
*zmq_msg_close()函数会通知ZMQ公共结构，所有和参数msg向关联的资源都不再被需要，并且会被释放。
*事实上，释放与消息对象相关联的资源会被ZMQ推迟，直到此消息的所有用户或者数据缓冲区明确指明此消息已经不再被需要为止。
*应用程序要确保当一个消息不再被使用的时候应该立刻调用zmq_msg_close()进行资源释放，否则可能引起内存泄露。
*永远不要直接对zmq_msg_t对象进行直接操作，而是要使用zmq_msg函数族进行操作。
*/
int zmq_msg_close (zmq_msg_t *msg);

/*
*从一个socket上接收一个消息帧
*zmq_recv()函数会从socket参数指定的socket上接收一个消息，并把这个消息存储在buf参数指定的内存空间中。
*超过len参数指定长度的任何数据都会被删去。如果在socket上没有消息可接收，zmq_recv()函数会进行阻塞，直到请求被满足为止。
*/
int zmq_recv (void *socket, void *buf, size_t len, int flags);

```
关于 zmq_socket() 的type 参见:https://www.cnblogs.com/fengbohello/p/4354989.html