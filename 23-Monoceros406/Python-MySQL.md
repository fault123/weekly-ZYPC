---
title: Python-MySQL
date: 2023-10-15 20:46:59
tags: Python MySQL
mathjax: true
---

# Python-MySQL

```python
import sqlite3
conn=sqlite3.connect('*.db')
cursor=conn.cursor()
cursor.execute('...') #SQL语句
cursor.close()
conn.commit() #提交事务
conn.close()
```

## 查询信息

```python
result1=cursor.fetchone() #返回下一条数据
result2=cursor.fetchmany(size) #返回size个数据
result3=cursor.fetchall() #返回所有数据
    #例如[(1,'MRSOFT'),(2,'Andy'),(3,'...')]
```

## 占位符

```python
cursor.execute('select * from user where id > ?',(1,))
```

```python
import pymysql
db=pymysql.connect("主机名/IP","用户名","密码","数据库名称",...)
    #可选参数：charset
cursor=db.cursor()
cursor.execute("...")
data=cursor.fetchone()
...
db.ping(reconnect=True) #如果断开则重连
db.close()
```

## 批量操作

```python
data=[(...),...]
try:
    cursor.executemany("insert into books(...)values(%s,...)",data)
    db.commit()
except:
    db.rollback()
```

