---
title: Java笔记
date: 2023-10-14 19:49:53
tags: Java
mathjax: true
---

# Java笔记

## 生成.exe方法

**准备`manifest.mf`**
    Manifest-Version:1.0

```bash
jar cvfm *.jar manifest.mf 目录名称
```

**安装exe4j**

1. Project type选"JAR in EXE" mode
2. Application info 文件名、.exe导出位置
3. Executable info
        选 GUI程序、控制台晨雾、web服务
        exe文件名、图标
        是否允许多开
        32-bit or 64-bit中勾上
        Manifest options中DPI选Always
4. Java invocation右侧+添加Archive选择.jar包
        右下角...添加主类
5. JRE 最低最高 最低选1.7
        表中3个删掉，添加自己的jre
6. Splash screen启动画面
7. Messages默认英语 没汉语

另一种：**Launch4j**

## 注解

```java
/***
 *@version
 *@author
 *@since //最早JDK
 *
 *@see //引用其他类的文档
 *@param ... //方法参数
 *@return //返回值
 *@throws ... //异常说明
 */
```

## 包管理

```java
package a.b.c;// a/b/c 包目录

import a.b.c.MainClass;// a/b/c中MainClass类
import a.b.c.*;// 所有类文件
//第三方包.jar放到jdk1.8\jre\lib\ext
```

## 基本结构

```java
public class ...{
    public static void main(String[] args){
        ...//args.length ...
        return;
    };
    public static void test(String... args){
        for(String arg:args){
            ...
        };
    };
};
```

## 数据类型

```java
byte
short
int
long//结尾加L
float//结尾加F
double//结尾加D
char
boolean

instanceof

123//十进制
0123//八进制
0x123//十六进制
0b0000_0011//二进制

System.out.print("..."+...)
System.out.println("..."+...)

Integer.toBinaryString()
Integer.toOctalString()
Integer.toHexString()

Byte.SIZE
Byte.MIN_VALUE
Byte.MAX_VALUE

Short.SIZE
Short.MIN_VALUE
Short.MAX_VALUE

Long.SIZE
Long.MIN_VALUE
Long.MAX_VALUE

Float.SIZE
Float.MIN_VALUE
Float.MAX_VALUE

Float.SIZE
Float.MIN_VALUE
Float.MAX_VALUE
```

## 读入

```java
import java.util.Scanner;
int number;
Scanner scanner=new Scanner(System.in);
number=scanner.nextInt();
```

## 字符串

```java
String str=new String("...");
str=String.valueOf(1); //Integer或Boolean或char
str=String.valueOf(Boolean.TRUE); //true
String str=new String(char[],a,b); //char[]中从a开始的b个
String str=String.valueOf(char[],a,b);
String str="a";
str=str.concat("b");//ab
str.length()
str.charAt(...)
str.indexOf((int)' ')或str.indexOf(" ")
str.indexOf((int)' ',fromIndex)或str.indexOf(" ",fromIndex)//从fromIndex开始，找不到-1
str.lastIndexOf()//用法同上
//StringSequence=String CharBuffer Segment StringBuffer StringBuilder
str=str.replace(a,b)//a、b可以是char StringSequence，找不到返回原str，不支持正则
str=str.replaceAll(a,b)//a、b为String，a可正则
str=str.replaceFirst()//同上
str=str.substring(beginIndex)//从beginIndex开始截取
str=str.substring(begin,end)
String[] strArray=str.split("",k)//分割k次（可选） 可正则
boolean str.startsWith("...")
boolean str.startsWith("...",toffset) //从toffset开始
boolean str.endsWith("...")
str=str.trim(); //首尾去空格
str=str.toLowerCase();
str=str.toUpperCase();
char[] cs=str.toCharArrary();
boolean str1.equals(str2)
boolean str1.equalsIgnoreCase(str2)
```

## 数据格式化

```java
/*
    %s 字符串    %c 字符  %b 布尔                 %d 十进制整数      %x 十六进制整数
    %o 八进制整数 %f 浮点  %a 十六进制浮点（FF.35AE）%e 指数（10,01e+5）%g 通用浮点（短ef）
    %h 散列码    %% 百分比 %n 换行
*/
System.out.println(String.format("...",...));
("%+d",25) //正负号
("%-5d",10) //左对齐
("%04d",99) //前补0，同理空格
("%,f",9999.99) //用,数字分组，仅十进制
("%(f",-99.99) //括号包含负数
("%#x"或"%#o",99) //浮点：加小数点 十六、八进制：加0x、0
("%f和%<3.2f",99.99) //格式化前一个转换符描述参数?
("%1$d,%2$s",99,"abc") //被格式化参数索引?
```

## 日期格式化

```java
/*（系统语言环境下）
    %tB 月全称    %tb 月简称      %tA 星期全称 %ta 星期简称 %tY 年 四位
    %ty 年最后两位 %tj 一年中第几天 %tm 月 两位  %td 天 两位 %te 天

    %tH 24小时 两位 %tI 12小时 两位 %tk 24小时       %tl 12小时  %tM 分 两位
    %tS 秒 两位     %tL 毫秒 三位   %tN 微秒 九位     %tp 上下午  %tz 数字时区偏移量 GMTRFC822格式
    %tZ 时区缩写    %ts UTC到现在秒 %tQ UTC到现在毫秒
    UTC(2970-01-01 00:00:00)
*/
Date date=new Date();
System.out.println(String.format("%tY %tB %td %tH %tM %tS",date,date,date,date,date,date));
```

