---
title: JDBC笔记
date: 2023-10-14 20:22:11
tags: JDBC
mathjax: true
---

# JDBC笔记

## 导入

```java
//导入mysql-connector-j-8.0.33.jar包
public static void main(String[]args)throws Exception{
    Class.forName("com.mysql.cj.jdbc.Driver");//加载驱动程序
};
```

## DriverManager类

```java
/*
    其他：
    deregisterDriver(Driver driver) 从DriverManager中删除指定驱动程序
    getConnection(String url) 连接指定数据库
    getConnection(String url,String user,String password) 同上
    getDriver(String url) 获取指定url驱动程序Driver对象
    getLoginTimeout() 获取登录最长等待时间(s)
    getLogWriter() 获取指定日志记录器对象
    println(String message) 打印当前JDBC日志流消息
    registerDriver(Driver driver) 注册指定驱动程序
    setLoginTimeout(int seconds) 设置连接超时时间
*/
import java.sql.DriverManager;
DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mysql","用户名","密码");
```

## Connection接口

```java
/*
    其他：
    abort(Executor executor) 断开连接
    clearWarnings() 清除警告
    close() 关闭连接
    commit() 将之前提交或回滚操作成为持久更改，释放Connection对象持有的数据库锁
    isClosed 判断Connection对象是否已关闭
    prepareStatement(String sql) 创建一个PrepareStatement对象，用于发送SQL语句
    rollback() 车险当前事务所有更改，释放Connection对象持有的数据库锁
*/
import java.sql.Connection;
Connection connection=DriverManager.getConnection(...);
System.out.println(connection.getClientInfo().toString());//客户端信息
System.out.println(connection.toString());//连接数据库Connection对象
connection.close();
```

## Statement接口

```java
/*
    其他：
    addBatch(String sql) 指定语句添加到命令列表中
    cancel() 终止语句
    close() 关闭库 释放JDBC资源
    getConnection() 获取连接库生成的Statement对象
*/
import java.sql.Statement;
Statement statement=connection.createStatement();
statement.execute("...");//执行SQL语句
statement.close();
```

## PreparedStatement接口

```java
/*
    其他：
    clearParameters() 清除当前参数值
    setString(int index,String str) 指定索引参数设为str
    setInt(int index,int i) 同上
    executeQuery() 执行查询语句，返回ResultSet
*/
PreparedStatement statement=connection.prepareStatement("...");
statement.setString(1,"Peter");
```

## ResultSet接口

```java
/*
    其他：
    absolute(int row) 指针移动到指定行数
    afterLast() 指针移动到最后一行之后
    beforeFirst() 指针移动到第一行之前
    cancelRowUpdate() 取消对当前行的更新
    clearWarnings() 消除警告
    close() 关闭释放系统资源
    deleteRow() 删除当前行
    findColumn(String columLabel) 查找列索引
    first() 移动到第一行
    getArray() 获取指定列的值作为Array返回
    getBigDecimal()
    getBoolean()
    getByte()
    getDate()
    getDouble()
    getFloat()
    getInt()
    getLong()
    getObject()
    getString()
    getTime() 获取指定列的值作为java.sql.Time返回
    getRow() 获取当前行号
    getStatement() 获取Statement对象
    last() 移动到最后一行
    next() 指针向前移动一行
    previous() 指针向上移动一行
*/
ResultSet resultSet=statement.executeQuery("...");
while(resultSet.next()){
    System.out.println(resultSet.getString("id")); //数据库读取id
    System.out.println(resultSet.getString("name"));
    ...
};
```

## JDBC操作MySQL

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.sql.PreparedStatement;
public class javatest1{
    public static void main(String[] args)throws Exception{
        //链接
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection connection=DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mysql","root","john376577");
        //创建数据表
        Statement statement=connection.createStatement();
        String sql="CREATE TABLE `user1` (`id` int(11) NOT NULL AUTO_INCREMENT,`name` varchar(45) DEFAULT NULL,`phone` varchar(45) DEFAULT NULL,`age` int(8) DEFAULT NULL,PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8";
        statement.executeUpdate(sql);
        statement.close();
        //向数据表添加、修改数据
        String sql="insert into user (`name`,`phone`,`age`)values(?,?,?,)";
        PreparedStatement statement=connection.prepareStatement(sql);
        statement.setString(1,"Peter");
        statement.setString(2,"13888888888");
        statement.setString(3,22);
        System.out.println("要插入的数据为："+statement.toString());
        statement.executeUpdate();
        statement.close();
        //添加数据+获取自动生成ID
        String sql="insert into user (`name`,`phone`,`age`)values(?,?,?,)";
        PreparedStatement statement=connection.prepareStatement(sql,Statement.RETURN_GENERATED_KEYS);
        statement.setString(1,"Peter");
        ...;
        statement.executeUpdate();
        ResultSet generatedIds=statement.getGeneratedKeys();
        while(generatedIds.next())
            System.out.println(generatedIds.getLong(1));
        statement.close();
        //查询数据
        Statement queryStatement=connection.createStatement();
        ResultSet resultSet=queryStatement.executeQuery("select * from user");
        while(resultSet.next())
            System.out.println(resultSet.getString("id")+resultSet.getString("name")+resultSet.getString("age"));
        //批量处理
        connection.setAutoCommit(false);
        Statement statement=connection.createStatement();
        statement.addBatch("...");
        statement.addBatch("...");
        statement.executeBatch();
        connection.commit();
        //事务回滚
        connection.rollback();
        //关闭链接
        connection.close();
    };
};
```

