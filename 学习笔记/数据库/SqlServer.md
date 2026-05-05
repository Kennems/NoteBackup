# SqlServer

SQL Server 是微软开发的关系型数据库管理系统，使用 T-SQL（Transact-SQL）作为查询语言。

## 启动前的准备工作

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20240109165628.png)

确保 SQL Server 服务已在 Windows 服务中启动（`services.msc` → 找到 `SQL Server (MSSQLSERVER)`）。

## 常用管理工具

- **SQL Server Management Studio (SSMS)**：官方免费的图形化管理工具
- **Azure Data Studio**：跨平台的轻量级数据库工具
- **sqlcmd**：命令行工具

## 基本 T-SQL 操作

### 创建数据库

```sql
CREATE DATABASE MyDatabase;
```

### 创建表

```sql
CREATE TABLE Users (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(50) NOT NULL,
    Email NVARCHAR(100) UNIQUE,
    CreatedAt DATETIME DEFAULT GETDATE()
);
```

### 增删改查

```sql
-- 插入
INSERT INTO Users (Name, Email) VALUES ('John', 'john@example.com');

-- 查询
SELECT * FROM Users WHERE Name LIKE 'J%';

-- 更新
UPDATE Users SET Email = 'new@example.com' WHERE Id = 1;

-- 删除
DELETE FROM Users WHERE Id = 1;
```

### 常用系统查询

```sql
-- 查看所有数据库
SELECT name FROM sys.databases;

-- 查看当前数据库的所有表
SELECT name FROM sys.tables;

-- 查看表结构
EXEC sp_help 'Users';
```

## 参考链接

- [SQL Server 安装教程](https://www.cnblogs.com/JohnnyLei/p/17115711.html)
