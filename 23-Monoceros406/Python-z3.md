---
title: Python-z3
date: 2023-10-15 20:58:33
tags: z3
mathjax: true
---

# Python-z3

## 安装

```bash
pip install z3_solver
```

## 使用

```python
from z3 import *
```

## 整形

```python
n=Int('n')
a,s,d=Ints('a s d')
x=Solver()
x.add(a-d==18)
x.add(a+s==12)
x.add(s-d==20)
#...
check=x.check()
print(check)#sat有解 unsat无解
model=x.model()
print(model)#输出解
```

## 有理数

```python
m=Real(m)
x,y=Reals('x y')
s=Solver()
s.add(x**2+y**2==3)
s.add(x**3==2)
check=s.check()
print(check)
model=s.model()
print(model)
```

## 二进制位运算

```python
m=BitVec('m',8)#8bit=1Byte
x,y,z=BitVecs('x y z',8)
s=Solver()
s.add(x^y&z==12)
s.add(y&z>>3==3)
s.add(z^y==4)
check=s.check()
print(check)
model=s.model()
print(model)
```

