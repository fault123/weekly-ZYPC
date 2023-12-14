---
title: CvxPy整数规划笔记
date: 2023-11-15 15:57:10
tags: CvxPy
mathjax: true
---

# CvxPy整数规划笔记

## 安装

```base
pip install cvxpy
pip install cvxopt
```

## 实例：[HNCTF 2022 WEEK3]Help_Me!

01背包问题。

```python
import numpy,cvxpy,sys
sys.setrecursionlimit(1000000000) #设置最大递归深度
weight=[71,34,82,23,1,88,12,57,10,68,5,33,37,69,98,24,26,83,16,26] #每样东西的重量
value=[26,59,30,19,66,85,94,8,3,44,5,1,41,82,76,1,12,81,73,32] #每样东西的价值
index=numpy.array(weight)
mul_value=numpy.array(value)
x=cvxpy.Variable(20,integer=True) #20个物品，定义一个未知数向量
obj=cvxpy.Maximize(x@mul_value) #object目标：@是numpy中的矩阵乘法，目标为x点乘mul_value的值最大 Minimize
cons=[cvxpy.sum(x@index)<=200,x>=0,x<=1] #constraints约束条件：每个未知数只能0或1 不选和选 且x点乘index即总重不得超过200
prob=cvxpy.Problem(obj,cons)
prob.solve(solver='GLPK_MI')
print(prob.solve()) #输出得到的最大总价值
print(x.value) #未知量x向量的解
```

## 选课策略模型

| 课号 | 课名       | 学分 | 所属类别       | 先修课要求       |
| ---- | ---------- | ---- | -------------- | ---------------- |
| 1    | 微积分     | 5    | 数学           |                  |
| 2    | 线性代数   | 4    | 数学           |                  |
| 3    | 最优化方法 | 4    | 数学；运筹学   | 微积分；线性代数 |
| 4    | 数据结构   | 3    | 数学；计算机   | 计算机编程       |
| 5    | 应用统计   | 4    | 数学；运筹学   | 微积分；线性代数 |
| 6    | 计算机模拟 | 3    | 计算机；运筹学 | 计算机编程       |
| 7    | 计算机编程 | 2    | 计算机         |                  |
| 8    | 预测理论   | 2    | 运筹学         | 应用统计         |
| 9    | 数学实验   | 3    | 运筹学；计算机 | 微积分；线性代数 |

要求：至少选两门数学课、三门运筹学、两门计算机课。

决策变量：$\displaystyle x_i=\begin{cases}1:选修课号i的课程\\0:不选课号i的课程\end{cases}$

目标函数：$\displaystyle\min\left\{\sum_{i=1}^0x_i\right\}$

约束条件：

* 最少2门数学课，3们运筹学课，2门计算机课：
    $$
    \begin{cases}
    	x_1+x_2+x_3+x_4+x_5\geqslant2\\
    	x_3+x_5+x_6+x_8+x_9\geqslant3\\
    	x_4+x_6+x_7+x_9\geqslant2
    \end{cases}
    $$

* 先修课程要求：

    例如$2x_3-x_1-x_3\leqslant0$即为$\displaystyle x_3\leqslant\frac{x_1+x_3}{2}$。
    $$
    \begin{cases}
    	2x_3-x_1-x_2\leqslant0\\
    	x_4-x_7\leqslant0\\
    	2x_5-x_1-x_2\leqslant0\\
    	x_6-x_7\leqslant0\\
    	x_8-x_5\leqslant0\\
    	2x_9-x_1-x_2\leqslant0
    \end{cases}
    $$

