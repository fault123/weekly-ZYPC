---
title: Angr做题笔记
date: 2023-11-16 13:05:12
tags: Angr
mathjax: true
---

# Angr做题笔记

## 00_angr_find

程序：

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+1Ch] [ebp-1Ch]
  char v5[9]; // [esp+23h] [ebp-15h] BYREF
  unsigned int v6; // [esp+2Ch] [ebp-Ch]
 
  v6 = __readgsdword(0x14u);
  printf("Enter the password: ");
  __isoc99_scanf("%8s", v5);
  for ( i = 0; i <= 7; ++i )
    v5[i] = complex_function(v5[i], i);
  if ( !strcmp(v5, "JACEJGCS") )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

脚本：

```python
import angr
project=angr.Project("./00_angr_find",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)	
sim.explore(find=0x08048675)
if sim.found:
    res=sim.found[0]
    res=res.posix.dumps(0)
    print(res)
```

`sim.found[0]`代表探索路径时得到的一条可解的路径。`res.posix.dumps(0)`表示去获取对应路径中，`stdin`的内容。

## 01_angr_avoid

加上表示被避开的路径：

```python
import angr
project=angr.Project("./01_angr_avoid",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x080485E0,avoid=0x080485A8)
if sim.found:
    res=sim.found[0]
    res=res.posix.dumps(0)
    print(res)
```

## 02_angr_find_condition

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+18h] [ebp-40h]
  int j; // [esp+1Ch] [ebp-3Ch]
  char v6[20]; // [esp+24h] [ebp-34h] BYREF
  char v7[20]; // [esp+38h] [ebp-20h] BYREF
  unsigned int v8; // [esp+4Ch] [ebp-Ch]
 
  v8 = __readgsdword(0x14u);
  for ( i = 0; i <= 19; ++i )
    v7[i] = 0;
  qmemcpy(v7, "VXRRJEUR", 8);
  printf("Enter the password: ");
  __isoc99_scanf("%8s", v6);
  for ( j = 0; j <= 7; ++j )
    v6[j] = complex_function(v6[j], j + 8);
  if ( !strcmp(v6, v7) )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

不同点在于：这个汇编程序中到处都是“Good Job.”，没法全部罗列出来。对find参数修改为一个函数：

```python
import angr
def succ(state):
    res=state.posix.dumps(1)
    if b"Good Job." in res:
        return True
    else:
        return False
project=angr.Project("./02_angr_find_condition",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=succ)
```

`state.posix.dumps(1)`返回`stdout`中的内容，`avoid`也可以这么用。

## 03_angr_simbolic_registers

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v3; // ebx
  int v4; // eax
  int v5; // edx
  int v6; // ST1C_4
  unsigned int v7; // ST14_4
  unsigned int v9; // [esp+8h] [ebp-10h]
  unsigned int v10; // [esp+Ch] [ebp-Ch]
 
  printf("Enter the password: ");
  v4 = get_user_input();
  v6 = v5;
  v7 = complex_function_1(v4);
  v9 = complex_function_2(v3);
  v10 = complex_function_3(v6);
  if ( v7 || v9 || v10 )
    puts("Try again.");
  else
    puts("Good Job.");
  return 0;
}
```

其中输入函数如下：

```c++
int get_user_input()
{
  int v1; // [esp+0h] [ebp-18h]
  int v2; // [esp+4h] [ebp-14h]
  int v3; // [esp+8h] [ebp-10h]
  unsigned int v4; // [esp+Ch] [ebp-Ch]
 
  v4 = __readgsdword(0x14u);
  __isoc99_scanf("%x %x %x", &v1, &v2, &v3);
  return v1;
}
```

Angr对`scanf`这类格式化字符串的支持并不好，但是这样写也能得到结果：

```python
import angr
project=angr.Project("./03_angr_symbolic_registers",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x80489E9)
if sim.found:
    res=sim.found[0]
    res=res.posix.dumps(0)
    print(res)# b'b9ffd04e ccf63fe8 8fd4d959'
else:
    print("No")