## 转化

```java
boolean str.contains("...")
int str1.compareTo(str2) //ASCII序，str1<st2时<0 反之>0 相同=0
int str1.compareToIgnoreCase() //同上
? str.hashCode()
str=str.toString;
```

## ArrayList

```java
import java.util.ArrayList;
import java.util.List;
List<String>strList=new ArrayList<>();
strList.add("...");
System.out.println(strList.toString());
```

## StringBuilder

```java
StringBuilder stringBuilder1=new StringBuilder();
StringBuilder stringBuilder2=new StringBuilder(capacity); //初始容量为capacity
StringBuilder stringBuilder3=new StringBuilder("...");
StringBuilder stringBuilder4=new StringBuilder(stringBuilder3);
stringBuilder.append("..."或boolean或'...'或int或float或StringBuilder);
System.out.println(stringBuilder);
stringBuilder.insert(0,...);//索引0处插入
stringBuilder.delete(begin,end);
char stringBuilder.charAt(0);
int stringBuilder.capacity();
stringBuilder.replace(start,end,"...");
stringBuilder.reverse();
System.out.println(stringBuilder.toString());
```

## StringBuffer

```java
//同StringBuilder
```

## Arrays

```java
import java.util.Arrays;
char charArray[]=new char[]{'a','b','c','d'};
String Arrays.toString(charArray) //[a,b,c,d]
char charArray[][]=new char[][]{{'a','b','c','d'},{'e','f','g'}};
String Arrays.deepToString(char Array) //[[a,b,c,d],[e,f,g]]
int array.length
Arrays.fill(array,123);
Arrays.fill(array,start,end,123); //左闭右开
int Arrays.binarySearch(array,value) //不存在返回-1，否侧索引
Arrays.sort(array);
char[] copyArray=Arrays.copyOf(charArray,len) //长度相等：一摸一样 小于原来：从0截取 大于原来：0或null填充
boolean Arrays.equals(charArray1,charArray2)
```

## 正则表达式

```java
/*
    \A 仅开头 \b 单词边界（...\b） \B 非单词边界 \f 换页   \n 换行
    \r 回车  \t 跳进             \v 垂直跳进   \z 仅结尾 \Z 仅结尾或换行前
    其他Python
*/
import java.util.regex.*;
String Pattern.matches(String regex,String str) 
Pattern pattern=Pattern.compile(regex);
Matcher matcher=pattern.matcher(str);
int groupCount=matcher.groupCount();//匹配到组合数量
if(matcher.find()){
    for(int i=0;i<=groupCount;i++){
        System.out.println(matcher.group(i));
    };
};
/*
    ?= 零宽正预测先行断言
    ?<= 零宽正回顾后发断言
    ?! 零宽负预测先行断言
    ?<! 零宽负回顾后发断言
*/
/*
    E-mail:^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
    Internet URL:^(https?:\/\/)?([\da-z.-]+).([a-z]{2,6})([\/\w.-])\/?$
    十六进制:^#?([a-f0-9]{6}|[a-f0-9]{3})$
    HTML标签:^<([a-z]+)([^<]+)(?:>(.)<\/\1>|\s+\/>)$
    匹配首尾空白:^\s|\s$
    手机号码:^(13[0-9]|14[0-9]|15[0-9]|166|17[0-9]|18[0-9]|19[8|9])\d{8}$
    电话号码:^(\(\d{3,4}-)|\d{3,4}-)?\d{7,8}$
    国内电话:\d{3}-\d{8}|\d{4}-\d{7}
    18位身份证:^((\d{18})|([0-9x]{18})|([0-9X]{18}))$
    账号合法（字母开头，5~16 字母数字_）:^[a-zA-Z][a-zA-Z0-9_]{4,15}
    密码（字母开头，6~18 字母数字_）:^[a-zA-Z]\w{5,17}$
    强密码（大小写数字 无特殊 8~10）:^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$
    日期：^\d{4}-\d{1,2}-\d{1,2}
    12个月:^(0?[1-9]|1[0-2])$
    31天:^((0?[1-9])|((1|2)[0-9])|30|31)$
    IP:^((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))$
*/
```

## OOP

```java
boolean object1.equals(object2)
final
//静态方法不可调用非静态方法，非静态方法可调用静态方法 main()需要静态方法
static{
    //只执行一次
};
public class ChildClass extends ParentClass{
    //...
};
super super() super(...)//子类构造方法重写 必须第一条执行
abstract
@Override
```

```java
public interface AnimalService{
    public void sleep();
};
public class AnimalServiceImpl implements AnimalService{
    @Override
    public void sleep(){
        //...
    };
};
```

## 箱操作

```java
Integer x=new Integer(10或"10");//手动装箱
Integer y=10;//自动装箱
int m=x.intValue();//手动拆箱
int n=x;//自动拆箱
//自动装箱陷阱
    Integer i1=100,i2=100;
    System.out.println(i1==i2);//true
    System.out.println(i1.equals(i2));//true
    i1=i2=200;
    System.out.println(i1==i2);//false
    System.out.println(i1.equals(i2));//true
    //java会缓存-128~127的数值，复用这个对象，分配同一地址，超过会重建一个对象
```

## 类型常量

