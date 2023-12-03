---
title: Frida初体验
date: 2023-10-23 19:05:38
tags: Frida
mathjax: true
---

# Frida初体验

## Frida安装

Frida与Python与模拟器的版本是互相匹配的，大约对应关系如下：

> 雷电模拟器9 对应 Android9 对应 Frida14.2.18 对应 Python3.10

如果能这么安装上更好：

```bash
pip3 install frida==14.2.18
```

装不上自认倒霉（比如说我），去清华源上啥的自己下载报错提示的.egg文件，放在报错提示的缓存文件夹下，应该就行了。

然后下载Frida-tools：

```bash
pip3 install frida-tools==9.2.5
pip install frida-dexdump
```

去Github上：https://github.com/frida/frida/releases/tag/ 下载`frida-server-14.2.18-android-x86_64.xz`，直接解压，得到的文件可以自己重命名为`frida-server`啥的。如果报错可以换个其他版本试试。

雷电模拟器目录下有个`adb.exe`，但是版本很老了，从官网上下个新版本的，连着其他文件一起覆盖掉。

在雷电模拟器的“设置”的“其他设置”中开启root权限。用以下命令将`frida-server`push进`/data/local/tmp`文件夹。

```
adb push xxx\frida-server /data/local/tmp/
```

紧接着服务器提权：

```bash
adb shell
cd /data/local/tmp
ls -sail
chmod 777 frida-server
```

接下来转发端口：

```bash
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043
```

## Objection安装

Objection与Frida版本的对应关系也非常严格，建议对比两款工具在Github上发行时间，选择时间相近的版本。

```bash
pip3 install objection==1.11.0
```

## 实战：[SWPU 2019]easyapp

开个终端，输入以下命令打开模拟器shell：

```bash
adb shell
```

在该shell中运行上传的Frida服务器：

```bash
su
ls -sail
./frida-server
```

终端会卡住，不要关，再开个终端，对该app进程进行Objection注入：

```bash
objection -g com.example.ndktest2 explore
```

找到MainActivity类的Encrypt函数进行Hook：

```bash
android hooking watch class_method com.example.ndktest2.MainActivity.Encrypt --dump-args --dump-backtrace --dump-return
```

在模拟器中输入什么点登录Objection都会给出Hook到的返回值‘YouaretheB3ST’。

## 附录：adb常用操作

```bash
adb install <APK路径> #安装
adb uninstall <package> #卸载
adb shell #打开shell
adb logcat #查看日志
adb push xxx /data/local/tmp #上床文件
adb pull /data/local/tmp/some_file some_location #下载文件
adb forward [--no-rebind] LOCAL REMOTE #本地端口转发到远程设备端口
adb forward --list #列出转发端口
adb reverse [--no-rebind] REMOTE LOCAL #远程设备端口转发到本地
adb reverse --list #列出反向端口转发
adb kill-server #终止
adb start-server #启动
adb root #root权限重启
adb reboot #重启设备
adb reboot bootloader #重启进入bootloader
adb reboot recovery #重启进入recovery
adb remount #system分区重新挂载为可读写分区
adb connect HOST[:PORT] #TCP/IP连接设备 默认端口5555
```