```

正确的操作：看汇编，发现`__isoc99_scanf`函数实际操作将值储存在寄存器中：

```assembly
.text:0804891E                 lea     ecx, [ebp+var_10]
.text:08048921                 push    ecx
.text:08048922                 lea     ecx, [ebp+var_14]
.text:08048925                 push    ecx
.text:08048926                 lea     ecx, [ebp+var_18]
.text:08048929                 push    ecx
.text:0804892A                 push    offset aXXX     ; "%x %x %x"
.text:0804892F                 call    ___isoc99_scanf
.text:08048934                 add     esp, 10h
.text:08048937                 mov     ecx, [ebp+var_18]
.text:0804893A                 mov     eax, ecx
.text:0804893C                 mov     ecx, [ebp+var_14]
.text:0804893F                 mov     ebx, ecx
.text:08048941                 mov     ecx, [ebp+var_10]
.text:08048944                 mov     edx, ecx
```

我们手动设置寄存器的值，然后将`get_user_input`函数跳过：

```python
import angr,claripy
project=angr.Project("./03_angr_symbolic_registers",auto_load_libs=False)
state=project.factory.blank_state(addr=0x08048980)
input1=claripy.BVS("input1",32) #(name,size)
input2=claripy.BVS("input2",32)
input3=claripy.BVS("input3",32)
state.regs.eax=input1
state.regs.ebx=input2
state.regs.edx=input3
sim=project.factory.simgr(state)
sim.explore(find=0x80489E9)
if sim.found:
    res=sim.found[0]
    res1=res.solver.eval(input1)
    res2=res.solver.eval(input2)
    res3=res.solver.eval(input3)
    print(hex(res1)+" "+hex(res2)+" "+hex(res3))#0xb9ffd04e 0xccf63fe8 0x8fd4d959
else:
    print("No")
```

## 04_angr_symbolic_stack

```c++
int handle_user()
{
  int v1; // [esp+8h] [ebp-10h] BYREF
  int v2[3]; // [esp+Ch] [ebp-Ch] BYREF
 
  __isoc99_scanf("%u %u", v2, &v1);
  v2[0] = complex_function0(v2[0]);
  v1 = complex_function1(v1);
  if ( v2[0] == 1999643857 && v1 == -1136455217 )
    return puts("Good Job.");
  else
    return puts("Try again.");
}
```

这么一把梭也行：

```python
import angr
project=angr.Project("./04_angr_symbolic_stack",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x080486E4)
if sim.found:
    res=sim.found[0]
    res=res.posix.dumps(0)
    print(res)#b'1704280884 2382341151'
```

正确的做法，看`v1`和`v2[3]`在栈中的位置：分别为`[ebp-10h]`和`[ebp-Ch]`。

```python
import angr,claripy
project=angr.Project("./04_angr_symbolic_stack",auto_load_libs=False)
state=project.factory.blank_state(addr=0x08048694)
input1=claripy.BVS("input1",32)
input2=claripy.BVS("input2",32)
state.regs.ebp=state.regs.esp
state.regs.esp-=0x1c
state.memory.store(state.regs.ebp-0xc,input1)
state.memory.store(state.regs.ebp-0x10,input2)
sim=project.factory.simgr(state)
sim.explore(find=0x080486E4)
if sim.found:
    res=sim.found[0]
    res=res.solver.eval(input1)
    print(res)
    res=sim.found[0]
    res=res.solver.eval(input2)
    print(res)