```java
Integer.SIZE//32 二进制补码形式 位数
Integer.MIN_VALUE//-2^{31}
Integer.MAX_VALUE//2^{31}-1
Integer.BYTES//4 字节数
Integer.TYPE//?
boolean Integer.equals(Object)
static int Integer.compare(int,int)//相等0 1<2:-1 1>2:1
int Integer.compareTo(Integer)//相等0 小于入参-1 大于入仓1
static Integer Integer.valueOf(int)
static int Integer.parseInt(String)
static Integer Integer.valueOf(String)
String Integer.toString()
static String Integer.toString(int)
static String Integer.toBinaryString(int)
static String Integer.toOctalString(int)
static String Integer.toHexString(int)
static int Integer.signum(int)//正负
byte Integer.byteValue()
short Integer.shortValue()
int Integer.intValue()
long Integer.longValue()
float Integer.floatValue()
double Integer.doubleValue()
static int max(int,int)
static int min(int,int)
static int sum(int,int)
    
//MAX_VALUE(2^{1023}(2-2^{-52})) MIN_VALUE(2^{-1074}) SIZE(64) BYTES(8) TYPE同Integer
Double.MAX_EXPONENT//可能最大指数
Double.MIN_EXPONENT//可能最小指数
Double.NEGATIVE_INFINITY//负无穷大
Double.POSITIVE_INFINITY//正无穷大
Double.NaN //不是一个数字常量
Double d=new Double(10.01或"10.01")
//equals compare compareTo valueOf parseDouble toString toHexString byteValue shortValue intValue longValue floatValue doubleValue max min sum同Integer
boolean Double.isNaN()//是否非数字值
boolean Double.isInfinite()//是否无穷大
    
//TYPE同Integer
Boolean.TRUE
Boolean.FALSE
Boolean b=new Boolean(true或"true")
//equals compare compareTo valueOf parseBoolean toString booleanValue同Integer
boolean logicalAnd(boolean,boolean)
boolean logicalOr(boolean,boolean)
boolean logicalXor(boolean,boolean)

//MAX_VALUE('\uFFFF') MIN_VALUE('\u0000') SIZE(16) TYPE同Integer
Character.PRIVATE_USE//Unicode规范中'Co'类别
//equals compare compareTo valueOf toString charValue同Integer
static bool Character.isLowerCase(char)
static bool Character.isUpperCase(char)
static bool Character.isWhitespace(char)
static bool Character.toLowerCase(char)
static bool Character.toUpperCase(char)
import java.math.BigInteger;
import java.math.BigDecimal;
    BigInteger bigInteger=new BigInteger("1010");
    System.out.println(bigInteger.toString());
    bigInteger=bigInteger.add(new BigInteger("101"));
    BigDecimal bigDecimal=new BigDecimal("3.14");
    System.out.println(bigDecimal.precision());//精度
```

## 数学

```java
Math.PI
Math.E
double sin(double)
//asin sinh cos acos cosh tan atan tanh toRadians toDegrees同sin
double atan2(double,double)//直角坐标系转极坐标系 返回所得角
import java.lang.Math;
    double ceil(double)//>=入参的最小整数
    //floor（<=入参最大整数） rint（最接近或相等整数，同样接近返回偶数）同ceil
    int round(float)//最接近
    long round(double)//同上
double exp(double)//e次方
//log log10 sqrt cbrt（立方根）同exp
double pow(double,double)
int/long/float/double max/min( , )
int/long/float/double abs( )
double nextUp/nextDown( )//比入参大/小一些的浮点
double Math.random()//[0.0,1.0)
import java.util.Random;
    Random ran=new Random(无或seedValue);
    ran.nextInt()//随机int
    ran.nextInt(int n)//[0,n)
    ran.nextLong()
    ran.nextBoolean()
    ran.nextFloat()
    ran.nextDouble()
    ran.nextGaussian()//概率密度为高斯分布的double
```

## Enum

```java
public enum ColorEnum{
    RED("红色"),GREEN("绿色"),YELLOW("黄色"),BLUE("蓝色");
    public String color;
    private ColorEnum(){};//构造方法必须private
    private ColorEnum(String color){
        this.color=color;
    };
};
public class UseEnum{
    public static void main(String[] args){
        ColorEnum colorArray[]=ColorEnum.values();//按数组形式返回
        for(int i=0;i<colorArray.length;i++){
            System.out.println(colorArray[i]);//RED GREEN YELLOW BLUE
        };
        for(int i=0;i<colorArray.length;i++){
            System.out.println(colorArray[i].color);//红色 绿色 黄色 蓝色
        };
        System.out.println(ColorEnum.RED.compareTo(ColorEnum.GREEN));//比较定义时前后顺序 -1
        for(int i=0;i<colorArray.length;i++){
            System.out.println(colorArray[i].ordinal());//获取索引 0 1 2 3
        };
        return;
    };
};
```

## EnumSet

挖坑待填

## EnumMap

挖坑待填

## 泛型类

```java
public class Demo<T>{
    private T name;
    private List<T> desc;
    public void setName(T name){
        this.name=name;
        return;
    };
    public T getName(){
        return name;
    };
    public void setDesc(List<T>desc){
        this.desc=desc;
        return;
    };
    public List<T>getDesc(){
        return desc;
    };
};
public class Demo<T,S,U>{
    private T name;
    private List<S>desc;
    private U age;
};
public class ...{
    public <T> void toString(T t){
        System.out.println(t.getClass().getName());
    };
    public static void main(String[] args){
        //...
    };
};
```

## Collection接口

