---
title: Python-Pandas
date: 2023-10-15 20:47:39
tags: Pandas
mathjax: true
---

# Python-pandas

```python
import pandas
```

## 文件读写通用

```python
folder_name=os.path.dirname(__file__)
file_name=os.path.join(folder_name,'*.txt')
```

## 读写.csv文件

```python
df=pandas.read_csv(file_name,encoding='GBK')
"""
    read_csv常用参数：
    index_col:设置索引，如：
        index_col=0 第1列
        index_col='orderDate'
        index_col=[0,1]
        index_col=['orderDate','itemNo']
    sep或delimiter:二选一
        sep='\t' 设置分隔符
        delimiter='\t' 设置备用界定符
    header:指定第几行为表头，跳过此前数据
        header=3 从第4行为表头开始
"""
print(df)
csv_default=os.path.join(folder_name,'*.csv')
df.to_csv(csv_default) #出现中文乱码
csv_GBK=os.path.join(folder_name,'*.csv')
df.to_csv(csv_GBK,encoding='GBK',index=False)
```

## 读写Excel

```python
df=pandas.read_excel(file_name)
print(df)
df=pandas.read_excel(
    file_name,
    converters={
        '表头1':str,
        '表头2':float,
        ...
    }
)
print(df)
```

## 读取复杂表头Excel

```python
pandas.set_option('display.max_columns',None)
"""
    常用参数：
    display.max_columns/display.min_columns 最多/少显示列数 None无限制
    display.max_colwidth 最大显示列宽 默认50 None无限制
    display.max_rows/display.min_rows 最多/少显示行数 None无限制
    display.precision 浮点精度（小数点后位数） 默认6 38以上不使用科学计数法
"""
df=pandas.read_excel(file_name,sheet_name=1,header=None,usecols='A:P',nrows=7,skiprows=2,
                     names=['新表头1','新表头2',...])
"""
    常用参数：
    
"""
df.to_excel(result_file,sheet_name='...',index=False)
```

