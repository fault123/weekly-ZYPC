---
title: IDA脚本编程
date: 2023-10-29 16:35:00
tags: IDA
---

# IDA脚本编程

## 寄存器操作

```python
idc.get_reg_value('rax')
idaapi.set_reg_value('rax',1234)
```

### xmm寄存器

```python
def read_xmm_reg(name):
    rv=idaapi.regval_t()
    idaapi.get_reg_val(name,rv)
    return (struct.unpack('Q',rv.bytes())[0])
```

## 调试内存操作

```python
idc.read_dbg_bytes(addr)
idc.read_dbg_memory(addr,size)
idc.read_dbg_dword(addr)
idc.read_dbg_qword(addr)
idc.patch_dbg_byte(addr,val)
```

## 本地内存操作

```python
idc.get_qword(addr)
idc.patch_qword(addr,val)
idc.patch_dword(addr,val)
idc.patch_word(addr,val)
idc.patch_byte(addr,val)
idc.get_db_byte(addr)
idc.get_bytes(addr,size)
```

## 反汇编操作

```python
GetDisasm(addr) #获取反汇编文本
idc.next_head(ea) #获取下一条指令地址
```

## 交叉引用分析

```python
for ref in idautils.XrefsTo(ea):
    print(hex(ref.frm))
```

## OLLVM批量断点设置

```python
fn=0x401f60 #main函数
ollvm_tail=0x405d4b #汇聚点
f_blocks=idaapi.FlowChart(idaapi.get_func(fn),flags=idaapi.FC_PREDS)
for block in f_blocks:
    for succ in block.succs(): #后继
        if succ.start_ea==ollvm_tail:
            print(hex(block.start_ea))
            idc.add_bpt(block.start_ea)
```

## 杂项

```python
idc.add_bpt(0x409437) #添加断点
idaapi.get_imagebase() #获取基地址
idc.create_insn(addr) #c,make code
ida_funcs.add_func(addr) #p, create function
ida_bytes.create_strlit(addr) #a, 创建字符串
```

## 函数遍历

```python
for func in idautils.Functions():
    print("0x%x,%s"%(func,idc.get_func_name(func)))
```

## 基本块

### 基本块遍历

```python
fn=... #目标函数地址
f_blocks=idaapi.FlowChart(idaapi.get_func(fn),flags=idaapi.FC_PREDS)
for block in f_blocks:
    print(hex(block.start_ea))
```

### 基本块前驱

```python
for pred in block.preds():
    print(hex(pred.start_ea))
```

### 基本块后继

```python
for succ in block.succs():
    print(hex(succ.start_ea))
```

## 指令遍历

```python
for ins in idautils.FuncItems(0x401000):
    print(hex(ins))
```

## 条件断点

编写断点函数脚本，IDA底部导入该函数。

```python
def bp():
    rax=idc.get_reg_value('rax')
    return rax==16949 #当为true时停下，false不命中
```

设置普通断点，在`call rand`后设置。右键‘Edit Break...’中点击‘Condition’中‘...’。输入‘bp()’，并选择语言为Python。