```java
/*
    add(E e)
    addAll(Collection<? extends E>c)
    clear()
    boolean contains(Object o)
    isEmpty()
    remove(Object o)
    size()
    iterator()
*/
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
public class UseCollection{
    public static void main(String[] args){
        Collection<String>collection=new ArrayList<String>();
        collection.add("a");
        collection.add("b");
        Iterator iterator=collection.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        };
        return;
    };
};
```

## List集合

AbstractList Stack Vector AttributeList挖坑待填

## ArrayList类

```java
/*其他
    sort(Comparator<? super E>c) 排序?
    subList(int fromIndex,int toIndex) 获取子集合
    trimToSize() 大小修改为当前元素大小
    indexOf(Object o) 返回索引，第一个
    lastIndexOf(Object o) 同上 最后一个
*/
import java.util.*;
public class UseArrayList{
    public static void main(String[]args){
        List<String>list=new ArrayList<String>();//后String可省，括号里选填容量
        list.add("a");
        list.add("b");
        list.add("c");
        //add(index,"...") 加入
        //set(index,"...") 改变值
        //get(index)
        list.remove(1);
        for(String element:list){
            System.out.println(element);
        };
        Iterator<String>iterator=list.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        };
        return;
    };
};
```

## LinkedList类

```java
/*
    pop() 取一元素
    push(E e) 放入堆栈
    offer(E e) 添加到末尾
    peek() 获取第一个元素
*/
```

## Set集合

AbstractSet EnumSet LinkedHashSet挖坑待填

## HashSet类

```java
import java.util.*;
Set<String>hashSet=new HashSet<>();
hashSet.add("...");//有重复时只保留一个
hashSet.remove("...");
int hashSet.size()
Iterator<String>iterator=hashSet.iterator();
```

## TreeSet类

```java
/*
    ceiling(E e) 返回>=给定的最小元素 不存在null
    descendingIterator() 降序迭代器
    first() 第一个元素
    floor(E e) <=给定的最大元素 不存在null
    last() 最后一个元素
    higher(E e) >给定元素最小元素 不存在null
    lower(E e) 同上<
    headSet(E toElement) 返回部分集合 <入参
    subSet(E fromElement,E toElement) 同上 [fromElement,toElement)
    tailSet(E fromElement) 同上>=入参
*/
import java.util.*;
public class UseTreeSetMethod{
    public static void main(String[]args){
        TreeSet<Integer>treeSet=new TreeSet<>();
        treeSet.add(1);
        treeSet.add(2);
        treeSet.add(3);
        Iterator<Integer>iterator=treeSet.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        };
        TreeSet<Person>personSet=new TreeSet<>();
        personSet.add(new Person(26,"a"));
        personSet.add(new Person(22,"b"));
        personSet.add(new Person(33,"c"));
        Iterator<Person>personIterator=personSet.iterator();
        while(personIterator.hasNext()){
            Person person=personIterator.next();
            System.out.println(""+person.name+person.age);
        };
        return;
    };
};
public class Person implements Comparable<Person>{
    public Person(int age,String name){
        this.age=age;
        this.name=name;
    };
    int age;
    String name;
    public int compareTo(Person person){
        int num=this.age-person.age;
        return num;
    };
    public String toString(){
        return ""+this.name+this.age;
    };
};
```

## Map集合

### hashMap

```java
/*其他：
    putAll(Map<? extends K,? extends V>m) 全部添加到集合中
    clear()
    boolean containsKey(Object key) 是否包含
    boolean containsValue(Object value)
    isEmpty()
    remove(Object key)
    size()
    hashCode()
*/
public class UseHashMap{
    public static void main(String[]args){
        Map<String,String>hashMap=new HashMap<>();
        hashMap.put("a","1");
        hashMap.put("b","2");
        hasMap.put("c","3");
        for(String key:hashMap.keySet()){
            System.out.println(""+key+hashMap.get(key));
        };

        Iterator<Map.Entry<String,String>>it=hashMap.entrySet().iterator();
        while(it.hasNext()){
            Map.Entry<String,String>entry=it.next();
            System.out.println(""+entry.getKey()+entry.getValue());
        };
        
        for(Map.Entry<String,String>entry:hasMap.entrySet()){
            System.out.println(""+entry.getKey()+entry.getValue());
        };//速度最快

        for(String v:hashMap.values()){
            System.out.println(v);
        };
        return;
    };
};
```

### TreeMap

```java
/*
    ceilingEntry(K key) >=key的最小key键值对 不存在null下同
    ceilingKey(K key) >=key的最小key
    descendingKeySet()
    firstEntry() 最小键关联的键值对
    subMap(K fromKey,K toKey) 返回[fromKey,toKey)的映射集合
*/
import java.util.Iterator;
import java.util.TreeMap;//Person类同上
public class UseTreeMap{
    public static void main(String[]args){
        TreeMap<Person,String>treeMap=new TreeMap<>();
        treeMap.put(new Person(1,"a"),"a1");
        treeMap.put(new Person(2,"b"),"b1");
        treeMap.put(new Person(3,"c"),"c1");
        Iterator<Person>personIterator=treeMap.keySet().iterator();
        while(personiterator.hasNext()){
            Person person=personIterator.next();
            System.out.println(person.toString);
        };
        return;
    };
};
```

## Collection算法

```java
Collections.sort(list) //升序
Collections.shuffle(list)
Collections.reverse(list)
```

## Java反射

### 获取Class对象的引用

