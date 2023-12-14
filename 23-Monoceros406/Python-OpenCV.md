---
title: Python-OpenCV
date: 2023-10-15 21:00:20
tags: OpenCV
mathjax: true
---

# Python-OpenCV

## 安装

```bash
pip install opencv-contrib-python
```

## 基本操作

### 读取+显示图像

```python
import cv2
image=cv2.imread('*.jpg',cv2.IMREAD_UNCHANGED)
"""
    image=cv2.imread(filename,flags)
        filename 文件名
        flags 默认1为彩色 0为灰度图像
        cv2.IMREAD_UNCHANGED 保持原格式 可选
"""
cv2.imshow("flower",image)
"""
    cv2.imshow(winname,mat)
        winname 窗口名称
        mat 图像
"""
cv2.waitKey()
"""
    retval=cv2.waitKey(delay)
        retval 按下按键的ASCII值 如Esc为27 没有按下返回-1
        delay 等待用户按下按键的时间 单位ms 当delay<=0或为None时无限等待
"""
cv2.destroyAllWindows() #销毁所有正在显示图像的窗口
```

### 保存图像

```python
cv2.imwrite('*.jpg',image)
```

### 获取属性

```python
print(image.shape) #（像素行数，像素列数，通道数）
print(image.size) #像素个数
print(image.dtype) #数据类型
```

## 数字化基础

### 获取+修改像素BGR

```python
px=image[x,y]
print(px) #[B,G,R]
px=[255,255,255]
```

### RGB/BGR转GRAY/HSV色彩空间

```python
image2=cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)
"""
    dst=cv2.cvtColor(src,code)
        code 色彩空间转换码
            cv2.COLOR_BGR2GRAY
            cv2.COLOR_RGB2GRAY
            cv2.COLOR_BGR2HSV
            cv2.COLOR_RGB2HSV
"""
```

### 通道

```python
#拆分通道
b,g,r=cv2.split(bgr_image)
r,g,b,a=cv2.split(bgra_image)
h,s,v=cv2.split(hsv_image)
#调整通道
h[:,:]=180 #将H通道调整为180
#合并通道
rgb=cv2.merge([b,g,r]) #顺序不可反
hsv=cv2.merge([h,s,v])
```

### NumPy数组类型

```python
import numpy
"""
    bool_
    int_
    intc
    intp
    int8
    int16
    int32
    int64
    uint8
    uint16
    uint32
    uint64
    float
    float16
    float32
    float64
    complex_
    complex64
    complex128
    datatime64
    timedelta64
"""
```

### 数组转换方法

```python
numpy.int8(3.141)#3
numpy.float64(8)#8.0
numpy.float(True)#1.0
```

### 创建数组

```python
n=numpy.array([1,2,3])
n=numpy.array([0.1,0.2,0.3])
n=numpy.array([1,2],[3,4])
n=numpy.array([1,2,3],dtype=numpy.float_)
print(n.dtype) #float64
n=numpy.array([1,2,3],dtype=float)
n=numpy.array([1,2,3],ndmin=3) #创建3维数组
print(n)#[[[1 2 3]]]
"""
    numpy.array(object,dtype,copy,order,subok,ndmin)
    object 具有数组接口方法的对象
    dtype 数据类型
    copy 默认True object被复制；否则只有当__array__返回副本，object为嵌套序列，或需要副本满足数据类型和顺序要求时，才会生成副本
    order 元素在内存中出现的顺序
        object不是数组
            C按行排列 F按列排序
        object是数组
            C、F、A原顺序、K元素在内存中出现的顺序
    sudok 默认False返回的数组将强制为基类数组 True则传递子类
    ndmin 指定生成数组的最小维数
"""
n=numpy.empty([2,3]) #创建2行3列未初始化数组
n=numpy.zeros((3,3),numpy.uint8) #创建3行3列纯0数组
n=numpy.ones((3,3),numpy.uint8) #创建3行3列纯1数组
```

### 创建随机数组

```python
n=numpy.random.randint(1,3,10) #生成1*10随机数组 1~3
n=numpy.random.randint(5,10) #生成1个5~10随机数
n=numpy.random.randint(5,size=(2,5)) #生成2*5随机数组 <=5
```

### 数组运算

