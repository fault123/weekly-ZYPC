---
title: Linux命令笔记
date: 2023-10-25 09:01:15
tags: Shell
mathjax: true
---

# Linux命令笔记

## 基础命令

### 时间日期

```bash
date
```

### 月份日历

```bash
cal
```

### 磁盘可用空间

```bash
df
```

### 可用内存容量

```bash
free
```

### 结束终端会话

```bash
exit
```

## 导航&探索

### 当前工作目录

```bash
pwd
```

### 改变当前目录

```bash
ch - #切换回前一个工作目录
ch ~user_name #更换为用户user_name主目录
```

### ls

```bash
ls
ls /usr
ls ~ /usr
```

| 选项                | 描述                                           |
| ------------------- | ---------------------------------------------- |
| -a,--all            | 列出所有文件。                                 |
| -A,--almost-all     | 同上 不列出.和..。                             |
| -d,--directory      | 与-l结合使用，查看目录详细信息，而非其中内容。 |
| -F,--classify       | 名称后加类型指示符。                           |
| -h,--human-readable | 人类可读形式显示文件大小。                     |
| -l                  | 长格式输出。                                   |
| -r,--reverse        | 降序，一般字母升序。                           |
| -S                  | 按照大小排序。                                 |
| -t                  | 按照修改日期排序。                             |

### file

确定文件类型。

```bash
file filename
```

### less

查看文本文件。

```bash
less filename
```

| 命令             | 操作                 |
| ---------------- | -------------------- |
| Page Up或b       | 后翻一页。           |
| Page Down或Space | 前翻一页。           |
| 上方向键         | 向后一行。           |
| 下方向键         | 向前一行。           |
| G                | 移动到文本文件末尾。 |
| IG或g            | 移动到文本文件开头。 |
| /characters      | 向前搜索指定字符串。 |
| n                | 重复上一次搜索。     |
| h                | 显示帮助信息。       |
| q                | 退出less。           |

### 系统目录

| 目录           | 注释                                                         |
| -------------- | ------------------------------------------------------------ |
| /              | 根目录。                                                     |
| /bin           | 系统引导执行的二进制可执行文件。                             |
| /boot          | Linux内核、初始化RAM磁盘映像、引导装载器。                   |
| /dev           | 包含设备节点的特殊目录。                                     |
| /etc           | 包含系统范围的所有配置文件。例如c：rontab何时执行自动化作业；fstab存储设备及其关联的挂载点；passwd所有用户信息。 |
| /home          | 每个用户都有各自的目录。                                     |
| /lib           | 系统核心程序用到的共享库文件。类似DLL。                      |
| /lost+found    | 格式化过的分区或设备包含该目录，用于文件系统损坏时的部分恢复。一般都是空的。 |
| /media         | 各种可移动存储设备的挂载点。                                 |
| /mnt           | 同上，手动挂载。                                             |
| /opt           | 存放安装的商业软件。                                         |
| /proc          | 内核的窥探孔，可使用户了解到内核如何管理计算机。文件全部可读。并非真实文件系统，内核维护的虚拟文件系统。 |
| /root          | 超级用户主目录。                                             |
| /sbin          | 系统二进制可执行文件。                                       |
| /tmp           | 各种程序生成的临时文件。                                     |
| /usr           | 普通用户用到的所有程序和支持文件。                           |
| /usr/bin       | Linux发行版安装的程序。                                      |
| /usr/lib       | /usr/bin中程序用到的共享库。                                 |
| /usr/local     | 计划在系统范围内使用的程序。                                 |
| /usr/sbin      | 系统管理工具。                                               |
| /usr/share     | /usr/bin中程序用到的共享数据（配置文件、图标、桌面背景、声音文件）。 |
| /usr/share/doc | 大部分软件包自带文档。                                       |
| /var           | 可能会改变的数据（数据库、假脱机文件、用户邮件）。           |
| /var/log       | 日志文件、系统活动记录。比较有用的：/var/log/messages和/var/log/syslog。 |

## 操作文件和目录

### 通配符

| 通配符        | 含义                                        |
| ------------- | ------------------------------------------- |
| *             | 匹配任意多个字符。                          |
| ?             | 匹配任意单个字符。                          |
| [characters]  | 匹配属于字符集合character中任意单个字符。   |
| [!characters] | 匹配不属于字符集合character中任意单个字符。 |
| [[:class:]]   | 匹配属于字符类class中任意单个字符。         |

