---
title: MySQL笔记
date: 2023-10-15 20:30:16
tags: SQL
mathjax: true
---

# MySQL笔记

## 现有服务

```
老笔记本现有服务：
    mysql1:
        root
        john376577
新笔记本现有服务：
    mysql1:
        root
        john376577
```

## MySQL安装方法

下载并解压压缩包，添加`bin`为环境变量。

`README`同目录下新建`my.ini`，内容为：

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=E:\\software\\mysql\\mysql-8.0.11-winx64   # 切记此处一定要用双斜杠\\，单斜杠我这里会出错，不过看别人的教程，有的是单斜杠。自己尝试吧
# 设置mysql数据库的数据的存放目录
datadir=E:\\software\\mysql\\mysql-8.0.11-winx64\\Data   # 此处同上
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```

其中路径要自己新建。

管理员身份运行`cmd`。

bin下执行：

```bash
mysqld --initialize --console
```

>  [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: og(jsq8Otd6l

记住密码`og(jsq8Otd6l`。

```bash
mysqld --install [服务名]
net start [服务名] 启动
  net stop [服务名] 停止
  sc delete [服务名]或mysqld -remove 卸载
mysql -u root -p
```

执行后输入（临时）密码。

改密码：

```sql
alter user "root"@'localhost' identified with mysql_native_password by 'john376577';
```

## MySQL Workbench安装方法

安装，`Preference`中`Apprearance`中`Fonts`改为`Simplified Chinese`，`data`文件夹中`main_menu.xml`内容改为`main_menu_Chinese.xml`中内容。

## 启动

```bash
mysql -u用户名 -p密码 -h ip地址 -P端口号
```

## 注释

```sql
--注释
/*
    多行注释
*/
```

## 类型

```sql
--整数类型 可unsigned
tinyint -128~127
smallint -32768~32767
mediumint -8388608~8388607
int -2147483648~2147483647
bigint -9223372036854775808~9223372036854775807

--浮点类型
float
double

--定点类型
decimal(M,D)
/*
    M为不算小数点总长
    D为小数点后长度
*/

--字符串类型
char varchar
binary varbinary
tinytext text mediumtext longtext
enum
set --假如有abc 可选a b c ab ac bc abc

--时间类型
year 1901~2155
/*
    插入4位数字或字符串
    插入2位字符串：'0'和'00'~'69'相当于"2000~2069" '70'~'99'相当于"1970~1999"
    插入2位数字：同上，插入0表示0000
*/
time -838:59:59~838:59:59
/*
    插入HH:MM:SS
    插入D HH:MM:SS时相当于(D*24+HH):MM:SS
    可插入HH:MM、SS、D HH、D HH:MM、HHMMSS、current_time()、now()
*/
date 1000-01-01~9999-12-31
/*
    YYYY-MM-DD ‘-’可用'@''.'等代替
    YY-MM-DD YY规则与year类似
    可插入current_date()、now()
*/
datetime 1000-01-01 00:00:00~9999-12-31 23:59:59
/*
    YYYY-MM-DD HH:MM:SS
*/
timestamp 19700101080001~20380119111407
/*
    可输入current_timestamp()、null、（无输入）,都表示当前timestamp
    与时区相关
*/
```

## create

```sql
--[表示可选参数] {a|b|...}任选一个
--创建库
create database ...;--创建不存在的库
create database if not exists ...;--创建可能存在的库时不报错
create database ... default character set utf8;--指定字符集UTF-8
create database ... default charset utf8 collate utf8_romanian_ci;--指定校验规则
create unique index 索引名 on 表名(列名1[(长度)],...[desc]);--创建唯一索引 desc降序
create index 索引名 on 表名(列名(长度));--创建普通索引
create [temporary] table [if not exists] ...( --创建表
    列名1 数据类型 [约束条件] [默认值],
    列名2 数据类型 [约束条件] [默认值],
    ...
)[表约束条件];
primary key(列名1,列名2,...) --主键约束
[constraint<外键名>]foreign key(列名1,...)references<父表名>(主键列名1,...) --外键约束
列名 数据类型 not null --非空约束
列名 数据类型 unique --唯一约束
列名 数据类型 default 默认值 --默认约束
列名 数据类型 auto_increment --自增属性
    例如：
    create table 'user_tmp3'(
        'id' int(11),
        'name' varchar(128),
        'age' int(11),
        primary key('id','name'),
        unique [索引名](列名(长度)),
        index [索引名](列名(长度))
    )engine=innodb default charset=utf8
```

## use

```sql
use ...;--切换库
```

## show

```sql
show warnings;--查看警告
show tables;--查看表
show create database ...;--查看库的创建方式
show create table ...;--查看表结构
show databases;--查看有哪些库
show engines;--查看支持哪些引擎
show character set;--查看支持的字符集
    IE6用UTF-8 命令行用GBK 一般程序用GB2312 UTF-8格式所有编码通吃
    LATIN1<GB2312<GBK<UTF-8 一定保证connection字符集>client字符集
show collation;--查看字符集校验规则
show variables like 'character%'\g;--查看每列字符集
    character_set_client 客户端请求数据的字符集
    character_set_connection 客户机/服务器连接的字符集
    character_set_database 默认数据库字符集，建议不要人为定义
    character_set_filesystem os上文件名(character_set_client)转成此字符集，默认binary不转换
    character_set_results 返回给客户端的字符集
    character_set_server 数据库服务器默认字符集
    character_set_system 总是utf8，用于表列、函数的名字
show variables like 'collation%'\g;--查看校验规则
    collation_connection 当前连接的字符集
    collation_server 服务器默认校验
    collation_database 当前日期默认校验。每次跳转另一个库时改变，没有库时等于collation_server
```

## desc

```sql
desc ...;--查看表的列定义
```

## alter

```sql
alter database ... default character set utf8;--调整字符集
alter table 原表名 rename [to] 新表名;
alter table 表名 modify 列名 数据类型;--修改字段类型
alter table 表名 change 原列名 新列名 数据类型;
alter table 表名 add column 新列名 数据类型 [约束条件][first|after 字段名]; --添加字段 first第一列 after在某字段后
alter table 表名 drop 列名; --删除字段
alter table 表名 modify 列名 数据类型 {first|after 字段名};--调整字段位置
alter table 表名 engine=新引擎名;
alter table 表名 drop foreign key 外键约束名;--删除外键约束
alter table 表名 add unique [索引名](列名[(长度)]);--创建唯一索引
alter table 表名 add index 索引名(列名);--创建普通索引
```

## drop

```sql
drop database ...;--删除库
drop table [if exists] 表1,表2,...;
drop index [索引名] on 表名;
```

## rename

```sql
rename table 原表名 to 新表名;
```

## insert

```sql
insert into 表名 (列名1,...,值n)values(值1,...,值n),(...),...;--新增数据
```

## select

```sql
select 列名1,... from 表名 [where 条件][limit n][offset m];--查询数据
例如：
    select * from user;
    select name,age from user where age>20;
```

## update

```sql
update 表名 set 列名1=新值1,... [where 条件];--修改数据
```

## delete

```sql
delete from 表名 [where 条件];--删除数据
```

## replace

```sql
replace [into] 表名 [(列名1,...)]{values|value}({expr|default},...),(...),...;--有则修改 无则插入
```