```

## 05_angr_symbolic_memory

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+Ch] [ebp-Ch]
 
  memset(&user_input, 0, 33);
  printf("Enter the password: ");
  __isoc99_scanf("%8s %8s %8s %8s", &user_input, &unk_A1BA1C8, &unk_A1BA1D0, &unk_A1BA1D8);
  for ( i = 0; i <= 31; ++i )
    *(i + 169583040) = complex_function(*(i + 169583040), i);
  if ( !strncmp(&user_input, "NJPURZPCDYEAXCSJZJMPSOMBFDDLHBVN", 32) )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

一把梭：

```python
import angr
project=angr.Project("./05_angr_symbolic_memory",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x0804866D)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))#b'NAXTHGNR JVSFTPWE LMGAUHWC XMDCPALU'
```

跟上一道题一样：

```python
import angr,claripy
project=angr.Project("./05_angr_symbolic_memory",auto_load_libs=False)
state=project.factory.blank_state(addr=0x080485FE)
pwd1=claripy.BVS("pwd1",64)
pwd2=claripy.BVS("pwd2",64)
pwd3=claripy.BVS("pwd3",64)
pwd4=claripy.BVS("pwd4",64)
state.memory.store(0x0A1BA1C0,pwd1)
state.memory.store(0x0A1BA1C0+8,pwd2)
state.memory.store(0x0A1BA1C0+8+8,pwd3)
state.memory.store(0x0A1BA1C0+8+8+8,pwd4)
sim=project.factory.simgr(state)
sim.explore(find=0x0804866D)
if sim.found:
    res=sim.found[0]
    print(res.solver.eval(pwd1))
    print(res.solver.eval(pwd2))
    print(res.solver.eval(pwd3))
    print(res.solver.eval(pwd4))
```

## 06_angr_symbolic_dynamic_memory

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  _BYTE *v3; // ebx
  _BYTE *v4; // ebx
  int v6; // [esp-18h] [ebp-24h]
  int v7; // [esp-14h] [ebp-20h]
  int v8; // [esp-10h] [ebp-1Ch]
  int v9; // [esp-8h] [ebp-14h]
  int v10; // [esp-4h] [ebp-10h]
  int v11; // [esp+0h] [ebp-Ch]
  int i; // [esp+0h] [ebp-Ch]
 
  buffer0 = malloc(9, v6, v7, v8);
  buffer1 = malloc(9, v9, v10, v11);
  memset(buffer0, 0, 9);
  memset(buffer1, 0, 9);
  printf("Enter the password: ");
  __isoc99_scanf("%8s %8s", buffer0, buffer1);
  for ( i = 0; i <= 7; ++i )
  {
    v3 = (_BYTE *)(buffer0 + i);
    *v3 = complex_function(*(char *)(buffer0 + i), i);
    v4 = (_BYTE *)(buffer1 + i);
    *v4 = complex_function(*(char *)(buffer1 + i), i + 32);
  }
  if ( !strncmp(buffer0, "UODXLZBI", 8) && !strncmp(buffer1, "UAORRAYF", 8) )
    puts("Good Job.");
  else
    puts("Try again.");
  free(buffer0);
  free(buffer1);
  return 0;
}
```

一把梭：

```python
import angr
project=angr.Project("./06_angr_symbolic_dynamic_memory",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x08048759)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))
```

这道题储存位置为堆，不能直接给出一个地址去储存。符号执行有个好处：不是真的执行，只是模拟执行代码，对地址本身没有限制，完全可以随意设定内存的使用方法。`endness`用于指定储存端序，`project.arch.memory_endness`反应平台默认端序（小端序）。

```python
import angr,claripy
project=angr.Project("./06_angr_symbolic_dynamic_memory",auto_load_libs=False)
state=project.factory.blank_state(addr=0x08048699)
buff0=0x0ABCC8A4
buff1=0x0ABCC8AC
pwd1=claripy.BVS("pwd1",64)
pwd2=claripy.BVS("pwd2",64)
state.memory.store(buff0,0xffffff00,endness=project.arch.memory_endness)
state.memory.store(buff1,0xffffff80,endness=project.arch.memory_endness)
state.memory.store(0xffffff00,pwd1)
state.memory.store(0xffffff80,pwd2)
sim=project.factory.simgr(state)
sim.explore(find=0x08048759)
if sim.found:
    res=sim.found[0]
    print(res.solver.eval(pwd1))
    print(res.solver.eval(pwd2))
```

## 07_angr_symbolic_file

