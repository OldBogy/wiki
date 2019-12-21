---
title: skynet_timer_init
categories:
- skynet
- timer
tags:
- timer
date: 2019-12-21
---

# 函数体
```C
void 
skynet_timer_init(void) {
	TI = timer_create_timer();
	TI->current = _gettime();
}

static struct timer *
timer_create_timer()
{
	struct timer *r=(struct timer *)malloc(sizeof(struct timer));
	memset(r,0,sizeof(*r));

	int i,j;

	for (i=0;i<TIME_NEAR;i++) {
		link_clear(&r->near[i]);
	}

	for (i=0;i<4;i++) {
		for (j=0;j<TIME_LEVEL-1;j++) {
			link_clear(&r->t[i][j]);
		}
	}

	r->lock = 0;
	r->current = 0;

	return r;
}

static uint32_t
_gettime(void) {
	struct timespec ti;
	clock_gettime(CLOCK_MONOTONIC, &ti);
	uint32_t t = (uint32_t)(ti.tv_sec & 0xffffff) * 100;
	t += ti.tv_nsec / 10000000;

	return t;
}
```

# 结构
```C
#define TIME_NEAR_SHIFT 8
#define TIME_NEAR (1 << TIME_NEAR_SHIFT)
#define TIME_LEVEL_SHIFT 6
#define TIME_LEVEL (1 << TIME_LEVEL_SHIFT)
#define TIME_NEAR_MASK (TIME_NEAR-1)
#define TIME_LEVEL_MASK (TIME_LEVEL-1)

struct timer_event {
	uint32_t handle;
	int session;
};

struct timer_node {
	struct timer_node *next;
	int expire;
};

struct link_list {
	struct timer_node head;
	struct timer_node *tail;
};

struct timer {
	struct link_list near[TIME_NEAR];
	struct link_list t[4][TIME_LEVEL-1];
	int lock;
	int time;
	uint32_t current;
};

static struct timer * TI = NULL;
```