```python
n1=numpy.array([1,2])
n2=numpy.array([3,4])
print(n1+n2)
print(n1-n2)
print(n1*n2)
print(n1/n2)
print(n1**n2)
print(n1>=n2)
print(n1==n2)
print(n1<=n2)
print(n1!=n2)
```

### 图像拼接

```python
#水平拼接
a=numpy.array([1,2,3])
b=numpy.array([4,5,6])
c=numpy.array([7,8,9])
result=numpy.hstack((a,b,c))
#垂直拼接
result=numpy.hstack((a,b,c))
```

## 绘制

### 线段绘制

```python
canvas=numpy.zeros((300,300,3),numpy.uint8)
canvas=cv2.line(canvas,(50,50),(250,50),(255,0,0),5)
#起点坐标(50,50) 终点坐标为(250,50) 蓝色 线条宽度为5
```

### 矩形绘制

```python
canvas=cv2.rectangle(canvas,(50,50),(200,150),(255,255,0),20)
#左上角坐标(50,50) 右下角坐标(200,150) 青色 线条宽度20
canvas=cv2.rectangle(canvas,(50,50),(200,150),(255,255,0),-1)
#实心
```

### 绘制圆形

```python
canvas=cv2.circle(canvas,(50,50),40,(0,0,255),-1)
#圆心坐标(50,50) 半径40 红色实心圆形
```

### 绘制多边形

```python
pts=numpy.array([[100,50],[200,50],[250,250],[50,250]],numpy.int32)
canvas=cv2.polylines(canvas,[pts],True,(0,0,255),5)
#根据4个点坐标 闭合True不封闭False 红色 线条宽度5
```

### 绘制文字

```python
"""
    img=cv2.putText(img,text,org,fontFace,fontScale,color,thickness,lineType,bottomLeftOrigin)
    org 左下角坐标
    fontsFace 字体样式
        FONT_HERSHEY_SIMPLEX 正常大小sans-serif字体
        FONT_HERSHEY_PLAIN 小号sans-serif字体
        FONT_HERSHEY_BUPLEX 正常大小、更复杂sans-serif字体
        FONT_HERSHEY_COMPLEX 正常大小serif字体
        FONT_HERSHEY_TRIPLEX 正常大小、更复杂serif字体
        FONT_HERSHEY_COMPLEX_SMALL 更简单serif字体
        FONT_HERSHEY_SCRIPT_SIMPLE 手写字体
        FONT_HERSHEY_SCRIPT_COMPLEX 进阶手写字体
        FONT_ITALIC 斜体
    fontScale 字体大小
    color
    thickness
    lineType 线形 4或8（默认）
    bottomLeftOrigin 绘制文字方向 True或False（默认）
"""
fontStyle=cv2.FONT_HERSHEY_TRIPLEX+cv2.FONT_ITALIC
cv2.putText(canvas,"...",(20,70),fontStyle,2,(0,255,0),5)
#垂直镜像效果
cv2.putText(canvas,"...",(20,70),fontStyle,2,(0,255,0),5,8,True)
```

## 阈值

### 阈值处理

```
retval,dst=cv2.threshold(src,thresh,maxval,type)
retval 采用的阈值
dst 处理后图像
src 被处理的图像
thres 阈值 建议125~150
maxval 采用的最大值
type 处理类型
    cv2.THRESH_BINARY 二值化
    cv2.THRESH_BINARY_INV 反二值化
    cv2.THRESH_TOZERO 低于阈值零处理
    cv2.THRESH_TOZERO_INV 超出阈值零处理
    cv2.THRESH_TRUNC 截断阈值
    Otsu方法:
        cv2.THRESH_OTSU
```

### 二值化

```
if 像素值<=阈值:
    像素值=0
if 像素值>阈值:
    像素值=最大值
```

```python
t1,dst=cv2.threshold(img,127,255,cv2.THRESH_BINARY)
```

### 反二值化

```
if 像素值<=阈值:
    像素值=最大值
if 像素值>阈值:
    像素值=0
```

### 零处理

```python
if 像素值<=阈值:
    像素值=0
if 像素值>阈值:
    像素值=原值
```

### 超出阈值零处理

```
if 像素值<=阈值:
    像素值=原值
if 像素值>阈值:
    像素值=0
```

