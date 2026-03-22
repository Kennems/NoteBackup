---
title : 'C 字符串'
date : 2024-09-03T22:30:13+08:00
lastmod: 2024-09-03T22:20:13+08:00
description : "C 字符串" 
draft : false    
categories : ["操作系统"]
tags : ["C"]
---

## C 字符串

指针 + 双引号定义时 ：内容不可以修改

指针 + 双引号定义时 ： 会复用

```c
#include <stdio.h>
int main(int argc, char const *argv[])
{
    char strArr[10][100] = {
        "one",
        "two",
        "three",
        "four",
        "five",
        "six",
        "seven",
        "eight",
        "nine",
        "ten"};

    for (int i = 0; i < 10; i++)
    {
        char *str = strArr[i];
        printf("%s\n", str);
    }

    char *strArr2[10] = {
        "one",
        "two",
        "three",
        "four",
        "five",
        "six",
        "seven",
        "eight",
        "nine",
        "ten"};

    for (int i = 0; i < 10; i++)
    {
        char *str = strArr2[i];
        printf("%s\n", str);
    }

    return 0; // 返回 0 表示程序正常结束
}
```

### string中常用函数

- `strlen`：获取字符串长度
- `strcat`：拼接两个字符串
- `strcpy`：复制字符串
- `strcmp`：比较两个字符串
- `strlwr`：将字符串变成小写
- `strupr`：将字符串变成大写