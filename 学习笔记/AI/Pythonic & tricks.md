---
title : 'Pythonic & tricks'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "print("cwd:", os.getcwd())"
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# Pythonic & tricks

```py
import os
print("cwd:", os.getcwd())
print("尝试读取 CSV 的目录:", os.path.abspath(os.path.dirname(INPUT_CSV)))
print("该目录下都有什么文件:", os.listdir(os.path.abspath(os.path.dirname(INPUT_CSV))))

```

