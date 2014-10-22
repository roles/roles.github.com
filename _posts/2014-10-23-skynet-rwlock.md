---
layout: post
title: "skynet源码分析-读写锁rwlock"
description: ""
category:
tags: ["c/c++", "skynet"]
---

## GCC的原子操作

参考自https://gcc.gnu.org/onlinedocs/gcc-4.1.2/gcc/Atomic-Builtins.html

* full内存屏障：双向屏障，屏障之前的读写操作全部执行之后，再执行屏障之后的读写操作
* acquire内存屏障：单向屏障，保证其后的读写操作不会提前到屏障之前
* release内存屏障：单向屏障，保证之前的读写操作已经生效
