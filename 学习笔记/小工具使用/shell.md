---
title : 'shell'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "for i in {1..10};"
image : img/cat.jpg
draft : false
categories : ["小工具使用"]
tags : ["学习笔记", "小工具使用"]
---
# shell

```shell
for i in {1..10}; 
do 
	echo "$i"
done
```



```shell
for file in *.txt;
do 
	echo "Processing $file"
	wc -l "$file"
done

```