从文件读取。

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int result; // eax
  int i; // [esp+Ch] [ebp-Ch]
 
  memset(&buffer, 0, 64);
  printf("Enter the password: ");
  __isoc99_scanf("%64s", &buffer);
  ignore_me(&buffer, 64);
  memset(&buffer, 0, 64);
  fp = fopen("OJKSQYDP.txt", "rb");
  fread(&buffer, 1, 64, fp);
  fclose(fp);
  unlink("OJKSQYDP.txt");
  for ( i = 0; i <= 7; ++i )
    *(_BYTE *)(i + 134520992) = complex_function(*(char *)(i + 134520992), i);
  if ( strncmp(&buffer, "AQWLCTXB", 9) )
  {
    puts("Try again.");
    exit(1);
  }
  puts("Good Job.");
  exit(0);
  _libc_csu_init();
  return result;
}
```

一把梭：

```python
import angr
project=angr.Project("./07_angr_symbolic_file",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x080489B0)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))
#b'AZOMMMZM\x00@\x04\x00\x01\x01\x01\x01\x01\x00\x00\x00\x02\x00\x01\x00\x80\x04\x80\x00\x02\x01\x04\x00\x02\x80\x08\x01\x00\x02\x01\x01\x01@\x01\x00\x08\x08\x04\x80\x04\x01\x80\x01\x04\x80\x02\x00\x00@\x00\x00\x00\x00\x00\x00'
```

模拟文件系统：

```python
import angr,claripy
project=angr.Project("./07_angr_symbolic_file",auto_load_libs=False)
state=project.factory.blank_state(addr=0x080488EA)
filename = 'OJKSQYDP.txt'
pwd1=claripy.BVS("pwd1",64*8)
pwdfile=angr.storage.SimFile(filename,content=pwd1,size=64)
state.fs.insert(filename,pwdfile) #将模拟出来的为你教案插入state符号中
sim=project.factory.simgr(state)
sim.explore(find=0x080489B0)
if sim.found:
    res=sim.found[0]
    print(hex(res.solver.eval(pwd1)))
#0x415a4f4d4d4d5a4d0000000000000000000000000002000020000000000200000000000000008000000000401002000000000000000000000004001000000000
```

## 08_angr_constraints

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+Ch] [ebp-Ch]
 
  qmemcpy(&password, "AUPDNNPROEZRJWKB", 16);
  memset(&buffer, 0, 17);
  printf("Enter the password: ");
  __isoc99_scanf("%16s", &buffer);
  for ( i = 0; i <= 15; ++i )
    *(i + 134520912) = complex_function(*(i + 134520912), 15 - i);
  if ( check_equals_AUPDNNPROEZRJWKB(&buffer, 16) )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
BOOL __cdecl check_equals_AUPDNNPROEZRJWKB(int a1, unsigned int a2)
{
  int v3; // [esp+8h] [ebp-8h]
  unsigned int i; // [esp+Ch] [ebp-4h]
 
  v3 = 0;
  for ( i = 0; i < a2; ++i )
  {
    if ( *(i + a1) == *(i + 134520896) )
      ++v3;
  }
  return v3 == a2;
}
```

一把梭不太行。缓解“路径爆炸”方法：在check函数前结束，然后手动为求解器添加条件。

```python
import angr,claripy
project=angr.Project("./08_angr_constraints",auto_load_libs=False)
state=project.factory.blank_state(addr=0x08048625)
pwd=claripy.BVS("pwd",16*8)
state.memory.store(0x0804A050,pwd)
sim=project.factory.simgr(state)
sim.explore(find=0x08048565) #check_equals_AUPDNNPROEZRJWKB的第一行指令
if sim.found:
    res=sim.found[0]
    now_str=state.memory.load(0x0804A050,16) #获取buffer
    res.solver.add("AUPDNNPROEZRJWKB"==now_str)
    print(res.solver.eval(pwd))
```

