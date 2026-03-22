---
title : '苍穹外卖前端开发(Day2)'
date : 2024-10-27T22:30:13+08:00
lastmod: 2024-10-27T22:20:13+08:00
description : "苍穹外卖前端开发(Day2)"  
categories : ["Java后端"]
tags : ["JavaWeb项目-苍穹外卖"]
---

# 苍穹外卖前端开发(Day2)

## 前端环境搭建

- 通过登录功能梳理前端代码

通过[NVM](https://github.com/coreybutler/nvm-windows/releases)管理node版本

### 安装nvm

```shell
nvm -v
```

![image-20241027210334721](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241027210334721.png)

查看所有已经安装的nvm

```shell
nvm list
```

查看可以可获取的node版本

```shell
nvm ls available
```

![image-20241027210500999](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241027210502746.png)

在项目中执行

```shell
npm install 
```

![image-20241027210605527](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241027210605527.png)

使用特定版本的node 

```shell
nvm use 16
```

### 前端调用过程

通过登录功能梳理前端代码

1. 获得登录页面路由地址
2. 从 `main.ts` 中找到路由文件
3. 从路由文件中找到登录视图组件
4. 从登录视图组件中找到登录方法
5. 跟踪登录方法的执行过程

## 员工分页查询



## 启用禁用员工账号



## 修改员工



## 套餐管理



删除套餐