> 匹配点号开始的文件（隐藏文件）使用：
>
> ```bash
> ls -d .[!.]*
> ```

### 字符类

| 字符类    | 含义                       |
| --------- | -------------------------- |
| [:alnum:] | 匹配任意单个字母数字字符。 |
| [:alpha:] | 匹配任意单个字母。         |
| [:digit:] | 匹配任意单个数字。         |
| [:lower:] | 匹配任意单个小写字母。     |
| [:upper:] | 匹配任意单个大写字母。     |

### mkdir

```bash
mkdir directory...
```

> ‘...’表示可重复出现该参数。

### cp

单个文件或目录item1复制到文件或目录item2：

```bash
cp item1 item2
```

多个文件或目录item复制到目录directory中：

```bash
cp item... directory
```

| 选项             | 含义                                 |
| ---------------- | ------------------------------------ |
| -a,--archive     | 复制包括所有权与权限在内的所有属性。 |
| -i,--interactive | 覆盖文件之前提示用户确认。           |
| -r,--recursive   | 递归复制目录及其内容。               |
| -u,--update      | 只复制目标目录中不存在或更新的文件。 |
| -v,--verbose     | 显示相关信息。                       |

### mv

```bash
mv item1 item2
mv item... directory
```

| 选项             | 含义 |
| ---------------- | ---- |
| -i.--interactive |      |
| -u,--update      |      |
| -v,--verbose     |      |

### rm

```bash
rm item...
```

| 选项             | 含义                             |
| ---------------- | -------------------------------- |
| -i,--interactive |                                  |
| -r,--recursive   |                                  |
| -f,--force       | 忽略不存在的文件，不提示，屏蔽-i |
| -v,--verbose     |                                  |

### ln

创建硬链接：

```bash
ln file link
```

创建符号链接：

```bash
ln -s item link
```

## 命令

### type

显示指定的命令属于哪种类型。

```bash
type command
```

### which

显示可执行文件的位置。

```bash
which ls
```

### help

获取内建命令帮助信息。

```bash
help command
```

### man

显示命令手册页。

```bash
man program
```

搜索手册页内容：

```bash
man section search_term
```

例如：

```bash
man 5 passwd
```

| 节   | 内容                               |
| ---- | ---------------------------------- |
| 1    | 用户命令。                         |
| 2    | 系统调用的编程接口。               |
| 3    | C库函数的编程接口。                |
| 4    | 特殊文件，例如设备节点和驱动程序。 |
| 5    | 文件格式。                         |
| 6    | 游戏和娱乐，例如屏幕保护程序。     |
| 7    | 杂项。                             |
| 8    | 系统管理命令。                     |

### apropos

显示适合的命令清单。同`man -k`。

```bash
apropos command
```

### whatis

显示手册页的单行简述。

```bash
whatis command
```

### info

手册页包含超链接。

```bash
info [command]
```

| 命令               | 操作                             |
| ------------------ | -------------------------------- |
| ?                  | 显示命令帮助。                   |
| Page Up或BackSpace | 显示上一页。                     |
| Page Down或Space   | 显示下一页。                     |
| n                  | 显示下一个节点。                 |
| p                  | 显示上一个节点。                 |
| u                  | 显示当前节点的父节点，通常菜单。 |
| Enter              | 进入光标所在的超链接。           |
| q                  | 退出。                           |

### alias

创建自定义命令。

```bash
alias foo='cd /usr; ls; cd -'
foo
unalias foo
alias #显示所有别名
```

## 重定向

### 标准输入、输出、错误重定向

#### 标准输出重定向

```bash
ls -l /bin/usr > ls-output.txt
```

追加到文件尾部：

```bash
ls -l /bin/usr >> ls-output.txt
```

#### 标准错误重定向

```bash
ls -l /bin/usr 2> ls-output.txt
```

标准输出和标准错误重定向到同一个文件中：

```bash
ls -l /bin/usr > ls-output.txt 2>&1
ls -l /bin/usr &> ls-output.txt
ls -l /bin/usr &>> ls-output.txt #追加
```

