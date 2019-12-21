---
title: skynet_handle_init
categories:
- skynet
- handle
tags:
- handle
date: 2019-12-21
---

# 函数体
```C
void 
skynet_handle_init(int harbor) {
	assert(H==NULL);
	struct handle_storage * s = malloc(sizeof(*H));
	s->slot_size = DEFAULT_SLOT_SIZE;
	s->slot = malloc(s->slot_size * sizeof(struct skynet_context *));
	memset(s->slot, 0, s->slot_size * sizeof(struct handle_slot *));

	rwlock_init(&s->lock);
	s->handle_index = 0;
	s->harbor = (uint32_t)(harbor & 0xff) << HANDLE_REMOTE_SHIFT;
	s->handle_index = 1;
	s->name_cap = 2;
	s->name_count = 0;
	s->name = malloc(s->name_cap * sizeof(struct handle_name));

	H = s;
	// Don't need to free H
}
```
# 结构
```C

static struct handle_storage *H = NULL;

struct handle_storage {
	struct rwlock lock;
	
	uint32_t harbor;
	uint32_t handle_index;
	int slot_size;
	struct skynet_context ** slot;
	
	int name_cap;
	int name_count;
	struct handle_name *name;
};

struct handle_name {
	char * name;
	uint32_t handle;
};
```