```python
import numpy,cvxpy
x=cvxpy.Variable(9,integer=True)
c=numpy.array([5,4,4,3,4,3,2,2,3])
obj=cvxpy.Minimize(cvxpy.sum(x))
con=[x>=0,x<=1, 
     cvxpy.sum(x[0:5])>=2, 
     x[2]+x[4]+x[5]+x[7]+x[8]>=3, 
     x[3]+x[5]+x[6]+x[8]>=2, 
     2*x[2]-x[0]-x[1]<=0, 
     x[3]-x[6]<=0, 
     2*x[4]-x[0]-x[1]<=0, 
     x[5]-x[6]<=0, 
     x[7]-x[4]<=0, 
     2*x[2]-x[0]-x[1]<=0]
prob=cvxpy.Problem(obj,con)
prob.solve()
print(prob.value) #最优课程总数
print(x.value) #最优解
print(numpy.sum(x.value*c)) #总学分
```

目标改变：选修课程最小时，为了学分尽量多，应学习哪些课程？

目标函数：$\displaystyle\max\left\{\sum_{i=1}^9c_ix_i\right\}$

约束条件添加：$\displaystyle\sum_{i=1}^9x_i=6$

```python
import numpy,cvxpy
x=cvxpy.Variable(9,integer=True)
c=numpy.array([5,4,4,3,4,3,2,2,3])
obj=cvxpy.Maximize(c@x)
con=[x>=0,x<=1,
     cp.sum(x)==6,
     cvxpy.sum(x[0:5])>=2, 
     x[2]+x[4]+x[5]+x[7]+x[8]>=3, 
     x[3]+x[5]+x[6]+x[8]>=2, 
     2*x[2]-x[0]-x[1]<=0, 
     x[3]-x[6]<=0, 
     2*x[4]-x[0]-x[1]<=0, 
     x[5]-x[6]<=0, 
     x[7]-x[4]<=0, 
     2*x[2]-x[0]-x[1]<=0]
prob=cvxpy.Problem(obj,con)
prob.solve()
print(prob.value) #最优课程总数
print(x.value) #最优解
print(numpy.sum(x.value*c)) #总学分
```

## 装箱问题

有$7$种规格的包装箱要装到两辆铁路平板车上去。包装箱的宽和高是一样的，但厚度$l$（单位：厘米）及重量$w$（单位：千克）是不同的。每辆平板车有$10.2\textrm{m}$长的地方来装包装箱，载重量为$40\textrm{t}$。对$C_5,C_6,C_7$类包装箱的总数有一个特别的限制：这类箱子所占的空间（厚度）不能超过$302.7\textrm{cm}$。要求给出最好的装运方式。

|                 | $C_1$  | $C_2$  | $C_3$  | $C_4$  | $C_5$  | $C_6$  | $C_7$  |
| --------------- | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| $l/\textrm{cm}$ | $48.7$ | $52.0$ | $61.3$ | $72.0$ | $48.7$ | $52.0$ | $64.0$ |
| $w/\textrm{kg}$ | $2000$ | $3000$ | $1000$ | $500$  | $4000$ | $2000$ | $1000$ |
| 件数            | $8$    | $7$    | $9$    | $6$    | $6$    | $4$    | $8$    |

设决策变量$x_{ij}(i\in\{1,2\},j\in\{1,2,\cdots,7\})$表示第$i$辆车上装第$j$种包装箱的件数，$l_j,w_j,a_j(j\in\{1,2,\cdots,7\})$分别表示第$j$种包装箱的厚度、重量和件数。

### 体积上多装

约束条件：
$$
\begin{cases}
	\displaystyle\sum_{i=1}^2x_{ij}\leqslant a_j,&j\in\{1,2,\cdots,7\}\\
	\displaystyle\sum_{i=1}^7l_ix_{ij}\leqslant1020,&i\in\{1,2\}\\
	\displaystyle\sum_{i=1}^7w_jx_{ij}\leqslant40000,&i\in\{1,2\}\\
	\displaystyle\sum_{j=5}^7l_j(x_{1j}+x_{2j})\leqslant302.7,&x_{ij}\in\mathbb{N_+},i\in\{1,2\},j\in\{1,2,\cdots,7\}
