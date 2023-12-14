---
title: Python-pyenv
date: 2023-10-15 20:55:00
tags: pyenv
mathjax: true
---

# Python-pyenv

**这玩意巨难使，建议不要尝试**

## 安装

从Github上下载源码
新建系统环境变量：`PYENV` `PYENV_ROOT` `PYENV_HOME` 均为`.../pyenv-win`
添加用户环境变量：`.../pyenv-win/shims`和` .../pyenv-win/bin`
打开`pyenv-win\.versions-cache.xml` 把`https://www.python.org/ftp/python`替换为`https://registry.npmmirror.com/-/binary/python`
打开`pyenv-win\libexec\libs\pyenv-install-lib.vbs` 替换操作同上

## 命令

```bash
#查看pyenv版本，测试安装成功：
pyenv --version
#查看所有可安装的版本：
pyenv install --list
#列出已安装的所有版本：
pyenv versions
#安装特定版本：
pyenv install 3.7.10
#卸载特定版本：
pyenv uninstall 3.7.10
#创建shims，每次更新、pip后都要执行：
pyenv rehash
#全局默认版本：
pyenv global 3.7.10
#当前目录默认版本：
pyenv local 3.7.10
#当前shell会话临时版本（推荐）：
pyenv shell 3.7.10
```

