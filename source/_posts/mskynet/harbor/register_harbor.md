---
title: register_harbor
categories:
- skynet
- harbor
tags:
- harbor
date: 2019-12-21
---

## 函数体

```C
// Call only at init
static void
register_harbor(void *request, const char *local, int harbor) {
	char tmp[1024];
	sprintf(tmp,"%d",harbor);
	size_t sz = strlen(tmp);

	zmq_msg_t req;
	zmq_msg_init_size (&req , sz);
	memcpy(zmq_msg_data(&req),tmp,sz);
	zmq_send (request, &req, 0);
	zmq_msg_close (&req);

	zmq_msg_t reply;
	zmq_msg_init (&reply);
	int rc = zmq_recv(request, &reply, 0);
	_report_zmq_error(rc);

	sz = zmq_msg_size (&reply);
	if (sz > 0) {
		memcpy(tmp,zmq_msg_data(&reply),sz);
		tmp[sz] = '\0';
		fprintf(stderr, "Harbor %d is already registered by %s\n", harbor, tmp);
		exit(1);
	}
	zmq_msg_close (&reply);

	sprintf(tmp,"%d=%s",harbor,local);
	sz = strlen(tmp);
	zmq_msg_init_size (&req , sz);
	memcpy(zmq_msg_data(&req),tmp,sz);
	zmq_send (request, &req, 0);
	zmq_msg_close (&req);

	zmq_msg_init (&reply);
	rc = zmq_recv (request, &reply, 0);
	_report_zmq_error(rc);
	zmq_msg_close (&reply);
}
```