### 截断处理

```
if 像素<=阈值:
    像素=原值
if 像素>阈值:
    像素=阈值
```

### 自适应处理

```
dst=cv2.adaptiveThreshold(src,maxValue,adaptiveMethod,thresholdType,blocksize,C)
src 须灰度图像
maxValue 阈值采用最大值
adaptiveMethod 自适应阈值计算方法
    cv2.ADAPTIVE_THRESH_MEAN_C 对正方形区域内像素平均加权
    cv2.ADAPTIVE_THRESH_GAUSSIAN_C 根据高斯函数按照像素与中心点距离对正方形区域内所有像素加权计算
blockSize 正方形区域大小
C 阈值等于均值或加权值减去这个常量
```

## 几何变换

### 缩放

```python
"""
    dst=cv2.resize(src,dsize,fx,fy,interpolation)
    dsize (宽,高) 单位px
    fx 可选 水平缩放比例
    fy 可选 竖直缩放比例
    interpolation 可选 缩放插值方式 建议默认
"""
dst=cv2.resize(img,(100,100))
dst=cv2.resize(img,None,fx=1/3,fy=1/2)
```

### 翻转

```python
dst=cv2.flip(img,0) #x轴翻转
dst=cv2.flip(img,1) #y轴翻转
dst=cv2.flip(img,-1) #同时xy轴翻转
```

### 仿射变换

```python
dst=cv2.warpAffine(src,M,dsize,flags,borderMode,borderValue)
"""
    M 2*3举止
    dsize 尺寸
    flags 可选插值方式 建议默认
    borderMode 可选边界类型 建议默认
    borderValue 可选边界值 建议默认9
    
    M 放射矩阵
    M=[[a,b,c],
       [d,e,f]]
    x'=xa+yb+c
    y'=xd+ye+f
"""

#M矩阵创建方式
M=numpy.zeros((2,3),numpy.float32) #全是0
M=numpy.float32([[1,2,3],[4,5,6]]) #赋予具体值

#平移
M=[[1,0,水平移动距离],[0,1,垂直移动距离]]

#旋转
"""
    M=c2.getRotationMatrix2D(center,angle,scale)
    center 旋转中心点坐标
    angle 旋转角度 正数逆时针 复数顺时针
    scale 缩放比例 
"""

#倾斜
p1=numpy.zeros((3,2),numpy.float32)
p2=numpy.zeros((3,2),numpy.float32)
p1[0]=[x,y] #左上角
p1[1]=[x,y] #右上角
p1[2]=[x,y] #左下角
#...
M=cv2.getAffineTransform(p1,p2)

#透射
p1=numpy.zeros((3,2),numpy.float32)
p2=numpy.zeros((3,2),numpy.float32)
p1[0]=[x,y] #左上角
p1[1]=[x,y] #右上角
p1[2]=[x,y] #左下角
p1[2]=[x,y] #右下角
...
M=cv2.getPerspectiveTransform(p1,p2)
```

## 图像运算（掩模）

```python
#加法
sum=cv2.add(img1,img2) #当超过255时取255
#位运算
#按位与
dst=cv2.btiwise_and(src1,src2,mask)
    #mask 掩模参数 可选
#按位或
dst=cv2.bitwise_or(src1,src2,mask)
#按位取反
dst=cv2.bitwise_not(src,mask)
#按位异或
dst-cv2.bitwise_xor(src,mask)
#加权和
dst=cv2.addWeighted(src1,alpha,src2,beta,gamma)
"""
    src1 src2 第一、二幅图片
    alpha beta 第一、二幅图片的权重
    gamma 在结果上添加的标量 值越大越亮 可以复数
    dst 叠加后的图像
"""
#图像覆盖
img=overlay_img(src1,src2,95,90) #将src2覆盖到scr1上
```

## 滤波器

### 均值滤波器

```python
dst=cv2.blur(src,ksize,anchor,borderType)
"""
    src 被处理图像
    ksize 滤波核大小 (高度,宽度) 建议等宽高相等的奇数边长 滤波核越大越模糊
    anchor 滤波核锚点 可选 建议默认
    borderType 边界样式 可选 建议默认
    dst 处理后图像
"""
```

### 中值滤波器

