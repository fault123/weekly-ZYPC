---
title: IDA在Kali Linux下的远程动态调试笔记
date: 2023-10-21 11:22:08
tags: IDA
---

# IDA在Kali Linux下的远程动态调试笔记

## 一、安装Kali Linux

已经安装过的最好重装一个，下Vmware镜像就行。

## 二、配置服务器

在IDA目录下的`dbgsrv`文件夹中有`linux_server`和`linux_server64`两个文件。在虚拟机中找个地方放起来，并给权限：

```bash
chmod a+x ./linux_server
chmod a+x ./linux_server64
```

然后利用以下命令查看Kali Linux的IP地址并记住：

```bash
ifconfig
```

把要被调试的程序放入虚拟机，并记住位置。该程序也要给权限。

运行服务器，注意32位运行`linux_server`，64位运行`linux_server64`。看到Kali开始监听。

## 三、配置IDA

打开需要调试的程序，在上方工具栏下拉框中找到`Remote Linux debugger`调试器。设置断点，按F9进入调试。

出现对话框，第一空中填Kali Linux中要被调试的程序的路径，第二空中同上，第三空填该程序的目录（即去掉文件名）。在主机名中填入Kali Linux的IP地址，端口号默认。

点击确认，开始动调。
