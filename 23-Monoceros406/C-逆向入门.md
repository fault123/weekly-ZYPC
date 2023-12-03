---
title: C++逆向入门
date: 2023-10-29 17:39:35
tags: C++
---

# C++逆向入门

## std::string

### 内存布局

```c++
struct basic_string{
    char *begin_; //数据地址
    size_t size_; //字符串长度v
    union{
        size_t capacity_;
        char sso_buffer[16];
    };
};
```

### dump脚本

```python
def dbg_read_cppst_64(objectAddr):
    strPtr=idc.read_dbg_qword(objectAddr)
    result=''
    i=0
    while True:
        onebyte=idc.read_dbg_byte(strPtr+i)
        if onebyte==0:
            break
        else:
            result+=chr(onebyte)
            i+=1
    return result
```

## std::stringstream

```c++
struct stringstream{
    void *vtable1; //std::basic_stringstream
    int64 pad;
    void *vtable2; //std::basic_strinstream
    void *vtable3; //std::stringbuf
    char *_M_in_beg; //输入流开始
    char *_M_in_cur;
    char *_M_in end;
    char *_M_out_beg; //输出流开始
    char *_M_out_cur;
    char *_M_cout_end;
    //其余字段不关心
};
```

## std::vector

适用于：vector、priority_queue

### 内存布局

```c++
struct vector{
    void *start; //起始地址
    void *end; //最后元素地址
    void *max; //最大内存地址
};
```

### dump脚本

```python
def dump_vector(addr):
    ELEMENT_SIZE=8
    data_addr=[]
    vector_base=idc.read_dbg_qword(addr+0x0)
    vector_end=idc.read_dbg_qword(addr+0x8)
    for i in range(vector_base,vector_end,ELEMENT_SIZE):
        data_addr.append(i)
    return data_addr
```

## std::list

### 内存布局

```c++
struct List_node{
    List_node *next;
    List_node *prev;
    //数组区域
};
struct List_node_header{
    List_node *next;
    List_node *prev;
    size_t SIZE;
};
```

### dump脚本

```python
import idc
def dump_stl_list(p_list_header):
    data_addr=[]
    list_size=idc.read_dbg_qword(p_list_header+0x10)
    cur_node=p_list_header
    for i in range(list_size):
        cur_node=idc.read_dbg_qword(cur_node+0x0)
        data_addr.append(cur_node+0x10)
    return data_addr
```

## std::deque

适用于：deque、stack、queue

### 内存布局

```c++
struct stl_deque_iterator{
    void *cur;
    void *first;
    void *last;
    void *node;
};
struct stl_deque{
    void *map;
    size_t map_size;
    stl_deque_iterator start;
    stl_deque_iterator finish;
};
```

### dump脚本

```python
from collection import namedtuple
deque_iter=namedtuple('deque_iter',['cur','first','last','node'])
def parse_iter(addr):
    cur=idc.read_dbg_qword(addr+0x0)
    first=idc.read_dbg_qword(addr+0x8)
    last=idc.read_dbg_qword(addr+0x10)
    node=idc.read_dbg_qword(addr+0x18)
    return deque_iter(cur,first,last,node)
def dump_deque(addr):
    ELEMENT_SIZE=4
    data_addr=[]
    start_iter=parse_iter(addr+0x10)
    finish_iter=parse_iter(addr+0x30)
    buf_size=start_iter.last-start_iter.first
    map_start=start_iter.node
    map_finish=finish_iter.node
    for i in range(start_iter.cur,start_iter.last,ELEMENT_SIZE):
        data_addr.append(i)
    for i in range(finish_iter.first,finish_iter.cur,ELEMENT_SIZE):
        data_addr.append(i)
    for b in range(map_start+8,map_finish-8,8):
        buf_start=idc.read_dbg_qword(b)
        for i in range(buf_start,buf_start+buf_size,ELEMENT_SIZE):
            data_addr.append(i)
    return data_addr
```

## std::map

适用于：map、set、multimap、multiset

### 内存布局

```c++
struct std::map{
    void *allocator;
    _Rb_tree_color _M_color;
    node *root;
    node *leftmost;
    node *rightmost;
    size_t node_count;
};
struct node{
    _Rb_tree_color color;
    node *parent;
    node *left;
    node *right;
    TypeKey key;
    TypeValue value;
};
```

### dump脚本

```python
def parse_gnu_map_header(address):
    root=idc.read_dbg_qword(address+0x10)
    return root
def parse_gnu_map_node(address):
    left=idc.read_dbg_qword(address+0x10)
    right=idc.read_dbg_qword(address+0x18)
    data=address+0x20
    return left,right,data
def parse_gnu_map_travel(address):
    result=[]
    worklist=[parse_gnu_map_header(address)]
    while len(worklist)>0:
        addr=worklist.pop()
        (left,right,data)=parse_gnu_map_node(addr)
        if left>0:
            worklist.append(left)
        if right>0:
            worklist.append(right)
        result.append(data)
    return result
```



## std::unsorted_map

适用于：unsorted_map、unsorted_set

### 内存布局

```c++
struct bucket{
    bucket *next;
    valtype val;
    size_t hash;
};
struct hashmap{
    bucket *buckets;
    size_t buckets_count;
    bucket *first;
    size_t elements_count;
};
```

### dump脚本

```python
def dump_stl_hashmap(addr):
    data_addr=[]
    bucket_addr=idc.read_dbg_qword(addr+0x10)
    node_addr=bucket_addr
    while node_addr!=0:
        data_addr.append(node_addr+0x8)
        node_addr=idc.read_dbg_qword(node_addr)
    return data_addr
```

## std::shared_ptr

### 内存布局

```c++
struct Sp_counted{
    void *vt;
    int use_count;
    int weak_count;
};
struct shared_ptr{
    void *ptr;
    Sp_counted *refcount;
};
```

