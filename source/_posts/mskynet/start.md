---
title: skynet_start
categories:
- skynet
tags:
- start
date: 2019-12-21
---

### skynet_start

启动流程

```C

void skynet_start(struct skynet_config * config){
    skynet_harbor_init(config->master, config->local, config->harbor);
	skynet_handle_init(config->harbor);
	skynet_mq_init(config->mqueue_size);
	skynet_module_init(config->module_path);
	skynet_timer_init();
	struct skynet_context *ctx;
	ctx = skynet_context_new("logger", config->logger);
	assert(ctx);
	ctx = skynet_context_new("snlua", config->start);
	_start(config->thread);
}

```

```C

static void _start(int thread)
启动skynet 创建 thread+2 条线程 
第一条为timer
第二条为skynet_harbor_dispatch_thread
剩下的thread条为worker

```



