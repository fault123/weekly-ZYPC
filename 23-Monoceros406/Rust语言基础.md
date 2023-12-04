---
title: Rust语言基础
date: 2023-11-11 13:34:01
tags: Rust
mathjax: true
---

# Rust语言基础

## 命令行

### 输出

```rust
fn main(){
    let a=12;
    println!("a is {0}, a again is {0}",a);
    println!("{{}}");//输出大括号
}
```

### 详细输出

```rust
#[derive(Debug)]
struct Rectangle{
    width:u32,
    height:u32,
}
fn main(){
    let rect1=REctangle{width:30,height:50};
    println!("rect1 is {:?}",rect1);//替换为{:#?}时输出带格式的输出
}
```

### 命令行输入

```rust
use std::io::stdin;
fn main(){
    let mut str_buf=String::new();
    stdin().read_line(&mut str_buff).unwrap();
    printfln!("{}",str_buf);
}
```



