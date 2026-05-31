---
title : 'Hexo博客问题记录'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "报错：fatal: unable to access 'https://github.com/esappear/hexo-theme-clover/': HTT..."
image : img/cat.jpg
draft : false
categories : ["小工具使用"]
tags : ["学习笔记", "小工具使用"]
---
# Hexo博客问题记录

git clone 

报错：fatal: unable to access 'https://github.com/esappear/hexo-theme-clover/': HTTP/2 stream 1 was not closed cleanly before end of the underlying stream

```
git config --global http.version HTTP/1.1
```