```java
package com.demo;
class Student{};
public class Test{
    public static void main(String[]args){
        //法1
        Student student=new Student();
        Class clazz=student.getClass();
        //法2
        Class clazz Student.class;
        //法3?
        try{
            Class clazz=Class.forName("com.demo.Student");
        };
        catch(ClassNotFoundException e){
            e.printStackTrace();
        };
        return;
    };
};
```

### 获取构造方法

```java
/*
    其他方法：?
    getConstructor(Class<?>...parameterTypes)
    getDeclaredConstructor(Class<?>...parameterTypes)
    isVarArgs()
    setAccessible(boolean flag)
    getParameterTypes()
*/
package com.demo;
import java.lang.reflect.Constructor;
public class Test{
    public static void main(String[]args)throws Exception{
        Class clazz=Class.forName("com.demo.Person");
        Constructor[]constructors=clazz.getDeclaredConstructors();
        for(Constructor constructor:constructors){
            System.out.println(""+constructor);
        };
        //public com.demo.Person()
        //public com.demo.Person(java.lang.String,int)
        Constructor constructor1=clazz.getConstructor();
        Object object1=constructor1.newInstance();
        Person person1=(Person)object1;
        person1.say();
        Constructor contructor2=clazz.getConstructor(String.class,int.class)
        Object object2=constructor2.newInstance("b",2);
        Person person2=(Person)object2;
        person2.say();
        return;
    };
};
class Person{
    private String name="a";
    private int age=1;
    public Person(){};
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    };
    public void say(){
        System.out.println(""+name+age);
        return;
    };
};
```

### 获取成员变量

```java
/*
    其他方法：?
    getField(String name)
    getFields()
    getDeclearedField(String name)
    get(Object obj)
    set(Object obj,Object value)
    getInt(Object obj,int i)
    setInt(Object obj.int i)
    getBoolean(Object obj)
    setBoolean(Object obj,boolean z)
    getFloat(Object obj)
    setFloat(Object obj,float f)
*/
package com.demo;
import java.lang.reflect.Field;
public class Test{
    public static void main(String[]args){
        printClassVariables(new Person());
        return;
    };
    public static void printClassVariables(Object obj){
        Class c=obj.getClass();
        Field[]fields=c.getDeclaredFields();
        for(Field field:fields){
            Class fieldType=field.getType();
            String typeName=fieldType.getSimpleName();
            String fieldName=field.getName();
            System.out.println(""+typeName+fieldName);
            //Stringname
            //intage
        };
        return;
    };
};
class Person{
    private String name="a";
    private int age=1;
};
```

### 获取方法

```java
/*
    其他方法：
    getMethods()
    getDeclaredMethod(String name,Class<?>...parameterTypes)
    getDeclaredMethods()
    getExceptionTypes()
*/
import java.lang.reflect.Method;
public class Test{
    public static void main(String[]args){
        printClassMethods(new Person("a",1));
        return;
    };
    public static void printClassMethods(Object obj){
        Class c=obj.getClass();
        Method[]methods=c.getDeclareMethods();
        for(Method method:methods){
            Class returnType=method.getReturnType();
            System.out.print(returnType.getSimpleName());
            System.out.print(""+method.getName()+"(");
            Class[]parameterTypes=method.getParameterTypes();
            for(Class paramType:parameterTypes){
                System.out.print(paramType.getSimpleName()+",");
            };
            //void run()
            //void swim()
            //void say(String,)
        };
        return;
    };
};
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    };
    public void say(String message){
        ...
        return;
    };
    public void run(){
        ...
        return;
    };
    public void swim(){
        ...
        return;
    };
};
```

### invoke()调用

```java
package com.demo;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
public class Test{
    public static void main(String[]args)throws InvocationTargetException,IllegalAccessException{
        PrintUtil printUtil=new PrintUtil();
        Class clazz=printUtil.getClass();
        try{
            Method m1=clazz.getMethod("print",int.class,int.class);
            m1.invoke(printUtil,1,2);
            Method m2=clazz.getMethod("pirnt",String.class,String.class);
            m2.invoke(printUtil,"hello","world");
            Method m3=clazz.getMethod("print");
            m3.invode(printUtil);
            //12
            //HELLO WORLD
            //Hello world!
        };
        catch(NoSuckMethodException e){
            e.printStackTrace();
        };
        return;
    };
};
class PrintUtil{
    public void print(int a,int b){
        System.out.println(a+b);
        return;
    };
    public void print(String a,String b){
        System.out.println(a.toUpperCase()+b.toUpperCase());
        return;
    };
    public void print(){
        System.out.println("Hello world!");
        return;
    };
};
```

## 注解

