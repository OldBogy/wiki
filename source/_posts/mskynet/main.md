---
title: skynet_main
categories: 
- skynet
tags:
- skynet
- main
date: 2019-12-21
---

```C
skynet_main.c

struct skynet_config config;
config.thread = 8;
config.mqueue_size = 256;
config.module_path = "./";
config.logger = NULL; 
skynet_start(&config);

```

创建一份配置文件，然后启动skynet