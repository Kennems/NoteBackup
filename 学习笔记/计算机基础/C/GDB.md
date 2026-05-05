---
title : 'C GDB'
date : 2024-09-03T22:30:13+08:00
lastmod: 2024-09-03T22:20:13+08:00
description : "C GDB" 
draft : false    
categories : ["操作系统"]
tags : ["C"]
---

# GDB

GDB（GNU Debugger）是 Linux/Unix 下最常用的 C/C++ 调试器，可以调试运行中的程序、分析 core dump 文件。

## 编译时加入调试信息

```bash
gcc -g -o program program.c
```

`-g` 选项会将符号表和调试信息嵌入可执行文件中。

## 常用命令速查

| 命令       | 简写 | 作用                                |
| ---------- | ---- | ----------------------------------- |
| `break`    | `b`  | 设置断点，如 `b main`、`b file.c:42` |
| `run`      | `r`  | 启动程序，可带命令行参数            |
| `next`     | `n`  | 单步执行（不进入函数内部）          |
| `step`     | `s`  | 单步执行（进入函数内部）            |
| `continue` | `c`  | 继续执行到下一个断点                |
| `print`    | `p`  | 打印变量值，如 `p x`、`p *ptr`      |
| `display`  | —    | 每次暂停时自动显示表达式            |
| `backtrace`| `bt` | 查看调用栈                          |
| `frame`    | `f`  | 切换栈帧，如 `f 1`                  |
| `info`     | `i`  | 查看信息，如 `i locals`、`i break`   |
| `watch`    | —    | 监视变量变化，如 `watch x`           |
| `finish`   | —    | 执行完当前函数并返回                |
| `quit`     | `q`  | 退出 GDB                            |

## 常用场景

### 设置条件断点

```bash
break foo if x > 100
```

### 查看数组内容

```bash
p arr@10          # 打印数组前10个元素
p *arr@5          # 打印指针指向的5个元素
```

### 调试已运行的进程

```bash
gdb -p <pid>
```

### 分析 core dump

```bash
gdb program core
```

### 在 GDB 中使用 TUI（文本用户界面）

```bash
gdb -tui program
```

进入后可使用 `layout src`（源码视图）、`layout asm`（汇编视图）、`layout regs`（寄存器视图）切换布局。