```python
dst=cv2.medianBlur(src,ksize)
"""
    ksize 滤波核边长 必须大于1奇数 例如3、5、7等
"""
```

### 高斯滤波器

```python
dst=cv2.GaussianBlur(src,ksize,sigmaX,sigmaY,borderType)
"""
    ksize 滤波核大小 宽、高必须奇数
    sigmaX sigmaY 卷积核水平、垂直方向标准差 建议0自动
    borderType 边界样式 可选 建议默认
"""
```

### 双边滤波器

```python
dst=cv2.bilateralFilter(src,d,sigmaColor,sigmaSpace,borderType)
"""
    用于保留更多的边界信息
    d 以当前像素为中心的整个滤波区域的直径 当d<0时自动根据sigmaSpace计算得 该值与保留得边缘信息成正比 与方法运行效率成反比
    sigmaColor 参与计算的颜色范围 即像素颜色值与周围颜色值的最大差值 只有小于这个值才参与运算 255时所有颜色都参与
    sigmaSpace 坐标空间sigma值 值越大参与计算像素数越多
"""
dst=cv2.bilateralFilter(img,15,120,100)
```

## 腐蚀&膨胀

### 腐蚀

```python
dst=cv2.erode(src,kernel,anchor,iterations,borderType,borderValue)
"""
    抹除边界细节
    kernel 腐蚀用的核
    annchor 核的锚点位置 可选
    iterations 迭代次数 默认1 可选
    borderValue 边界值 建议默认 可选
"""
k=numpy.ones((3,3),numpy.uint8)
dst=cv2.erode(img,k)
```

### 膨胀

```python
dst=cv2.dilate(src,kernel,anchor,iterations,borderType,borderValue)
"""
    近视眼特效
    同上
"""
k=numpy.ones((9,9),numpy.uint8)
dst=cv2.dilate(img,k)
```

### 开运算

先腐蚀后膨胀，用于抹除外部细节或噪声。

### 闭运算

先膨胀后腐蚀，用于抹除内部细节或噪声。

### 形态学方法

```python
dst=cv2.morphologyEx(src,op,kernel,anchor,iterations,borderType,borderValue)
"""
    op 操作类型
        cv2.MORPH_ERODE 腐蚀
        cv2.MORPH_DILATE 膨胀
        cv2.MORPH_OPEN 开运算
        cv2.MORPH_CLOSE 闭运算
        cv2.MORPH_GRADIENT 梯度运算 膨胀图-腐蚀图=简易的轮廓
        cv2.MORPH_TOPHAT 顶帽运算 原图像-开运算图=外部细节
        cv2.MORPH_BALCKHAT 黑帽运算 闭运算图-原图像=内部细节
"""
```

## 图像检测

### 图像轮廓

```python
contours,hierarchy=cv2.findContours(iamge,mode,methode)
"""
    image 被检测的图像 必须8位单通道二值图像 原始图像->灰度图像->二值化阈值处理
    mode 轮廓检索模式
        cv2.RETR_EXTERNAL 只检测外轮廓
        cv2.RETR_LIST 检测所有轮廓 不建立层次关系
        cv2.RETR_CCOMP 检测所有轮廓 建立两级层次关系
        cv2.RETR_TREE 检测所有轮廓 建立树状结构层次关系
    methode 检测轮廓方法
        cv2.CHAIN_APPROX_NONE 储存轮廓上所有点
        cv2.CHAIN_APPROX_SIMPLE 只保存水平、垂直或对角线轮廓端点
        cv2.CHAIN_APPROX_TC89_L1 Ten-Chinl近似算法中一种
        cv2.CHAIN_APPROX_TC89_KCOS 同上
    contours 检测的所有轮廓 list 每个元素是某个轮廓像素坐标数组
    hierarchy 轮廓之间层次关系
"""
```

### 轮廓绘制

```python
image=cv2.drawContours(image,contours,controuIdx,color,thickness,lineTypee,hierarchy,maxLevel,offse)
"""
    image 原始图像 可多通道
    contours findContours()得到的轮廓列表
    contourIdx 绘制轮廓的索引 -1则绘制所有轮廓
    color 绘制颜色 BGR
    thickness 可选 画笔粗细程度 -1绘制实心轮廓
    lineTypee 可选 绘制轮廓线形
    hierarchy 可选 findContours()得出的层次关系
    maxLevel 绘制轮廓最深层次深度
    offse 可选参数 偏移量 改变绘制结果位置
"""
```

