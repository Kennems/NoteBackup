---
title : 'Jupyter Notebook'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "Jupyter Notebook 有两种模式：**命令模式**（Command Mode）和 **编辑模式**（Edit Mode）。"
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# Jupyter Notebook

## 快捷键

Jupyter Notebook 有两种模式：**命令模式**（Command Mode）和 **编辑模式**（Edit Mode）。

### 命令模式快捷键（按 Esc 进入）

| 快捷键 | 功能 |
|--------|------|
| `Enter` | 进入编辑模式 |
| `Shift + Enter` | 运行当前单元格并选中下一个 |
| `Ctrl + Enter` | 运行当前单元格 |
| `Option + Enter` | 运行当前单元格并在下方插入新单元格 |
| `A` | 在当前单元格上方插入新单元格 |
| `B` | 在当前单元格下方插入新单元格 |
| `DD` | 删除当前单元格 |
| `Z` | 撤销删除单元格 |
| `X` | 剪切当前单元格 |
| `C` | 复制当前单元格 |
| `V` | 粘贴到下方 |
| `Shift + V` | 粘贴到上方 |
| `M` | 切换为 Markdown 单元格 |
| `Y` | 切换为 Code 单元格 |
| `R` | 切换为 Raw 单元格 |
| `1~6` | 设置 Markdown 标题级别 |
| `L` | 切换行号显示 |
| `O` | 切换输出显示 |
| `S` | 保存当前文件 |
| `H` | 显示快捷键帮助 |
| `II` | 中断内核 |
| `00` | 重启内核 |
| `Shift + M` | 合并选中单元格 |
| `Ctrl + Shift + -` | 在光标处分割单元格 |
| `Space` | 向下滚动 |
| `Shift + Space` | 向上滚动 |

### 编辑模式快捷键（按 Enter 进入）

| 快捷键 | 功能 |
|--------|------|
| `Esc` | 进入命令模式 |
| `Tab` | 代码补全或缩进 |
| `Shift + Tab` | 显示函数文档提示 |
| `Ctrl + /` | 注释/取消注释当前行 |
| `Ctrl + ]` | 增加缩进 |
| `Ctrl + [` | 减少缩进 |
| `Ctrl + A` | 全选 |
| `Ctrl + Z` | 撤销 |
| `Ctrl + Y` | 重做 |
| `Ctrl + Home` | 跳转到单元格开头 |
| `Ctrl + End` | 跳转到单元格末尾 |
| `Ctrl + Shift + -` | 在光标处分割单元格 |
| `Ctrl + S` | 保存（同命令模式） |

### 常用魔法命令

| 命令 | 功能 |
|------|------|
| `%run script.py` | 运行外部 Python 脚本 |
| `%timeit expr` | 多次执行并计时 |
| `%time expr` | 单次执行并计时 |
| `%%timeit` | 单元格级计时 |
| `%load file.py` | 将文件内容加载到当前单元格 |
| `%who` | 列出当前命名空间中所有变量 |
| `%whos` | 列出变量及其详细信息 |
| `%matplotlib inline` | 在笔记本中显示 matplotlib 图表 |
| `%pdb` | 异常时自动进入调试器 |
| `%env VAR=value` | 设置环境变量 |
| `!command` | 执行 Shell 命令 |
| `%debug` | 进入事后调试模式 |

## 命令

打开指定路径

```
jupyter-notebook ./
```

