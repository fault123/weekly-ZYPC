---
title: Python-segno
date: 2023-10-15 20:57:24
tags: segno
mathjax: true
---

# Python-segno

## 安装

```bash
pip install segno
pip install qrcode-artistic
```

## 基本使用

```python
import segno
qr_code_message='...'
qr=segno.make(qr_code_message)
qr.save('*.png',scale=10)
"""
    可选参数：
        border 边框大小
        dark 深色部分颜色
        light 浅色部分颜色
        finder_dark finder部分颜色
"""
qr.save('*.png',dark='red',light='#562396',scale=10,border=10,finder_dark='yellow')
```

## 背景图像上创建

```python
qrcode=segno.make('...',error='h')
qrcode.to_artistic(background='*.gif',target='*.gif',scale=8)
```

