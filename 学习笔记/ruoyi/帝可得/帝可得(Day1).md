---
title : '帝可得(Day1)'
date : 2024-11-02T22:30:13+08:00
lastmod: 2024-11-02T22:20:13+08:00
description : "帝可得(Day1)"  
categories : ["Java后端"]
tags : ["JavaWeb框架-ruoyi"]
---

# 帝可得(Day1)

## 需求说明

- 业务场景：公司计划在北京的高流量商业和居民区与潜在合作商洽谈，确定点位部署智能售货机，提供便捷的购买服务。
- 点位管理业务流程：

![image-20241102185501772](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241102185501772.png)

## 点位管理

### 库表设计

AI prompt

```
你是一位软件工程师，请帮我生成mysql的表结构：

需求如下：
1、区域表，表名为tb_region，字段有主键id， 区域名称
2、合作商表，表名为tb_partner，字段有主键id，合作商名称，联系人，联系电话，分成比例（int类型），账号，密码
3、点位表，表名tb_node，字段有主键id，点位名称，详细地址，商圈类型（int类型）

其他要求：
1、每张表都有创建时间（create_time)，修改时间（update_time)，创建人（create_by)，修改人（update_by), 备注（remark）这些字段
2、这张表的主键都是自增的
3、区域与点位是一对多的关系，合作商与点位是一对多的关系，请用字段表示出来，并建立外键约束
4、请为每个字段都添加上comment
5、帮我给生成的表插入一些北京城市相关的区域、点位、合作商的测试数据
```

```sql
-- 区域表
CREATE TABLE tb_region (
                           id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
                           region_name VARCHAR(50) NOT NULL COMMENT '区域名称',
                           create_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
                           update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
                           create_by VARCHAR(50) COMMENT '创建人',
                           update_by VARCHAR(50) COMMENT '修改人',
                           remark VARCHAR(255) COMMENT '备注'
) COMMENT='区域表';

-- 合作商表
CREATE TABLE tb_partner (
                            id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
                            partner_name VARCHAR(50) NOT NULL COMMENT '合作商名称',
                            contact_person VARCHAR(50) NOT NULL COMMENT '联系人',
                            contact_phone VARCHAR(20) NOT NULL COMMENT '联系电话',
                            split_ratio INT NOT NULL COMMENT '分成比例',
                            account VARCHAR(50) NOT NULL COMMENT '账号',
                            password VARCHAR(100) NOT NULL COMMENT '密码',
                            create_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
                            update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
                            create_by VARCHAR(50) COMMENT '创建人',
                            update_by VARCHAR(50) COMMENT '修改人',
                            remark VARCHAR(255) COMMENT '备注'
) COMMENT='合作商表';

-- 点位表
CREATE TABLE tb_node (
                         id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
                         node_name VARCHAR(50) NOT NULL COMMENT '点位名称',
                         address VARCHAR(255) NOT NULL COMMENT '详细地址',
                         business_district_type INT NOT NULL COMMENT '商圈类型',
                         region_id INT NOT NULL COMMENT '区域ID',
                         partner_id INT NOT NULL COMMENT '合作商ID',
                         create_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
                         update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
                         create_by VARCHAR(50) COMMENT '创建人',
                         update_by VARCHAR(50) COMMENT '修改人',
                         remark VARCHAR(255) COMMENT '备注',
                         FOREIGN KEY (region_id) REFERENCES tb_region(id) ON DELETE CASCADE,
                         FOREIGN KEY (partner_id) REFERENCES tb_partner(id) ON DELETE CASCADE
) COMMENT='点位表';

-- 测试数据
-- 区域数据
INSERT INTO tb_region (region_name, create_by, remark) VALUES
                                                           ('朝阳区', 'admin', '北京市朝阳区'),
                                                           ('海淀区', 'admin', '北京市海淀区'),
                                                           ('东城区', 'admin', '北京市东城区');

-- 合作商数据
INSERT INTO tb_partner (partner_name, contact_person, contact_phone, split_ratio, account, password, create_by, remark) VALUES
                                                                                                                            ('合作商A', '张三', '13800000001', 10, 'partnerA', 'passwordA', 'admin', '主要负责朝阳区的业务'),
                                                                                                                            ('合作商B', '李四', '13800000002', 15, 'partnerB', 'passwordB', 'admin', '主要负责海淀区的业务');

-- 点位数据
INSERT INTO tb_node (node_name, address, business_district_type, region_id, partner_id, create_by, remark) VALUES
                                                                                                               ('国贸点位', '北京市朝阳区建国门外大街1号', 1, 1, 1, 'admin', '朝阳区国贸商圈点位'),
                                                                                                               ('中关村点位', '北京市海淀区中关村大街59号', 2, 2, 2, 'admin', '海淀区中关村商圈点位'),
                                                                                                               ('王府井点位', '北京市东城区王府井大街88号', 1, 3, 1, 'admin', '东城区王府井商圈点位');
```

## 区域管理改造

- 实现此功能方案
  1. **同步存储**在区域表中有点位数的字段，当点位发生变化时，同步区域表中的点位数
  2. **关联查询**编写关联查询语句，在mapper层封装，区域和点位表，记录的个数都不是很多，所以我们采用关联查询这种方案

## 合作商管理改造

当`vue`只有一个input时，回车会提交表单导致页面刷新

在form中添加 `@submit.native.prevent`

```vue
    <el-form :model="queryParams" ref="queryRef" :inline="true" v-show="showSearch" label-width="68px"
             @submit.native.prevent>
      <el-form-item label="区域名称" prop="regionName">
        <el-input
            v-model="queryParams.regionName"
            placeholder="请输入区域名称"
            clearable
            @keyup.enter="handleQuery"
        />
      </el-form-item>
    </el-form>
```

### Element Plus 的表格组件隐藏过长内容

使用 show-overflow-tooltip 属性
Element Plus 的表格组件提供了 show-overflow-tooltip 属性，可以自动将超出部分的内容显示为工具提示。

```vue
      <el-table-column label="详细地址" align="center" prop="address" show-overflow-tooltip/>
```

## 点位管理改造



## 区域查看详情