### 实例：绘制几何图形轮廓

```python
gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
t,binary=cv2.threshold(gray,127,255,cv2.THRESH_BINARY)
contours,hierarchy=cv2.findContours(binary,cv2.RETR_LIST,cv2.CHAIN_APPROX_NONE)
    #换为cv2.RETR_EXTERNAL只绘制外轮廓
cv2.drawContours(img,contours,-1,(0,0,255),5)
cv2.imshow("img",img)
```

### 轮廓拟合

#### 矩形包围框

```python
x,y,w,h=cv2.boundingRect(array)
"""
    array 轮廓数组
    x,y,w,h 左上角横坐标、纵坐标、矩形宽、高
"""
img=cv2.imread("*.png")
gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
t,binary=cv2.threshold(gray,127,255,cv2.THRESH_BINARY)
contours,hierarchy=cv2.findContours(binary,cv2.RETR_LIST,cv2.CHAIN_APPROX_SIMPLE)
x,y,w,h=cv2.boundingRect(contours[0])
cv2.rectangle(img,(x,y),(x+w,y+h),(0,0,255),2)
cv2.imshow("img",img)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### 圆形包围框

```python
center,radius=cv2.minEnclosingCircle(points)
"""
    point 轮廓数组
    center 圆心坐标  float(横坐标,纵坐标)
    radius float 半径
"""
img=cv2.imread("*.png")
gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
t,binary=cv2.threshold(gray,127,255,cv2.THRESH_BINARY)
contours,hierarchy=cv2.findContours(binary,cv2.RETR_LIST,cv2.CHAIN_APPROX_SIMPLE)
center,radius=cv2.minEnclosingCircle(contours[0])
x=int(round(center[0]))
y=int(round(center[1]))
cv2.circle(img,(x,y),int(radius),(0,0,255),2)
cv2.imshow("img",img)
```

### 凸包

```python
hull=cv2.convexHull(points,clockwise,returnPoints)
"""
    points 轮廓数组
    clockwise 可选 True点顺时针 False逆时针
    returnPoints 可选 True返回点坐标 False返回点索引
    hull 凸包点阵数组
"""
img=cv2.imread("*.png")
gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
ret,binary=cv2.threshold(gray,127,255,cv2.THRESH_BINARY)
contours,hierarchy=cv2.findContours(binary,cv2.RETR_LIST,cv2.CHAIN_APPROX_SIMPLE)
hull=cv2.convexHull(contours[0])
cv2.polylines(img,[hull],True,(0,0,255),2)
cv2.imgshow("img",img)
cv2.waitKey()
cv2.destroyAllWindows()
```

### Canny边缘检测

```python
edge=cv2.Canny(image,threshold1,threshold2,apertureSize,L2gradient)
"""
    image 原始图像
    threshold1 通常最小阈值
    threshold2 通常最大阈值
    apertureSize 可选 Sobel算子孔径大小
    L2gradient 可选 默认False True更精细
    edge 边缘图像（二值灰度图像）

    两个阈值都小时检测出较多细节，都较大时忽略更多细节
"""
img=cv2.imread("flower.png")
r1=cv2.Canny(img,10,50)
r2=cv2.Canny(img,100,200)
r3=cv2.Canny(img,400,600)
cv2.imshow("img",img)
cv2.imshow("r1",r1)
cv2.imshow("r2",r2)
cv2.imshow("r3",r3)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 霍夫变换

#### 直线检测