```java
import java.lang.annotation.ElementType;
@Target
/*
    ANNOTATION_TYPE 注解类型
    CONSTRUCTOR 构造器
    FIELD 域
    LOCAL_VARIABLE 局部变量
    METHOD 方法
    PACKAGE 包
    PARAMETER 方法变量
    TYPE 类、接口、enum
    TYPE_PARAMETER Type声明式前
    TYPE_USE 所有Type
*/
import java.lang.annotation.RetentionPolicy;
@Retention
/*
    SOURCE 源文件有效
    CLASS 进.class
    RUNTIME 运行时有效
*/
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
public @interface Test{
    ...
};
@Inherited 被自动继承
@Documented 被javadoc文档化
@Override 重写方法
@Deprecated 已过时
@SuppressWarnings("...")
@SuppressWarnings({"...","..."})
/*
    deprecation 不赞成使用的
    unchecked 未检查的转换
    fallthrough switch没有break
    path 不存在的路径
    serial 可序列化类缺少serialVersionUID定义
    finally finally不能正常完成
    unused 变量方法没被使用
    rawtypes 泛型没指定类型
    all 所有
*/
//自定义注解：
@Target(...)
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface CustomAnnotation{//注解中方法不许protected private不写保持默认即可
    int num()default 10;
    String name()default "a";
    String desc()default "b";
};
public class AnnotationTest{
    @CustomAnnotation(name="c",desc="d")
    public void test1(){
        ...
    };
    @CustomAnnotation
    public void test2(){
        ...
    };
};
//注解解析?
public class MyAnnotationProcessor{
    public static void main(String[]args){
        try{
            Class clazz MyAnnotationProcessor.class.getClassLoader().loadClass("com.demo.AnnotationTest");
            Field[]fields=clazz.getDeclaredFields();
            for(Field field:fields){
                CustomAnnotation myAnnotation=field.getAnnotation(CustomAnnotation.class);
                System.out.println(""+myAnnotation.name()+myAnnotation.num()+myAnnotation.desc());
            };
            Method[]methods=clazz.getMethods();
            for(Method method:methods){
                if(method.isAnnotationPresent(CustomAnnotation.class)){
                    CustomAnnotation myAnnotation=method.getAnnotation(CustomAnnotation.class);
                    System.out.println(""+myAnnotation.name()+myAnnotation.num()+myAnnotation.desc());
                };
            };
        };
        //a10b
        //c10d
        catch(ClassNotFoundException e){
            e.printStackTrce();
        };
        return;
    };
};
```

## Date类

```java
/*
    G 纪元
    y 四位年份
    M 月
    d 日
    h 1~12小时
    D 0~23小时
    m 分
    s 秒
    S 毫秒
    E 星期几
    D 一年中日
    F 一月中第几周周几
    w 一年中第几周
    W 一月中第几周
    a A.M./P.M.
    k 1~24
    K A.M./P.M.(0~11)小时
    z 时区
    Z RFC882时区
    " 单引号
    例：yyyy-MM-dd HH:mm:ss
*/
//Date类
/*
    其他方法：
    after(Date when) 同before
    clone() 返回副本
    compareTo(Date date) 比较日期
    equals(Object o) 是否相同
    from(Instant instant) 从Instant获取Date
    hashCode() 返回哈希值
    setTime() 设置为自1970/1/1 00:00:00GMT到现在毫秒数
*/
import java.util.Date;
Date date=new Date(无或System.currentTimeMillis());
System.out.println(date.toString());
System.out.println(date.getTime());//自1970/1/1 00:00:00GMT到现在毫秒数
Date date2=new Date(date.getTime()-1000);
System.out.println(date.before(date2));//false date不在date2之前
```

## Calendar类

```java
/*
    ALL_STYLES 所有样式
    AM 午夜到中午前时间段
    PM 中午到午夜前时间段
    WEEK_OF_YEAR 当年第几周
    YEAR 年
    MONTH 月-1
    DATE或DAY_OF_MONTH 日期
    HOUR 12小时制小时
    HOUR_OF_DAY 24小时制小时
    MINUTE 分钟
    SECOND 秒
    DAY_OF_WEEK 1=星期日 以此类推
*/
import java.util.Calendar;
Calendar cal=Calendar.getInstance();
System.out.println(cal.getTime());
cal.set(年，月-1，日);
System.out.println(cal.get(Calendar.YEAR));
```

## DateFormat类

```java
/*
    SHORT
    MEDIUM =不填（默认）
    LONG
    FULL
*/
import java.text.DateFormat;
Date now=new Date();
DateFormat dateFormat=DateFormat.getDateInstance(DateFormat.MEDIUM);
System.out.println(dateFormat.format(now));
now=dateFormat.parse("2018-07-25");
System.out.println(now.toString());
```

## SimpleDateFormat类

```java
import java.text.SimpleDateFormat;
SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy-MM-dd hh:mm:ss:SSS");
System.out.println(simpleDateFormat.format(new Date()));
long time=simpleDateFormat.parse(str).getTime();//String str格式与simpleDateFormat 化为毫秒数
System.out.println(simpleDateFormat.format(time));//格式化
```

## DateTimeFormatter（线程安全）

```java
import java.time.format.DateTimeFormatter;
import java.time.LocalDateTime;
DateTimeFormatter formatter=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime time=LocalDateTime.parse("2018-07-25 10:00:00",formatter);
System.out.println(formatter.format(time));
```

## Unix时间戳

```java
System.currentTimeMillis()/1000;
```

## InputStream类

```java
/*
    其他：
    available() 预估可读字节数
    mark(int readlimit) 标记当前位置
    markSupported() 判断是否支持mark() reset()
    reset() 定位到上次调用mark()位置
    skip(long n) 跳过丢弃n字节数据
*/
import java.io.FileInputStream;
import java.io.InputStream;
InputStream inputstream=new FileInputStream("..."或new File("..."));
System.out.println(inputStream.read());
inputStream.close();
```

## Reader类

```java
/*
    其他：
    close() 关闭、释放
    mark(int readAheadLimit) 标记当前位置
    markSupported() 判断是否支持mark()
    ready() 判断是否准备好
    reset() 重置
    skip() 跳过当前字符
*/
import java.io.FileReader;
import java.io.Reader;
Reader reader=new FileReader("...");
System.out.println(reader.read());
```

