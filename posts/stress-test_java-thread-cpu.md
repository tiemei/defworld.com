---
title: 压力测试-如何找出CPU消耗高的JAVA线程
date: '2015-11-03'
description:
categories: 'stress-test'
---

1. 找出Java中资源消耗最大的线程id `top -H -p <java_pid>`
2. jstack打印出当前栈信息，找出对应的的线程（注意将十六进制nid转换成十进制）;也可以看这个线程的系统调用情况`sudo strace -T -c -p <java_thread_id>`
3. 看这个线程在干啥