```python
lines=cv2.HoughLinesP(image,rho,theta,threshold,minLineLength,maxLineGap)
"""
    image 原始图像
    rho 半径步长 为1时检测所有可能得半径步长
    theta 搜索直线的角度 为pi/180时检测所有角度
    threshold 阈值 越小检测出直线越多
    minLineLength 线段最小长度 小于该值不会被记录
    maxLineGap 线段之间最小距离
    lines 检测出的线段 格式：[[[x1,y1,x2,y2],[x1,y1,x2,y2]]]
"""
img=cv2.imread("*.png")
o=img.copy()
o=cv2.medianBlur(o,5)
gray=cv2.cvtColor(o,cv2.COLOR_BGR2GRAY)
binary=cv2.Canny(o,50,150)
lines=cv2.HoughLinesP(binary,1,numpy.pi/180,15,minLineLength=100,maxLineGap=18)
for line in lines:
    x1,y1,x2,y2=line[0]
    cv2.line(img,(x1,y1),(x2,y2),(0,0,255),2)
cv2.imshow("canny",binary)
cv2.imshow("img",img)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### 圆环检测

```python
circles=cv2.HoughCircles(image,method,dp,minDist,param1,param2,minRadius,maxRadius)
"""
    image 原始图像
    method 检测方法
        cv2.HOUGH_GRADIENT
    dp 累加器分辨率与原始图像分辨率之比的倒数 值为1时累加器与原始图像具有相同的分辨率（通常） 值为2时累加器分辨率为原始图像的1/2
    minDist 圆心之间最小距离
    param1 可选 Canny边缘检测使用的最大阈值
    param2 可选 检测圆环结果
    minRadius 可选 园的最小半径
    maxRadius 可选 圆的最大半径
    circles 数组 所有检测出的圆 格式：[[[x1,y1,r1],[x2,y2,r2]]]
"""
img=cv2.imread("*.jpg")
o=img.copy()
o=cv2.medianBlur(o,5)
gray=cv2.cvtColor(o,cv2.COLOR_BGR2GRAY)
circles=cv2.HoughCircles(gray,cv2.HOUGH_GRADIENT,1,70,param1=100,param2=25,minRadius=10,maxRadius=50)
circles=numpy.uint(numpy.around(circles))
for c in circles[0]:
    x,y,r=c
    cv2.circle(img,(x,y),r,(0,0,255),3)
    cv2.circle(img,(x,y),2,(0,0,255),3)
cv2.imshow("img",img)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 模板匹配

### 模板匹配方法

```python
result=cv2.matchTemplate(image,templ,method,mask)
"""
    image 原始图像
    templ 模板图像 尺寸小于等于原始图像
    method 匹配方法
        cv2.TM_SQDIFF 平方差匹配 匹配程度越高越小 完全匹配0
        cv2.TM_SQDIFF_NORMED 标准平方差匹配 同上
        cv2.TM_CCORR 相关匹配 匹配程度越高计算结果越大
        cv2.TM_CCORR_NORMED 标准相关匹配 同上
        cv2.TM_CCOEFF 相关系数匹配 -1~1的浮点数 1完全匹配 0毫无关系 -1两张图片刚好亮度相反
        cv2.TM_CCOEFF_NORMED 标准相关系数匹配 同上
    mask 可选 掩模 建议默认
        cv2.TM_SQDIFF
        cv2.TM_CCORR_NORMED
    result 一个W-w+1列、H-h+1行浮点型数组（原始图像宽高W、H，模板图像宽高w、h） 每个对应像素位置匹配结果
"""
```

### 单模板匹配

#### 绘制红框

```python
minValue,maxValue,minLoc,maxLoc=cv2.minMaxLoc(src,mask)
"""
    src matchTemple()方法计算得出的数组
    mask 可选 掩模 建议默认
    minValue 数组中最小值
    maxValue 数组中最大值
    minLoc 最小值坐标 格式(x,y)
    maxLoc 最大值坐标 格式同上
"""
img=cv2.imread("background.jpg")
temp1=cv2.imread("template.png")
height,width,c=temp1.shape
results=cv2.matchTemplate(img,temp1,cv2.TM_SQDIFF_NORMED)
minValue,maxValue,minLoc,maxLoc=cv2.minMaxLoc(results)
resultPoint1=minLoc
resultPoint2=(resultPoint1[0]+width,resultPoint[1]+height)
cv2.rectangle(img,resultPoint1,resultPoint2,(0,0,255),2)
cv2.imshow("img",img)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### 两幅中最佳匹配

```python
image=[]
image.append(cv2.imread("*.png"))
image.append(cv2.imread("*.png"))
temp1=cv2.imread(cv2.imread("temp1.png"))
index=-1
min=1
for i in range(0,len(image)):
    results=cv2.matchTemplate(image[i],temp1,cv2.TM_SQDIFF_NORMED)
    if min>any(results[0]):
        index=i
