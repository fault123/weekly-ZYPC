---
title: UPX动态脱壳
date: 2023-11-17 14:40:42
tags: UPX
mathjax: true
---

# UPX动态脱壳

* 找壳的入口点，即xdbg的EntryPoint，会发现有pushad或4个push。
* F8过所有push，在栈窗口中ESP右键打硬件访问断点4Bytes。F9过。
* 打开Scylla并附加进程。
* 一般pop后离真正的OEP不远了，找跳转较远的jmp，跳转地址即为真正的OEP。Scylla中输入OEP，dump到本地。
* 此时程序无法运行，但可IDA分析，因为IAT损坏。
* Scylla使用IAT Autosearch，不要选择高级搜索。再Get Imports，再Fix Dump选择刚才dump的文件。
