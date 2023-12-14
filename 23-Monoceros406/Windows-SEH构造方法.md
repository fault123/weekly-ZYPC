---
title: Windows-SEH构造方法
date: 2023-11-15 20:23:24
tags: SEH
mathjax: true
---

# Windows-SEH构造方法

常用系统异常值：

| 异常                               | 异常值     | 解释                                                         |
| ---------------------------------- | ---------- | ------------------------------------------------------------ |
| EXCEPTION_ACCESS_VIOLATION         | 0xC0000005 | 程序企图读写一个不可访问的地址时引发的异常。例如企图读取0地址处的内存。 |
| EXCEPTION_ARRAY_BOUNDS_EXCEEDED    | 0xC000008C | 数组访问越界时引发的异常。                                   |
| EXCEPTION_BREAKPOINT               | 0x80000003 | 触发断点时引发的异常。                                       |
| EXCEPTION_DATATYPE_MISALIGNMENT    | 0x80000002 | 程序读取一个未经对齐的数据时引发的异常。                     |
| EXCEPTION_FLT_DENORMAL_OPERAND     | 0xC000008D | 如果浮点数操作的操作数是非正常的，则引发该异常。所谓非正常，即它的值太小以至于不能用标准格式表示出来。 |
| EXCEPTION_FLT_DIVIDE_BY_ZERO       | 0xC000008E | 浮点数除法的除数是0时引发该异常。                            |
| EXCEPTION_FLT_INEXACT_RESULT       | 0xC000008F | 浮点数操作的结果不能精确表示成小数时引发该异常。             |
| EXCEPTION_FLT_INVALID_OPERATION    | 0xC0000090 | 该异常表示不包括在这个表内的其它浮点数异常。                 |
| EXCEPTION_FLT_OVERFLOW             | 0xC0000091 | 浮点数的指数超过所能表示的最大值时引发该异常。               |
| EXCEPTION_FLT_STACK_CHECK          | 0xC0000092 | 进行浮点数运算时栈发生溢出或下溢时引发该异常。               |
| EXCEPTION_FLT_UNDERFLOW            | 0xC0000093 | 浮点数的指数小于所能表示的最小值时引发该异常。               |
| EXCEPTION_ILLEGAL_INSTRUCTION      | 0xC000001D | 程序企图执行一个无效的指令时引发该异常。                     |
| EXCEPTION_IN_PAGE_ERROR            | 0xC0000006 | 程序要访问的内存页不在物理内存中时引发的异常。               |
| EXCEPTION_INT_DIVIDE_BY_ZERO       | 0xC0000094 | 整数除法的除数是0时引发该异常。                              |
| EXCEPTION_INT_OVERFLOW             | 0xC0000095 | 整数操作的结果溢出时引发该异常。                             |
| EXCEPTION_INVALID_DISPOSITION      | 0xC0000026 | 异常处理器返回一个无效的处理的时引发该异常。                 |
| EXCEPTION_NONCONTINUABLE_EXCEPTION | 0xC0000025 | 发生一个不可继续执行的异常时，如果程序继续执行，则会引发该异常。 |
| EXCEPTION_PRIV_INSTRUCTION         | 0xC0000096 | 程序企图执行一条当前CPU模式不允许的指令时引发该异常。        |
| EXCEPTION_SINGLE_STEP              | 0x80000004 | 标志寄存器的TF位为1时，每执行一条指令就会引发该异常。主要用于单步调试。 |
| EXCEPTION_STACK_OVERFLOW           | 0xC00000FD | 栈溢出时引发该异常。                                         |

## hgame creakme2

源代码：

```c++
int FilterFuncofDBZ(int dwExceptionCode){
    if(dwExceptionCode==EXCEPTION_INT_DIVIED_BY_ZERO){
        return EXCEPTION_EXECUTE_HANDLER;
    };
    return HEXCEPTION_CONTINUE_SEARCH;
};
int FilterFuncofOF(int dwExceptionCode){
    if(dwExceptionCode==EXCEPTION_INT_OVERFLOW){
        return EXCEPTION_EXECUTE_HANDLE;
    };
    return EXCEPTION_CONTINUE_SEARCH;
};
void encipher(unsigned int num_rounds,uint32_t v[2],uint32_t key[4]){
    unsigned int i;
    uint32_t v0=v[0],v1=v[1];
    int sum=0;
    for(i=0;i<num_rounds;i++){
        v0+=(((v1<<4)^(v1>>5))+v1)^(sum+key[sum&3]);
        int a;
        __try{
            __try{
                sum+=delta;
                a=1/(sum>>31);
            }
            __except(FilterFunofDBZ(GetExceptionCode())){
                sum^=0x1234567;
            };
        }
        __exception(FilterFuncofOF(GetExceptionCode())){
            sum=0x9E3779B1;
        }
        v1+=(((v0<<4)^(v0>>5))+v0)^(sum+key[(sum>>11)&3]);
    }
    v[0]=v0;
    v[1]=v1;
    return;
};
```

## [MoeCTF 2022]Fake_code