cv2.imshow("result",image[index])
cv2.waitKey()
cv2.destroyAllWindows()
```

## 视频处理

### 摄像头

#### VideoCapture基础操作

```python
capture=cv2.VideoCapture(index)
"""
    capture 要打开的摄像头
    index 设备索引
        0为笔记本内置摄像头 1为外置摄像头
"""
retval=cv2.VideoCapture.isOpened()
"""
    retval True初始化成功 否则False
"""
retval,image=cv2.VideoCapture.read()
"""
    retval 是否读取到帧 读到True否则False
"""
cv2.VideoCapture.release() #关闭摄像头
```

#### 使用VideoCapture

```python
capture=cv2.VideoCapture(0,cv2.CAP_DSHOW)
while(capture.isOpened()):
    retval,image=capture.read()
    cv2.imshow("Video",image)
    key=cv2.waitKey(1)
    if key==32:
        break
cv2.imwrite("*.png",image)
capture.release()
cv2.destroyAllWindows()
```

### 视频

#### 播放视频

```python
video=cv2.VideoCapture("*.avi")
while(video.isOpened()):
    retval,image=video.read()
    cv2.namedWindow("Video",0)
    cv2.resizeWindow("Video",420,300)
    if retval==True:
        cv2.imshow("Video",image)
    else:
        break
    key=cv2.waitKey(1)
    if key==27:
        break
video,release()
cv2.destroyAllWindows()
```

#### 获取属性

```python
retval=cv2.VideoCapture.get(propId)
"""
    retval 获取对应的属性值
    propId
        cv2.CAP_PRRP_POS_MSEC 播放时当前位置 单位ms
        cv2.CAP_PROP_POS_FRAMES 帧的索引 从0开始
        cv2.CAP_PROP_POS_AVI_RATIO 视频文件相对位置 0开始播放 1结束播放
        cv2.CAP_PROP_FRAME_WIDTH 帧宽
        cv2.CAP_PROP_FRAME_HEIGHT 帧高
        cv2.CAP_PROP_FPS 帧速率
        cv2.CAP_PROP_FOURCC 4字符表示视频编码格式
        cv2.CAP_PROP_FRAME_COUNT 帧数
        cv2.CAP_PROP_FORMAT retrieve()方法返回的Mat对象格式
        cv2.CAP_PROP_MODE 当前捕获模式后端专用的值
        cv2.CAP_PROP_CONVERT_RGB 指示是否将图转换为RGB
"""
```

#### 保存视频文件

```python
output=cv2.VideoWriter(filename,fourcc,fps,frameSize)
"""
    filename
    fourcc 4字符表示视频编码格式
    fps 帧速率
    frameSize 每一帧大小
"""
fourcc=cv2.VideoWriter_fourcc('x','x','x','x')
fourcc=cv2.VideoWriter_fourcc(*'xxxx') #同上
"""
    I420 .avi 未压缩YUV颜色编码格式
    PIMI .avi MPEG-1
    XVID .avi MPEG-4
    THEO .org OggVorbis
    FLVI .flv Flash
"""
capture=cv2.VideoCapture(0,cv2.CAP_DSHOW)
fourcc=cv2.VideoWriter_fourcc('X','V','I','D')
output=cvv2.VideoWriter("*.avi",fourcc,20,(640,480))
while (capture.isOpened()):
    retval,frame=capture.read()
    if retval==True:
        output.write(frame)
        cv2.imshow("frame",frame)
    key=cv2.waitKey(1)
    if key==27:
        break