\end{cases}
$$
目标函数：$\displaystyle\max\left\{\sum_{j=1}^7l_j(x_{1j}+x_{2j})\right\}$

```python
import cvxpy,numpy
L=numpy.array([48.7,52.0,61.3,72.0,48.7,52.0,64.0])	
w=numpy.array([2000,3000,1000,500,4000,2000,1000])
a=numpy.array([8,7,9,6,6,4,8])
x=cvxpy.Variable((2,7),integer=True)
obj=cvxpy.Maximize(cvxpy.sum(x@L))
con=[cvxpy.sum(x,axis=0,keepdims=True)<=a.reshape(1,7),
	 x@L<=1020,x@w<=40000,cvxpy.sum(x[:,4:]@L[4:])<=302.7,x>=0]
prob=cvxpy.Problem(obj, con)
prob.solve(solver='GLPK_MI',verbose=True)
print(prob.value) #最优值
print(x.value) #最优解
```

### 重量上多装

目标函数改为：$\displaystyle\max\left\{\sum_{j=1}^7w_j(x_{1j}+x_{2j})\right\}$

```python
import cvxpy,numpy
L=numpy.array([48.7,52.0,61.3,72.0,48.7,52.0,64.0])	
w=numpy.array([2000,3000,1000,500,4000,2000,1000])
a=numpy.array([8,7,9,6,6,4,8])
x=cvxpy.Variable((2,7),integer=True)
obj=cvxpy.Maximize(cvxpy.sum(x@w))
con=[cvxpy.sum(x,axis=0,keepdims=True)<=a.reshape(1,7),
	 x@L<=1020,x@w<=40000,cvxpy.sum(x[:,4:]@L[4:])<=302.7,x>=0]
prob=cvxpy.Problem(obj, con)
prob.solve(solver='GLPK_MI',verbose=True)
print(prob.value) #最优值
print(x.value) #最优解
```

## 非线性规划

### 问题一

$$
\min\left\{\dfrac{2+x_1}{1+x_2}-3x_1+4x_3\right\},x_i\in[0.1,0.9],i\in\{1,2,3\}
$$

```python
from scipy.optimize import minimize
from numpy import ones
def obj(x):
	x1,x2,x3=x
	return (2+x1)/(1+x2)-3*x1+4*x3
LB=[0.1]*3
UB=[0.9]*3
bound=tuple(zip(LB, UB)) #生成决策向量界限的元组
res=minimize(obj,ones(3),bounds=bound) #第2个参数为初值
print(res.fun,'\n',res.success,'\n',res.x) #输出最优值、求解状态、最优解
```

### 问题二

$$
\max\left\{x_1^2+x_2^2+3x_3^3+4x_4^2+2x_5^2-8x_1-2x_2-3x_3-x_4-2x_5\right\},\\
\begin{cases}
	x_1+x_2+x_3+x_4+x_5\leqslant400,\\
	x_1+2x_2+2x_3+x_4+6x_5\leqslant800,\\
	2x_1+x_2+6x_3\leqslant200,\\
	x_3+x_4+5x_5\leqslant200\\
\end{cases},x_i\in[0,99],i\in\{1,2,\cdots,5\}
$$

```python
from scipy.optimize import minimize
import numpy
c1=numpy.array([1,1,3,4,2])
c2=numpy.array([-8,-2,-3,-1,-2])	
A=numpy.array([[1,1,1,1,1],[1,2,2,1,6],
	           [2,1,6,0,0],[0,0,1,1,5]])
b=numpy.array([400,800,200,200])
obj=lambda x:numpy.dot(-c1,x**2)+numpy.dot(-c2,x)
cons={'type':'ineq','fun':lambda x:b-A@x}
bd=[(0,99)for i in range(A.shape[1])]
res=minimize(obj,numpy.ones(5)*90,constraints=cons,bounds=bd)
print(res)
```

