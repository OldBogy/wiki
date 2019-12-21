---
title: 编译通过 运行的时候找不到so
categories:
- skynet
- 杂记
tags:
- C
date: 2019-12-21
---

今日编译skynet-master时，编译通过后 执行时报错：
```
./skynet-master 
./skynet-master: error while loading shared libraries: libzmq.so.1: cannot open shared object file: No such file or directory
```
解决方案如下：

```
find ./ -depth -name "libzmq.so.1"
cd /etc
vim ld.so.conf
```
加入 libzmq.so.1 的路径  /data/zeromq/lib
保存之后 再执行如下命令
```
ldconfig
```