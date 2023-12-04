---
title: Angr符号执行笔记
date: 2023-10-29 20:56:26
tags: 符号执行
mathjax: true
---

# Angr符号执行笔记

## Angr安装

```bash
pip install angr
```

## 引子：[网鼎杯 2020 青龙组]singal

考点：**虚拟机、符号执行**

这题本身是个虚拟机题，给出了程序序列，要求找到合适的输入。本题采用符号执行最简便。

```python
import angr,sys
path="D:\\CTF-Workbench\\signal.exe"
project=angr.Project(path,auto_load_libs=False)
initial_state=project.factory.entry_state()
simulation=project.factory.simulation_manager(initial_state)
simulation.explore(find=0x0040179E,avoid=0x004016E6)
if simulation.found:
    for i in simulation.found:
        solution_state=i
        print(solution_state.posix.dumps(0))
else:
    print("no\n")
```

## 顶层接口

```python
import angr,sys
path="D:\\CTF-Workbench\\signal.exe"
project=angr.Project(path,auto_load_libs=False)
```

参数load-options指明加载方式：

| 名称                   | 描述                       | 传入参数 |
| ---------------------- | -------------------------- | -------- |
| `auto_load_libs`       | 是否自动加载程序的依赖。   | 布尔     |
| `skip_libs`            | 希望避免加载的库。         | 库名     |
| `excepts_missing_libs` | 无法解析库时是否抛出异常。 | 布尔     |
| `force_load_libs`      | 强制加载的库。             | 库名     |
| `ld_path`              | 共享库的优先搜索路径。     | 路径名   |

少加载无关结果的库能显著提高效率。

Project类中还有很多有用的方法、属性：

| 方法/属性                     | 含义       | 举例              |
| ----------------------------- | ---------- | ----------------- |
| `project.arch`                | 架构       | <Arch AMD64 (LE)> |
| `hex(project.entry)`          | 程序入口点 | 0x4023c0          |
| `project.filename`            | 文件名     | /bin/true         |
| `project.arch.bits`           | 位         | 64                |
| `project.arch.memory_endness` | 大小端     | Iend_LE           |

## 状态

```python
initial_state=project.factory.entry_state()
```

预设状态：

| 预设状态方式         | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| `entry_state`        | 初始化状态为程序运行到程序入口点处的状态。                   |
| `blank_state(addr=)` | 大多数数据都没有初始化，状态中下一条指令为addr处的指令。用于跳过降低angr效率的指令。 |
| `full_init_state`    | （不常用）共享库和预定义内容已经加载完毕，例如刚加载玩共享库。 |
| `call_status`        | （不常用）准备调用函数的状态。                               |

## 模拟管理器

挖坑待填...
