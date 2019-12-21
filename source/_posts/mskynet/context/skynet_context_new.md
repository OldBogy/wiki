---
title: skynet_main
categories: 
- skynet
- context
tags:
- skynet
- context
date: 2019-12-21
---

# 函数体
```C
struct skynet_context * 
skynet_context_new(const char * name, const char *parm) {
	struct skynet_module * mod = skynet_module_query(name);
	
	if (mod == NULL){
		return NULL;
	}
	void *inst = skynet_module_instance_create(mod);
	if (inst == NULL){
		return NULL;
	}
	struct skynet_context * ctx = malloc(sizeof(*ctx));
	ctx->mod = mod;
	ctx->instance = inst;
	ctx->ref = 2;
	ctx->cb = NULL;
	ctx->cb_ud = NULL;
	ctx->in_global_queue = 0;
	ctx->session_id = 0;
	char * uid = ctx->handle_name;
	uid[0] = ':';
	_id_to_hex(uid+1, ctx->handle);
	ctx->handle = skynet_handle_register(ctx);
	ctx->queue = skynet_mq_create(ctx->handle);
	// init function maybe use ctx->handle, so it must init at last

	int r = skynet_module_instance_init(mod, inst, ctx, parm);
	if (r == 0) {
		return skynet_context_release(ctx);
	} else {
		skynet_context_release(ctx);
		skynet_handle_retire(ctx->handle);
		return NULL;
	}
}
```