main函数：

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+20h] [rbp-B8h]
  int v5; // [rsp+24h] [rbp-B4h]
  __int64 inputLen; // [rsp+30h] [rbp-A8h]
  char inputStr[112]; // [rsp+50h] [rbp-88h] BYREF

  v5 = 0;
  puts("Can you read my assembly in exception?");
  puts("Give me your flag:");
  scanf("%s", inputStr);
  inputLen = -1i64;
  do
    ++inputLen;
  while ( inputStr[inputLen] );
  if ( inputLen == 51 )
  {
    for ( i = 0; i < 51; ++i )
    {
      v5 = (127 * v5 + 102) % 255;
      inputStr[i] ^= data[constPtr];
    }
    if ( strcompare((__int64)&enc, (__int64)inputStr) )
      puts("\nTTTTTTTTTTQQQQQQQQQQQQQLLLLLLLLL!!!!");
    else
      puts("\nQwQ, please try again.");
    return 0;
  }
  else
  {
    puts("\nQwQ, please try again.");
    return 0;
  }
}
```

第20行的汇编代码：

```assembly
loc_1400011B8:
;   __try { // __except at loc_1400011E9
imul    eax, [rsp+0D8h+var_B4], 7Fh
add     eax, 66h ; 'f'
cdq
mov     ecx, 0FFh
idiv    ecx
mov     eax, edx
mov     [rsp+0D8h+var_B4], eax
mov     eax, [rsp+0D8h+var_B4]
sar     eax, 7
mov     [rsp+0D8h+var_B0], eax
mov     eax, 1
cdq
idiv    [rsp+0D8h+var_B0]
mov     [rsp+0D8h+var_B0], eax
jmp     short loc_140001212
;   } // starts at 1400011B8
```

有一部分代码被隐藏在`__except`中：

```assembly
loc_1400011E9:
;   __except(loc_1400020D0) // owned by 1400011B8
imul    eax, cs:constPtr, 61h ; 'a'
add     eax, 65h ; 'e'
cdq
mov     ecx, 0E9h
idiv    ecx
mov     eax, edx
mov     cs:constPtr, eax
mov     eax, cs:constPtr
xor     eax, 29h
mov     cs:constPtr, eax
```

idiv为带符号除法，立即数为除数，被除数在eax中，商在eax中，余数在edx中。

loc_1400020D0函数为GetExceptCode()，内容如下：

```assembly
; START OF FUNCTION CHUNK FOR main

loc_1400020D0:
;   __except filter // owned by 1400011B8
push    rbp
sub     rsp, 20h
mov     rbp, rdx
mov     [rbp+48h], rcx
mov     rax, [rbp+48h]
mov     rax, [rax]
mov     eax, [rax]
mov     [rbp+38h], eax
mov     eax, [rbp+38h]
mov     ecx, eax
call    sub_140001000
nop
add     rsp, 20h
pop     rbp
retn
```

sub_140001000为自定义的filter，对值摁M找个枚举值：

```c++
_BOOL8 __fastcall sub_140001000(int a1)
{
  return a1 == (unsigned int)EXCEPTION_INT_DIVIDE_BY_ZERO;
}
```

发现是在`__try`中抛出了被零除的异常。

仔细分析代码，找出隐藏代码：

```c++
a=v5>>7;
if(a==0){
	dword_140005000=(dword_140005000*0x61+0x65)%0xE9;
	dword_140005000=dword_140005000^0x29;
};
```

变量0引出异常。写出exp：

```c++
#include<stdio.h> 
#include<stdlib.h>
#include<string.h>
unsigned char v7[]={30,112,122,110,234,131,158,239,150,226,178,213,153,187,187,120,185,61,110,56,66,194,134,255,99,189,250,121,163,109,96,148,179,66,17,195,144,137,189,239,212,151,248,123,139,11,45,117,126,221,203};
unsigned char bytes[]={172,4,88,176,69,150,159,46,65,21,24,41,177,51,170,18,13,137,230,250,243,196,189,231,112,138,148,193,133,157,163,242,63,130,142,215,3,147,61,19,5,107,65,3,150,118,227,177,138,74,34,85,196,25,245,85,166,31,14,97,39,203,31,158,90,122,227,21,64,148,71,222,0,1,145,102,183,205,34,100,245,165,156,104,165,82,134,189,176,221,118,40,171,22,149,197,38,44,246,57,190,0,165,173,227,147,158,227,5,160,176,29,176,22,11,91,51,149,164,9,22,135,86,31,131,78,74,60,85,54,111,187,76,75,157,177,174,229,142,200,251,14,41,138,187,252,32,98,4,45,128,97,214,193,204,59,137,197,139,213,38,88,214,182,160,80,117,171,23,131,127,55,43,160,29,44,207,199,224,229,73,201,250,107,192,152,102,153,146,0,2,212,117,70,34,5,53,209,75,197,173,224,142,69,59,80,21,181,46,133,48,137,84,18,222,241,90,240,43,167,27,74,38,93,152,212,161,190,209,77,126,56,222,11,10,84,184,115,109,173,140,30,217,49,95,86,126,189,72,50,152,46,62,235,162,29};
int tmp,a,b,v5,ptr=0x19;
int main(void){
    for(int i=0;i<51;i++){
        v5=((0x7f*v5+0x66)%255);
        a=v5>>7;
        if(a==0){
            ptr=(ptr*0x61+0x65)%0xE9;
            ptr=ptr^0x29;
        };
        v7[i]^=bytes[ptr];
        printf("%c",v7[i]);
    };
    return 0;
};
```

