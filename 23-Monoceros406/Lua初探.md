---
title: Lua初探
date: 2023-10-22 21:46:16
tags: Lua
---

# Lua初探

## 编译与反编译

用以下命令将.lua源代码编译成.out字节码：

```bash
luac main.lua
```

默认文件名为luac.out。反编译用unluac.exe，把.out文件拷到.exe同一文件夹下：

```bash
unluac luac.out>main.lua
```

## 输出

```lua
print(1989)
return 1989
=1989
=xxx --nil类型：不存在
print "xxx"
print "xxx" print "xxx" --自动换行
```

## 字符串拼接

```lua
='xxx'..'xxx'
```

## 取字符串长度

```lua
=#'...'
```

## 表达式

```lua
--数学表达式有+-*/% 和 乘方^
=not((true or false)and false)
=true or func() --“短路”：func()不会被执行
-- ==相等~=不相等
```

## 函数

```lua
function triple(num)
    return 3*num
end

--或如下：
=(function(num) return 3*num end)(2)

--函数传递
function call_twice(f)
    ff=function(num)
        return f(f(num))
    end
function triple(n)
	return n*3
end
times_nine=call_twice(triple)
=times_nine(5)
```

## 灵活传参

函数传参时少的默认`nil`，多的忽略。

```lua
function f(friend,...)
    foes={...}
    print(foes[1])
    print(foes[2])
end
```

## 尾递归优化

> 当一个递归函数对自己得调用是最后一件事时，该函数会被尾递归优化处理。为防止发生栈溢出，把尾递归优化为`goto`语句。

```lua
function reverse(s,t)
    if #s<1 then
        return t
    end
    first=string.sub(s,1,1) --string.sub(字符串,起始位置,终止位置) 可以负数
    rest=string.sub(s,2,-1)
    return reverse(rest,first..t)
end
large=string.rep('hello',5000)
print(reverse(large,''))
```

## 函数多返回值

多的忽略，少的默认`nil`。

```lua
function weapons()
    return 'a','b'
end
w1,w2=weapons()
```

## 具名参数

```lua
function f(table)
    print('xxx'..table.medium)
end
function(small=5.00,medium=7.00,jumbo=15.00)
```

## 控制流程

### `if`语句

```lua
if file=='a' then
    --...
elseif file=='b' then
    --...
```

### `for`循环

```lua
for i=1,5 do
    print(i)
end

for i=1,5,2 do
    print(i)
end
```

### `while`循环

```lua
while math.random(100)<50 do
    --...
end
```

## 局部变量

```lua
function f(a,b)
    local a2=a*a
    local b2=b*b
    return math.sqrt(a2+b2)
end
```

## Table

### Table构造器

```lua
book={
    title="xxx",
    author="xxx",
    pages=100
}
```

### Table操作

```lua
=book.title --输出
book.stars=5 --添加
book.author="xxx" --修改
book.pages=nil --删除

key="title"
=book[key] --当键包含空格、小数点或运行时计算
```

### Table打印

```lua
function print_table(t)
    for k,v in pairs(t) do --pairs迭代器 直到返回nil
        print(k..":"..v)
    end
end
```

### 字典（shu zu

```lua
--创建数据
medals={
    "a",
    "b",
    "c"
}

--读写
=medals[1]
medals[4]="lead"
```

### 风格混用

```lua
ice={
    "a",
    "b";
    c=true
}
=ice[1]
=ice.c
```

## 将文件加载到REPL

```lua
dofile('*.lua')
```

或启动时：

```bash
lua -l *
```

## metatable

### 元表基础

```lua
greek={
    t1="a",
    t2="b",
    t3="c"
}
function table_to_string(t)
    local result={}
    for k,v in pairs(t) do
        result[#result+1]=k..":"..v
    end
    return table.concat(result,"\n")
end
mt={
    __tostring=table_to_string --重写__tostring函数
}
setmetatable(greek,mt)
=greek
```

### 读写

目标：读取不存在的键或覆盖已存在的键导致运行时错误

```lua
local _private={}
function strict_read(table,key)
    if _private[key] then
        return _private[key]
    else
        error("xxx"..key) --不存在
    end
end
function strict_write(table,key,value)
    if _private[key] then
        error("xxx"..key) --已存在
    else
        _private[key]=value
    end
end
local mt={
    __index=strict_read,
    __newindex=strict_write
}
treasure={}
setmetatable(treasure,mt)
```

## 面向对象

```lua
Villain={
    health=100,
    new=function(self,name)
        local obj={
            name=name,
            health=self.health,
        }
        --以下两行保证take_hit函数成员仍属于Villain对象
        setmetatable(obj,self)
        self.__index=self
        return obj
    end,
    take_hit=function(self)
    	self.health=self.health-10
    end
}
dietrich=Villain.new(Villain,"Dietrich")
```

### 语法糖

> 如果写的是`table:method()`而不是`table.method(self)`，Lua会隐式地传入`self`参数，不需要显式存在。

```lua
Villain={
    health=100
}
function Villain:new(name)
    --可以直接使用self
end
function Villain:take_hit()
    --可以直接使用self
end
SuperVillain=Villain:new()
function SuperVillain:take_hit()
    --可以直接使用self
end

--使用：
dietrich=Villain:new("Dietrich")
dietrich:take_hit()
print(dietrich.health)
toht=SuperVillain:new("Toht")
toht:take_hit()
print(toht.health)
```

## 协程

```lua
function fibonacci()
    local m=1
    local n=1
    while true do
        coroutine.yield(m) --暂停协程并返回
        m,n=n,m+n
    end
end
generator=coroutine.create(fibonacci)
succeeded,value=coroutine.resume(generator) --转到fibonacci
=value --1
succeeded,value=coroutine.resume(generator) --继续fibonacci
=value --2
```

## 多任务

实现类似线程的行为，`scheduler.lua`：

```lua
local pending={}
local function schedule(time,action)
    pending[#pending+1]={
        time=time,
        action=action
    }
    sort_by_time(pending)
end
local function sort_by_time(array)
    table.sort(array,function(e1,e2)return e1.time<e2.time end)
end
local function wait(seconds)
    coroutine.yield(seconds)
end
local function run()
    while #pending>0 do
        while os.clock()<pending[1].time do
        end
        local item=remove_first(pending)
        local _,seconds=coroutine.resume(item.action)
        if seconds then
            later=os.clock()+seconds
            schedule(later,item.action)
        end
    end
end
local function remove_first(array)
    result=array[1]
    array[1]=array[#array]
    array[#array]=nil
    return result
end
return{
    schedule=schedule,
    run=run,
    wait=wait
}
```

使用方法：

```lua
scheduler=require 'scheduler'
--require()对比dofile()：检查是否加载过、搜索多个程序库加载路径、给局部变量提供安全的命名空间
function punch()
    for i=1,5 do
        print('punch '..i)
        scheduler.wait(1.0)
    end
end
function block()
    for i=1,3 do
        print('block '..i)
        scheduler.wait(2.0)
    end
end
scheduler.schedule(0.0,coroutine.create(punch))
scheduler.schedule(0.0,coroutine.create(block))
scheduler.run()
```