## OutputStream类

```java
/*
    其他：
    flush() 刷新、强制任何缓存输入字节被写出
*/
import java.io.*;
File outputFile=new File("...");
OutputStream outputStream=new FileOutputStream(outputFile);
outputStream.write(...);
outputStream.close();
```

## Writer类

```java
/*
    其他：
    append() 添加
    flush() 同上flush()
*/
import java.io.*;
Writer writer=new FileWriter("...");
writer.write(...);
writer.close();
```

## 系统预定义流

```java
import java.io.*;
BufferedReader reader=new BufferedReader(new InputStreamReader(System.in));
System.out.println(reader.readLine());
```

## File类

```java
/*
    其他：
    canExecute() 是否是可执行文件
    canWrite() 是否可写
    CompareTo(File pathname) 比较两个文件
    createTempFile(String prefix,String suffix) 用指定前缀后缀创建一个具有名称的空文件
    delete() 删除文件、目录
    deleteOnExit() JVM终止时删除文件、目录
    exists() 判断文件、目录是否存在
    File getAbsoluteFile() 绝对路径的File
    String getAbsolutePath() 绝对路径名称
    getParent() 获取父目录名称，不存在为null
    hashCode() 相对路径哈希值
    isFile() 是否为文件
    isHidden() 是否为隐藏文件
    lastModified() 上次修改时间(ms)
    length() 文件字节长度
*/
import java.io.*;
File file=new File("...");//文件或文件夹
boolean file.createNewFile() //是否创建新空文件成功
boolean file.isAbsolute() //是否为绝对路径
boolean file.canRead() //是否可读
boolean file.renameTo(new File("...")) //重命名并是否成功
boolean file.mkdirs() //创建目录是否成功
boolean file.isDirectory() //是否是目录
String s[]=file.list();
File[] files=file.listFiles();
String file.getName();
boolean file.delete() //删除并是否成功
```

## FileInput/OutputStream类

```java
FileInputStream input=new FileInputStream("..."或new File("..."));
FileOutputStream output=new FileOutputStream("..."或new File("..."));//File后可加boolean append
byte[] byteArray=new byte[input.available()];
input.read(byteArray);
output.write(byteArray);
input.close();
output.close();
```

## FileReader/Writer类

```java
FileReader fileReader=new FileReader("...");
FileWriter fileWriter=new FileWriter("...");
char fileReader.read();
fileWriter.write("...");
fileReader.close();
fileWriter.close();
```

## BufferedInput/OutputStream类

```java
import java.io.*;
FileInputStream input=new FileInputStream("...",选填size);
FileOutputStream output=new FileOutputStream("...",选填size);
BufferedInputStream bufferInput=new BufferedInputStream(input);
BufferedOutputStream bufferOutput=new BufferedOutputStream(output);
byte[] buffer=new byte[1024];
int pufferInput.read(buffer);//-1表示结束
String content=null;
content+=new String(buffer,0,flag);
bufferOutput.write(content.getBytes(),0,content.getBytes().length());
bufferOutput.flush();
bufferOutput.close();
bufferInput.close();
```

## BufferedReader/Wirter类

```java
/*
    其他：
    Stream lines() 读取所有行
    mark(int readAheadLimit)、markSupported()、read()、ready()、reset()、skip(long n)同上
*/
FileReader fileReader=new FileReader("...");
FileWriter fileWriter=new FileWriter("...");
BufferedReader reader=new BufferedReader(fileReader);
BufferedWriter writer=new BufferedWriter(fileWriter);
String reader.readLine();//返回null结束
reader.close();
writer.write("...");
writer.newLine();
writer.flush();
writer.close();
```

## DataInput/OutputStream类

```java
import java.io.*;
File file=new File("...");
DataOutputStream out=new DataOutputStream(new FileOutputStream(file));
out.writeBoolean(...);
out.writeByte(...);
out.writeChar(...);
out.writeShort(...);
out.writeInt(...);
out.writeLong(...);
out.writeUTF("...");
out.close();
DataInputStream in=new DataInputStream(new FileInputStream(file));
short in.readShort();
int in.readInt();
long in.readLong();
boolean in.readBoolean();
byte in.readByte();
char in.readChar();
in.close();
```

## ObjectOutput/InputStream序列化

```java
//transient修饰不能被序列化
/*
    flush()
    reset()
    write(int b)
    writeBoolean(boolean b)
    writeByte(int i)
    writeChar(String str)
    writeFloat(float f)
    writeDouble(double d)
    writeShort(int i)
    writeInt(int i)
*/
import java.io.Serializable;
public class Cat implements Serializable{
    public String name,desc;
    public Integer age;
};
import java.io.*;
Cat cat=new Cat();
FileOutputStream fileOut=new FileOutputStream("*.ser");
ObjectOutputStream out=new ObjectOutputStream(fileOut);
out.writeObject(cat);
out.close();
fileOut.close();
FileInputStream fileIn=new FileInputStream("*.ser");
ObjectInputStream in=new ObjectInputStream(fileIn);
cat=(Cat)in.readObject;
in.close();
filein.close();
```

## xlsx