#### 位桶

```bash
ls -l /bin/usr 2> /dev/null
```

#### cat

拼接文件。

```bash
cat filename...
```

例如：

```bash
cat movie.mpeg.0* > movie.mpeg
```

#### 标准输入重定向

```bash
cat < lazy_dog.txt
```

### 管道

#### sort

```bash
ls /bin /usr/bin | sort | less
```

#### uniq

重复行：

```bash
ls /bin /usr/bin | sort | uniq | less #删除重复行
ls /bin /usr/bin | sort | uniq -d | less #报告重复行
```

#### wc

统计文件中行数、单词数、字符数。

```bash 
ls /bin /usr/bin | sort | uniq | wc
ls /bin /usr/bin | sort | uniq | wc -l #只输出行数
```

#### grep

输出模式匹配的行。

```bash
ls /bin /usr/bin | sort | uniq | grep zip
```

| 选项 | 含义                       |
| ---- | -------------------------- |
| -i   | 忽略字母大小写。           |
| -v   | 只输出不匹配指定模式的行。 |

#### head/tail

输出前/后10行内容。

```bash 
head ls-output.txt
tail ls-output.txt
```

| 选项 | 命令                                         |
| ---- | -------------------------------------------- |
| -n 5 | 输出前/后5行。                               |
| -f   | 持续观察该文件，有追加立即显示，直到Ctrl+C。 |

#### tee

读取标准输入，将输出结果写入标准输出或文件。

```bash
ls /usr/bin | tee ls.txt | grep zip
```

## Shell特性

### 扩展

#### 路径名扩展

```bash
echo D*
echo *s
echo [[:upper:]]*
echo /usr/*/share
echo -d .*
```

#### 浪纹线扩展

```bash
echo ~
echo ~foo
```

#### 算数扩展

```bash
echo $((2+2))
echo $(((5**2)*3))
```

#### 花括号扩展

```bash
echo Front-{A,B,C}-Back
echo Number_{1..5}
echo {01..15}
echo {001..15}
echo a{A{1,2},B{3,4}}b
```

#### 参数扩展

```bash
echo $USER
printenv | less #查看可用的变量列表
```

#### 命令替换

```bash
echo $(ls)
ls -l $(which cp)
file $(ls -d /usr/bin/* | grep zip)
```

### 引用

#### 双引号

防止单词分割。参数、算数、命令扩展仍会发生。

```bash
ls -l "two words.txt"
```

> **注意！**
>
> 以下写法会将输出结果中的换行视为分隔符，即包含38个参数的命令。输出结果将以一行形式出现。
>
> ```bash
> echo $(cal)
> ```
>
> 而以下写法输出格式正确：
>
> ```bash
> echo "$(cal)"
> ```

#### 单引号

同双引号，但禁用所有扩展。

#### 转义字符

略。

#### 反斜线转义序列

| 转义序列 | 含义     |
| -------- | -------- |
| \a       | 响铃。   |
| \b       | 退格符。 |
| \n       | 新行符。 |
| \r       | 回车符。 |
| \t       | 制表符。 |

可使用`echo -e`或`$''`解释反斜线转义序列。

```bash
sleep 10;echo -e "Time's up\a"
sleep 10;echo "Time's up" $'\a'
```

## 历史记录

```bash
history | less
history | grep /usr/bin
```

默认1000个命令，以下表示扩展第88行内容：

```bash
!88
```

### 历史扩展

```bash
!! #重复上一个命令
!number #重复第number个命令
!string #重复以string 开头的最后一个命令
!?string #重复包含string的最后一个命令
```

### script

记录整个Shell对话到文件file中：

```bash
script file
```

如果没有指定file，则保存在typescript中

## 权限

### id

查看用户身份信息：

```bash
id
```

文件属性：

| 属性 | 文件类型                              |
| ---- | ------------------------------------- |
| -    | 普通文件。                            |
| d    | 目录。                                |
| l    | 符号链接。                            |
| c    | 字符设备文件。例如：终端、/dev/null。 |
| b    | 块设备文件。例如：硬盘、DVD。         |

文件目录权限：

| 权限属性 | 文件 | 目录 |
| -------- | ---- | ---- |
| r        |      |      |
| w        |      |      |
| x        |      |      |