## 09_angr_hooks

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  BOOL v3; // eax
  int i; // [esp+8h] [ebp-10h]
  int j; // [esp+Ch] [ebp-Ch]
 
  qmemcpy(&password, "XYMKBKUHNIQYNQXE", 16);
  memset(&buffer, 0, 17);
  printf("Enter the password: ");
  __isoc99_scanf("%16s", &buffer);
  for ( i = 0; i <= 15; ++i )
    *(_BYTE *)(i + 134520916) = complex_function(*(char *)(i + 134520916), 18 - i);
  equals = check_equals_XYMKBKUHNIQYNQXE(&buffer, 16);
  for ( j = 0; j <= 15; ++j )
    *(_BYTE *)(j + 134520900) = complex_function(*(char *)(j + 134520900), j + 9);
  __isoc99_scanf("%16s", &buffer);
  v3 = equals && !strncmp(&buffer, &password, 16);
  equals = v3;
  if ( v3 )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

还是“路径爆炸”，第二个方法：对函数进行钩取。

```python
import angr
import claripy
project=angr.Project("./09_angr_hooks",auto_load_libs=False)
state=project.factory.entry_state()
@project.hook(0x080486B3, length=5) #call指令地址 call长度为5
def skip_check(state):
    compare_str="XYMKBKUHNIQYNQXE"
    now_str=state.memory.load(0x0804A054,16)
    state.regs.eax=claripy.If(compare_str==now_str,claripy.BVV(1, 32),claripy.BVV(0, 32)) #相等为1否则0
sim=project.factory.simgr(state)
sim.explore(find=0x08048768)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))
```

## 10_angr_simprocedures

check_equals_ORSDDWXHZURJRBDH函数的调用次数过多，不能逐个地址钩取。

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+20h] [ebp-28h]
  char v5[17]; // [esp+2Bh] [ebp-1Dh] BYREF
  unsigned int v6; // [esp+3Ch] [ebp-Ch]
 
  v6 = __readgsdword(0x14u);
  memcpy(&password, "ORSDDWXHZURJRBDH", 16);
  memset(v5, 0, sizeof(v5));
  printf("Enter the password: ");
  __isoc99_scanf("%16s", v5);
  for ( i = 0; i <= 15; ++i )
    v5[i] = complex_function(v5[i], 18 - i);
  if ( check_equals_ORSDDWXHZURJRBDH(v5, 16) )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

函数钩取方式：

```python
import angr
import claripy
project=angr.Project("./10_angr_simprocedures",auto_load_libs=False)
state=project.factory.entry_state()
class ReplaceCmp(angr.SimProcedure):
    def run(self,arg1,arg2):
        cmp_str="ORSDDWXHZURJRBDH"
        input_str=self.state.memory.load(arg1,arg2)
        return claripy.If(cmp_str==input_str,claripy.BVV(1,32),claripy.BVV(0,32))
project.hook_symbol("check_equals_ORSDDWXHZURJRBDH", ReplaceCmp())
#...
```

## 11_angr_sim_scanf

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+20h] [ebp-28h]
  char v6[20]; // [esp+28h] [ebp-20h] BYREF
  unsigned int v7; // [esp+3Ch] [ebp-Ch]
 
  v7 = __readgsdword(0x14u);
  print_msg();
  memset(v6, 0, sizeof(v6));
  qmemcpy(v6, "DCLUESMR", 8);
  for ( i = 0; i <= 7; ++i )
    v6[i] = complex_function(v6[i], i);
  printf("Enter the password: ");
  __isoc99_scanf("%u %u", &buffer0, &buffer1);
  if ( !strncmp(&buffer0, v6, 4) && !strncmp(&buffer1, &v6[4], 4) )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

一把梭就行：

```python
import angr
project=angr.Project("./11_angr_sim_scanf",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state)
sim.explore(find=0x0804FCA1)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))
    #b'1146242628 1296386129'
```

## 12_angr_veritesting

函数某部分引发“路径爆炸”，其他部分在做必要的运算。

