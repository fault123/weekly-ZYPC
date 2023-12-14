---
title: Ruby初探
date: 2023-10-22 21:49:02
tags: Ruby
---

# Ruby初探

## 基本操作

```ruby
puts 'hello, world'
language='Ruby'
puts 'hello, #{language}'
```

## 判断

逻辑运算符短路。

```ruby
x=4
puts 'aaa' unless x==4
puts 'bbb' if x==4
if x==4
    puts 'ccc'
end
unless x==4
    puts 'ddd'
else
    puts 'eee'
end
puts 'fff' if not true
puts 'ggg' if !true
#等同于true：数字、字符串
#等同于false：nil
```

## 循环

```ruby
x=x+1 while x<10
x=x-1 until x==1
while x<10
    x=x+1
    puts x
end
```

## 鸭子类型

> Ruby在大多数情况下表现为“强类型语言”，即发生类型冲突时抛出错误。只有当程序运行时才进行类型检查，这叫做*动态类型*。

```ruby
i=0
a=['100',100.0]
while i<2
    puts a[i].to_i
    i=i+1
end
```

`.to_i`可将String、Float等类型转换成整数。

> 只要它像鸭子一样走路，像鸭子一样嘎嘎叫，那它就是只鸭子。

在这里`.to_i`相当于嘎嘎叫。

## 定义函数

```ruby
def tell_the_truch
	true
end
```

每个函数都会返回结果，如果没有显示指定返回值，则返回退出函数前最后一个表达式的值。

## 数组

```ruby
animals=['lions','tigers','bears']
animals[0]
animals[-1]
animals[0..1]
(0..1).class #Range

a=[1]
a.push(1)
a.pop
```

 ## 散列表

```ruby
numbers={1=>'one',2=>'two'}
numbers[1]
stuff={:array=>[1,2,3],:string=>'Hi,mom!'}
stuff[:string]
```

### 参数传递

```ruby 
def tell_the_truth(options={})
    if options[:profession]==:lawyer
        '...'
    else
       	true
    end
end
tell_the_truth #true
tell_the_truth :professon=>:lawyer
```

## 代码块

只占一行时用大括号，占多行用do/end。

### times

```ruby
3.times{puts '...'}

#以下为手动实现
class Fixnum
    def my_times
        i=self
        while i>0
            i=i-1
            yield
        end
    end
end
3.my_times{puts '...'}
```

### each

```ruby
animals=['lions and','tigers and','bears','oh my']
animals.each{|a| puts a}
```

### 传递闭包

代码可以用作一等参数。

```ruby
def call_block(&block) #'&'表示把代码块作为闭包传递给函数
    block.call
end
def pass_block(&block)
    call_block(&block)
end
pass_block{puts '...'}
```

## 类

```ruby
class Tree
    attr_accessor:children,:node_name
    def initialize(name,children=[]) #初始化新对象时调用
        @children=children
        @node_name=name
    end
    def visit_all(&block)
        visit &block
        children.each(|c| c.visit_all &block)
    end
    def visit(&block)
        block.call self
    end
end

ruby_tree=Tree.new("Ruby",[Tree.new("Reia"),Tree.new("MacRuby")])
ruby_tree.visit{|node|puts node.node_name}
ruby_tree.visit_all{|node|puts node.node_name}
```

> 类名以大写字母开头，采用大驼峰命名法。实例变量前必须加‘@’，类变量前必须加上‘@@’。实例变量和方法名以小写字母开头，采用下划线命名法。常量全大写形式。用于逻辑测试的函数和方法一般加上问号。

`attr`定义实例变量和访问变量的同名方法，`attr_accessor`定义实例变量、访问方法和设置方法。当把`true`作为第二个参数传给`attr`时，例如：`attr :children,true`，也可以定义设置方法。

## Mixin

Ruby采用模块解决多继承问题。

```ruby
module ToFile
    def filename
        "object_#{self.object_id}.txt"
    end
    def to_f
        File.open(filename,'w'){|f|f.write(to_s)}
    end
end
class Person
    include ToFile
    attr_accessor :name
    def initialize(name)
        @name=name
    end
    def to_s
        name
    end
end
Person.new('matz').to_f
```

## 模块、可枚举和集合

> 太空船操作符“a<=>b”：b较大返回-1，a较大返回1，相等返回0。

```ruby
'begin'<=>'end'
'same'<=>'same'
a=[5,3,4,1]
a.sort #[1,3,4,5]
a.any?{|i|i>4} #true
a.all?{|i|i>4} #false
a.collect{|i|i*2} #[10,6,8,2]
a.select{|i|i%2==1} #[5,3,1]
a.max #5
a.member?(2) #false
```

> inject方法：代码块中有两个参数。通过第二个参数把每个列表元素传入代码块，第一个参数是上一次执行的结果。第一次执行时初始值作为inject参数传入，不设初始值则为集合中第一个值。

```ruby
a.inject(0){|sum,i|sum+i} #13
a.inject{|sum,i|sum+i} #13
a.inject{|product,i|product+i} #60

a.inject(0) do |sum,i|
    puts "sum: #{sum} i: #{i} sum+i: #{sum+i}"
    sum+i
end
```

## method_missing

当找不到某个方法时调用。

```ruby
class Roman
    def self.method_missing name,*args
        roman=name.to_s
        roman.gsub!("IV","IIII")
        roman.gsub!("IX","VIIII")
        roman.gsub!("XL","XXXX")
        roman.gsub!("XC","LXXXX")
        (roman.count("I")+roman.count("V")*5+roman.count("X")*10+roman.count("L")*50+roman.count("C")*100)
    end
end
puts Roman.X
puts Roman.XC
puts Roman.XII
puts Roman.X
```

## 模块

### 超类

```ruby
class ActsAsCsv
    def read
        file=File.new(self.class.to_s.downcase+'.txt')
        @headers=file.get.chomp.split(', ')
        file.each do |row|
            @result<<row.chomp.split(', ')
        end
    end
    def headers
        @headers
    end
    def csv_contents
        @result
    end
    def initialize
        @result=[]
        read
    end
end
class RubyCsv<ActsAsCsv
end
m=RubyCsv.new
puts m.headers.inspect
puts m.csv_contents.inspect
```

### 宏

```ruby
class ActsAsCsv
    def self.acts_as_csv
        define_method "read" do
            file=File.new(self.class.to_s.downcase+'.txt')
            @headers=file.gets.chomp.split(', ')
            file.each do |row|
                @result<<row.chomp.split(', ')
            end
        end
        define_method "headers" do
            @headers
        end
        define_method "csv_contents" do
            @result
        end
        define_method "initialize" do
            @result=[]
            read
        end
    end
end
class RubyCsv<ActsAsCsv
    acts_as_csv
end
m=RubyCsv.new
puts m.headers.inspect
puts m.csv_contents.inspect
```

### 模块

```ruby
module ActsAsCsv
    def self.included(base)
        base.extend ClassMethods
    end
    module ClassMethods
        def acts_as_csv
            include InstanceMethods
        end
    end
    module InstanceMethods
        def read
            @csv_contents=[]
            filename=self.class.to_s.downclass+'.txt'
            file=File.new(filename)
            @headers=file.gets.chomp.split(', ')
            file.each do |row|
                @csv_contents<<row.chomp.split(', ')
            end
        end
        attr_accessor :headers, :csv_contents
        def initialize
            read
        end
    end
end
class RubyCsv
    include ActsAsCsv
    acts_as_csv
end
m=RubyCsv.new
puts m.headers.inspect
puts m.csv_contents.inspect
```

