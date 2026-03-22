---
title : 'ruoyi框架开发(Day1)'
date : 2024-10-29T22:30:13+08:00
lastmod: 2024-10-29T22:20:13+08:00
description : "ruoyi框架开发(Day1)"  
categories : ["Java后端"]
tags : ["JavaWeb框架-ruoyi"]
---

# ruoyi框架开发(Day1)

## 环境搭建

- MySql
- Redis
- 前端
- 后端

## 入门项目



## 功能详解

### 权限控制

- 若依内置了强大的权限控制系统，为企业级项目提供了通用的解决方案

- RBAC（基于角色的访问控制）是一种广泛使用的访问控制模型，通过角色来分配和管理用户的菜单权限



### 创建新用户小智并关联课研人员角色，仅限课程管理和统计分析菜单访问

## 数据字典

- 若依内置的数据字典，用汉语维护系统中常见的静态数据。例如：性别，状态…



## 系统管理

## 系统监控

## 系统工具

## 定时任务

- 若依为定时任务功能提供方便友好的Web界面，实现动态管理任务

## 代码生成

- 树表是一种展示层级数据的表格，能展开折叠，清晰呈现父子关系，便于管理。

## 系统接口

- Swagger



## 项目结构

### 后端部分

#### ruoyi-admin

![image-20241030113653080](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030113653080.png)

![image-20241030115542080](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030115542080.png)

#### ruoyi-common

![image-20241030114001712](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030114001712.png)

#### ruoyi-framework

![image-20241030114435866](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030114435866.png)

#### ruoyi-generator & ruoyi-quartz

![image-20241030114936212](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030114936212.png)

#### ruoyi-system

![image-20241030115122014](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030115122014.png)



## 模块依赖关系

![image-20241030115847335](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030115847335.png)

## 前端部分

![image-20241030144735136](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030144735136.png)



![image-20241030145147915](C:\Users\Acer\AppData\Roaming\Typora\typora-user-images\image-20241030145147915.png)



## 表结构

![image-20241030145419557](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241030145419557.png)

## 源码阅读

### 后端代码分析

- BaseController : web层通用数据处理
- TableDataInfo：表格分页数据对象
- AjaxResult：操作消息提醒
- BaseEntity：Entity基类



### 权限注解

- @PreAuthorize 注解是 Spring Security 框架中用来做权限检查的。
- 它在运行方法前先验证权限，权限够就放行，不够就拦截

```java
    /**
     * 查询课程管理列表
     */
    @PreAuthorize("@ss.hasPermi('course:course:list')")
    @GetMapping("/list")
    public TableDataInfo list(Course course)
    {
        startPage();
        List<Course> list = courseService.selectCourseList(course);
        return getDataTable(list);
    }
```



### 前后端交互流程



## 若依框架修改器

https://gitee.com/lpf_project/RuoYi-MT

- 若依框架修改器是一个可以一键修改RuoYi框架包名、项目名等的工具。
- 地址：
