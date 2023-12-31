---
layout: post
title: cgo中不输出C函数的printf结果
date: 2023-12-21
Author: qws 
tags: [golang,cgo]
comments: true
toc: false
---

在 C 语言中，当使用 printf 输出结果时，输出结果会先被存储在缓冲区中，等到缓冲区满或程序结束时，才会将缓冲区中的内容一起输出。而在 Go 中，调用 C 函数时默认是带有缓冲的，所以 printf 输出的结果也会被缓冲起来。

解决方法：
1. 在 printf 输出后，显式地调用 fflush(stdout) 刷新输出缓冲区：
```c
#include <stdio.h>
void PrintInC() {
    printf("Hello, World!\n");
    fflush(stdout);
}
```
2. 在调用函数时，指定 CGO_CFLAGS 环境变量，禁用缓冲：
```shell
CGO_CFLAGS=-D_BUFFERED_IO=0 go build
```