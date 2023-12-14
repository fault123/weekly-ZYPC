---
title: Yara使用入门
date: 2023-11-15 14:23:11
tags: Yara
mathjax: true
---

# Yara使用入门

## 安装

Github上下载yara和yara-rules，放在同一目录下，配置环境变量。

index.yar引用了所有规则，使用时例如：

```bash
yara index.yar 1.php
```

常用参数：

-w关闭警告信息，-m设置输出meta信息，-s设置输出匹配字符串，-g设置输入标签信息。

```bash
yara -w -msg index.yar *.*
```