capture.release()
output.release()
cv2.destroyAllWindows()
```

## 人脸检测

```python
"""
    级联分类器XML功能
    haarcascade_eye.xml 眼睛检测
    haarcascade_eye_tree_eyeglasses.xml 眼镜检测
    haarcascade_frontalcatface.xml 正面猫脸检测
    haarcascade_frontalface_default.xml 正面人脸检测
    haarcascade_fullbody.xml 身形检测
    haarcascade_lefteye_2splits.xml 左眼检测
    haarcascade_lowerbody.xml 下半身检测
    haarcascade_profileface.xml 侧面人脸检测
    haarcascade_righteye_2splits.xml 右眼检测
    haarcascade_russian_plate_number.xml 车牌检测
    haarcascade_smile.xml 笑容检测
    haarcascade_upperbody.xml 上半身检测
"""
cascade=cv2.CascadeClassifier("*.xml")
objects=cascade.detectMultiScale(image,scaleFactor,minNeighbor,flags,minSize,maxSize)
"""
    scaleFactor 可选 扫描图像时的缩放比例
    minNeighbors 可选 每个候选区至少保留多少个检测结果才可以判定为人脸 越大误差越小
    flags 可选 建议默认
    minSize 可选 最小目标尺寸
    maxSize 可选 最大目标尺寸
    objects [左上横,左上纵,区域宽,区域高] 格式[[,,,],[,,,]]
"""
img=cv2.imread("*.png")
faceCascade=cv2.CascadeClassifier("*.xml")
faces=faceCascade.detectMultiScale(img,1.15)
for(x,y,w,h)in faces:
    cv2.rectangle(img,(x,y),(x+w,y+h),(0,0,255),5)
cv2.imshow("img",img)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 图像覆盖

```python
def overlay_img(img,img_over,img_over_x,img_over_y):
    img_w,img_p=img.shape
    img_over_h,img_over_w,img_over_c=img_over.shape
    if img_over_c==3:
        img_over=cv2.cvtColor(img_over,cv2.COLOR_BGR2BGRA)
    for w in range(0,img_over_w):
        for h in range(0,img_over_h):
            if img_over[h,w,3]!=0:
                for c in range(0,3):
                    x=img_over_x+w
                    y=img_over_y+h
                    if x>=img_w or y>=img_h:
                        break
                    img[y,x,c]=img_over[h,w,c]
    return img
```

## 人脸识别

### Eigenfaces人脸识别器

```python
recognizer=cv2.face.EigenFaceRecognizer_create(num_components,threshold)
"""
    num_components PCA方法保留分量个数 建议默认
    thresh 可选 人脸识别阈值 建议默认
"""
recognizer.train(src,labels)
"""
    src 用来训练的人脸图片样本列表 格式list 必须宽高一致
    labels 样本对应标签 格式整数数组 长度与样本列表长度相同 与样本插入顺序一一对应
"""
label,confidence=recognizer.predict(src)
"""
    src 需要识别的人脸图像 宽高必须与样本一致
    label 匹配程度最高的标签值
    confidence 匹配最高的信用度评分 <5000匹配程度高 0完全一样
"""
photos=list()
lables=list()
photos.append(cv2.imread("face\\summer1.png",0))
lables.append(0)
photos.append(cv2.imread("face\\summer2.png",0))
lables.append(0)
photos.append(cv2.imread("face\\summer3.png",0))
lables.append(0)
photos.append(cv2.imread("face\\Elvis1.png",0))
lables.append(1)
photos.append(cv2.imread("face\\Elvis2.png",0))
lables.append(1)
photos.append(cv2.imread("face\\Elvis3.png",0))
lables.append(1)
names={"0":"Summer","1":"Elvis"}
recognizer=cv2.face.EigenFaceRecognizer_create()
recognizer.train(photos,numpy.array(lables))
i=cv2.imread("face\\summer4.png",0)
label,confidence=recognizer.predict(i)
print(str(confidence))
print(names[str(label)])
```

### Fisherfaces人脸识别器

```python
recognizer=cv2.face.FisherFaceRecognizer_create(num_components,threshold)
"""
    num_components 判断分析时保留的分量个数 建议默认
    threshold 阈值 建议默认
"""
recognizer.train(src,labels) #同上
label,confidence=recognizer.predict(src) #同上
```

### LBPH（局部二值模式直方图）人脸识别器

```python
recognizer=cv2.face.LBPHFaceRecognizer_create(radius,neighbors,grid_x,grid_y,threshold)
"""
    radius 可选 建议默认 局部二进制模式半径
    neighbors 可选 建议默认 局部二进制模式采样点数目
    grid_x 可选 建议默认 水平方向单元格数
    grid_y 可选 建议默认 垂直方向上单元格数
    threshold 可选 阈值 建议默认
"""
recognizer.train(src,labels) #同上
label,confidence=recognizer.predict(src) #confidence<50匹配程度高 0完全一样
```