```c++
// bad sp value at call has been detected, the output may be wrong!
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v3; // ebx
  int v5; // [esp-10h] [ebp-5Ch]
  int v6; // [esp-Ch] [ebp-58h]
  int v7; // [esp-8h] [ebp-54h]
  int v8; // [esp-4h] [ebp-50h]
  const char **v9; // [esp+0h] [ebp-4Ch]
  int v10; // [esp+4h] [ebp-48h]
  int v11; // [esp+8h] [ebp-44h]
  int v12; // [esp+Ch] [ebp-40h]
  int v13; // [esp+10h] [ebp-3Ch]
  int v14; // [esp+10h] [ebp-3Ch]
  int v15; // [esp+14h] [ebp-38h]
  int i; // [esp+14h] [ebp-38h]
  int v17; // [esp+18h] [ebp-34h]
  int v18[9]; // [esp+1Ch] [ebp-30h] BYREF
  unsigned int v19; // [esp+40h] [ebp-Ch]
  int *p_argc; // [esp+44h] [ebp-8h]
 
  p_argc = &argc;
  v9 = argv;
  v19 = __readgsdword(0x14u);
  print_msg();
  memset(
    v18 + 3,
    0,
    33,
    v5,
    v6,
    v7,
    v8,
    v9,
    v10,
    v11,
    v12,
    v13,
    v15,
    v17,
    v18[0],
    v18[1],
    v18[2],
    v18[3],
    v18[4],
    v18[5]);
  printf("Enter the password: ");
  __isoc99_scanf("%32s", v18 + 3);
  v14 = 0;
  for ( i = 0; i <= 31; ++i )
  {
    v3 = *(v18 + i + 3);
    if ( v3 == complex_function(87, i + 186) )
      ++v14;
  }
  if ( v14 != 32 || v19 )
    puts("Try again.");
  else
    puts("Good Job.");
  return 0;
}
```

Veritesting算法：让符号执行在动态符号执行DSE和静态符号执行SSE之间协同工作。

```python
import angr
project=angr.Project("./12_angr_veritesting",auto_load_libs=False)
state=project.factory.entry_state()
sim=project.factory.simgr(state,veritesting=True) #添加一个参数即可
sim.explore(find=0x08048684)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))
b'CXSNIDYTOJEZUPKFAVQLGBWRMHCXSNID'
```

## 13_angr_static_binary

静态编译生成的二进制文件。

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [esp+1Ch] [ebp-3Ch]
  int j; // [esp+20h] [ebp-38h]
  char v6[20]; // [esp+24h] [ebp-34h] BYREF
  char v7[ 20]; // [esp+38h] [ebp-20h] BYREF
  unsigned int v8; // [esp+4Ch] [ebp-Ch]
 
  v8 = __readgsdword(0x14u);
  print_msg();
  for ( i = 0; i <= 19; ++i )
    v7[i] = 0;
  qmemcpy(v7, "LJVNEPAU", 8);
  printf("Enter the password: ");
  _isoc99_scanf("%8s", v6);
  for ( j = 0; j <= 7; ++j )
    v6[j] = complex_function(v6[j], j);
  if ( !strcmp(v6, v7) )
    puts("Good Job.");
  else
    puts("Try again.");
  return 0;
}
```

Angr在库函数装载时钩取这些函数，静态编译没有这个过程，会被主动分析。所以要钩取静态编译生成的库函数。Angr内置了多个库函数，需要手动钩取。

```python
import angr
project=angr.Project("./13_angr_static_binary",auto_load_libs=False)
state=project.factory.entry_state()
project.hook(0x0804ED40,angr.SIM_PROCEDURES['libc']['printf']())
project.hook(0x0804ED80,angr.SIM_PROCEDURES['libc']['scanf']())
project.hook(0x0804F350,angr.SIM_PROCEDURES['libc']['puts']())
project.hook(0x08048D10,angr.SIM_PROCEDURES['glibc']['__libc_start_main']())
project.hook(0x0805B450,angr.SIM_PROCEDURES['libc']['strcmp']())
sim=project.factory.simgr(state,veritesting=True)
sim.explore(find=0x080489E1)
if sim.found:
    res=sim.found[0]
    print(res.posix.dumps(0))#LYZGMMMV
```

