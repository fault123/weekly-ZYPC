---
title: Batch学习笔记
date: 2023-10-14 19:10:08
tags: Batch
mathjax: true
---

# Batch学习笔记

> 学习网站：
>
> * https://zhuanlan.zhihu.com/p/415626343
> * https://www.cnblogs.com/yhlx/articles/2866854.html
> * https://www.xiaohongshu.com/explore/6329a832000000001801beab
> * https://www.xiaohongshu.com/explore/637dd69f000000001c005f85
> * https://blog.csdn.net/weixin_43165135/article/details/127575873
> * https://zhuanlan.zhihu.com/p/446337414

## 注释

```bash
:: 注释
rem 注释
```

## echo

```bash
echo. #空行 同：echo, echo; echo+ echo[ echo] echo/ echo
echo off
echo on
echo #显示状态
@echo off #不显示本身
echo %变量%
```

## 局部变量

```bash
setlocal #局部变量
set 变量=...
endlocal
```

比较以下代码：

```bash
setlocal
set var=test & echo show %var% # 未执行完语句，显示空：show
endlocal
```

```bash
setlocal enabledelayedexpansion
set var=test & echo show !var!
endlocal # 显示：show test
```

## errorlevel

```bash
echo %errorlevel% # 默认为0，一般出错为1
```

## dir

```bash
dir /a #隐藏、系统文件
dir c: /a:d #当前 C 盘目录中目录
dir c: /a:-d#当前 C 盘目录中文件
dir c: /b/p #只文件名、分页
dir *.exe /s
```

## cd

```bash
cd /d d:sdk #同时更改盘符目录
```

## md

```
md ...
mkdir ...
```

## rd

```bash
rd ...#要求空目录
rd /s/q d:temp #文件夹、子文件夹、文件+安静模式
```

## del

```bash
del ...#不能隐藏、系统、只读
```