```java
import org.apache.poi.ss.usermodel.*;
File file=new File("*.xlsx");
InputStream inputStream=new FileInputStream(file);
Workbook workbook=WorkbookFactory.create(inputStream);
inputStream.close();
Sheet sheet=workbook.getSheetAt(0);//工作表
int rowLength=sheet.getLastRowNum()+1;//总行数
Row row=sheet.getRow(int);//列
int colLength=row.getLastCellNum();//总列数
Cell cell=row.getCell(int);//单元格 null不存在
CellStyle cellStyle=cell.getCellStyle();//单元格样式
cell.setCellType(CellType.STRING);
String cell.getStringCellValue();
cell.setCellValue("...");//修改
OutputStream out=new FileOutputStream(file);
workbook.write(out);
```

## zip

```java
import java.util.zip.*;
public static String compress(String str)throws IOException{
    if(str==null||str.length()==0){
        return str;
    };
    ByteArrayOutputStream byteArrayOutputStream=new ByteArrayOutputStream();
    GZIPOutputStream gzipOutputStream=new GZIPOutputStream(byteArrayOutputStream);
    gzipOutputStream.write(str.getBytes());
    gzipOutputStream.close();
    return byteArrayOutputStream.toString("ISO-8859-1");
};
public static String uncompress(String str)throws IOException{
    if(str==null||str.length()==0){
        return str;
    };
    ByteArrayOutputStream byteArrayOutputStream=new ByteArrayOutputStream();
    ByteArrayInputStream byteArrayInputStream=new ByteArrayInputStream(str.getBytes("ISO-8859-1"));
    GZIPInputStream gzipInputStream=new GZIPInputStream(byteArrayInputStream);
    byte[]buffer=new byte[256];
    int n;
    while((n=gzipInputStream.read(buffer))>=0){
        byteArrayOutputStream.write(buffer,0,n);
    };
    return byteArrayOutputStream.toString();
};
```

## 异常处理

```java
throw new RuntimeException("...");
public static void test()throws Exception{
    ...
};
try{
    throw ...;
}
catch(Exception e){
    e.printStackTrace();
     //或
     System.out.println(e.getClass().getName());//异常
     System.out.println(e.getMessage())//内容
}
catch(... e){
    ...
}
finally{
    //正常执行
};
//自定义异常
public class DefineException extends Exception{
    public DefineException(String ErrorMessage){
        super(ErrorMessage);
    };
};
//注意：finally中return值覆盖try中
```

## Thread

```java
/*
    其他：
    activeCount() 当前进程、子进程活动数
    checkAccess() 是否有权限修改
    currentThread() 当前正执行线程对象
    getId() 标识符
    getName() 名称
    getPriority() 优先级
    getState() 状态
    interrupt() 终端
    interrupted() 是否终端
    isAlive() 是否存活
    isDaemon() 是否为守护线程
    isInterrupted() 是否被中断
    join() 等待终止
    join(long millis) 等待终止时间为ms
    run() 若使用Runnable则调用run() 否则无操作
    setName(String name) 设置名称
    setPriority(int priority) 设置优先级
        Thread.MIN_PRIORITY 1
        Thread.NORM_PRIORITY 5
        Thread.MAX_PRIORITY 10
    sleep(long millis) 休眠ms
    start() 开始
    yield() 暂停
*/
public class TreadDemo extends Thread{//将extends Thread改为implements Runnable可实现继承其他类
    private Thread t;
    private String threadName;
    public ThreadDemo(String name){
        threadName=name;//创建线程
    };
    public void run(){//重写
        try{
            ...
        }
        catch(InterruptedException e){
            ...//中断
        };
        //退出终止
        return;
    };
    public void start(){//启动
        if(t==null){
            t=new Thread(this,threadName);
            t.start();
            return;
        };
        this.start();
        return;
    };
};
public class Demo{
    public static void main(String[]args){
        ThreadDemo thread=new ThreadDemo("...");
        thread.start();
        return;
    };
};
```

## Callable&Future接口

```java
/*
    其他：
    cancel(boolean b) 尝试取消
    get(long timeout.TimeUnit unit) 等待最长指定时间完成 获取结果
    isCancelled() 是否正常完成前被取消
    isDone() 是否已经完成
*/
import java.util.concurrent.*;
public class Demo implements Callable<Integer>{
    public static void main(String[]args){
        Demo demo=new Demo();
        FutureTask<Integer>ft=new FutureTask<>(demo);
        //...
        System.out.println(Thread.currentThread().getName());//main
        new Thread(ft,"...").start();
        try{
            System.out.println(""+ft.get());
        }
        catch(Exception e){
            e.printStackTrace();
        };
        return;
    };
    @Override
    public Integer call()throws Exception{
        ...
        return ...;
    };
};
```

## 生命周期

```java
public class Demo implements Runnable{
    public synchronized void notifying()throws InterruptedException{
        notify();
        return;
    };
    public synchronized void waiting()throws InterruptedException{
        wait();
        return;
    };
    public void run(){
        try{
            Thread.sleep(100);
            waiting();
        }
        catch(Exception e){
            e.printStackTrace();
        };
        return;
    };
    public static void main(String[]args)throws InterruptedException{
        Demo demo=new Demo();
        Thread thread=new Thread(demo);
        System.out.println(thread.getState());//NEW
        thread.start();
        System.out.println(thread.getState());//RUNNABLE
        Thread.sleep(50);
        System.out.println(thread.getState());//TIMED_WAITING
        Thread.sleep(100);
        System.out.println(thread.getState());//WAITING
        demo.notifying();//返回同步方法前
        System.out.println(thread.getState());//BLOCKED
        thread.join();//结束
        System.out.println(thread.getState());//TERMINALED
    }
};
```

