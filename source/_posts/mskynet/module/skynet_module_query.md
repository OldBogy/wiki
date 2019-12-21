---
title: skynet_module_query
categories:
- skynet
- module
tags:
- module
date: 2019-12-21
---

# 函数体
```C
struct skynet_module * 
skynet_module_query(const char * name) {
	struct skynet_module * result = _query(name);
	if (result)
		return result;

	while(__sync_lock_test_and_set(&M->lock,1)) {}

	result = _query(name); // double check

	if (result == NULL && M->count < MAX_MODULE_TYPE) {
		int index = M->count;
		void * dl = _try_open(M, name);
		if (dl) {
			M->m[index].name = name;
			M->m[index].module = dl;

			if (_open_sym(&M->m[index]) == 0) {
				M->m[index].name = strdup(name);
				M->count ++;
				result = &M->m[index];
			}
		}
	}

	__sync_lock_release(&M->lock);

	return result;
}

static struct skynet_module *
_query(const char * name) {
        int i;
        for (i=0;i<M->count;i++) {
                if (strcmp(M->m[i].name,name)==0) {
                        return &M->m[i];
                }
        }
        return NULL;
}
```