# GORM实践 | 青训营

GORM（Go Object Relational Mapping）是一个在Go语言中使用的对象关系映射库，它提供了一种简洁、灵活的方式来操作数据库。GORM支持多种关系型数据库，如MySQL、PostgreSQL、SQLite和SQL Server等。

GORM的特点包括：

1. 简单易用：GORM提供了简洁直观的API，使得数据库操作变得简单易用。
2. ORM功能：GORM通过结构体和字段映射的方式，将数据表映射为Go语言的结构体，提供了CRUD（创建、读取、更新、删除）等常用的ORM功能。
3. 关联关系处理：GORM支持处理多种关联关系，包括一对一、一对多和多对多等。
4. 事务支持：GORM提供了事务支持，可以确保数据库操作的原子性和一致性。
5. 钩子函数：GORM支持定义钩子函数，在执行数据库操作前后进行自定义逻辑的处理。
6. 数据验证：GORM提供了数据验证功能，可以在保存或更新数据前对数据进行验证。
7. 自动迁移：GORM可以自动创建数据库表格，并根据数据模型的变化进行数据库迁移。

使用GORM时，首先需要创建一个数据库连接，然后定义结构体来表示数据库表格和字段，并使用GORM提供的API进行数据库操作。

今天我们从GORM的增删改查四个方面来实践。

## 1、连接数据库

连接数据库的过程可以分为三个步骤：准备工作、连接数据库和其他配置。

### 1.1、准备工作：下载驱动

[官方连接数据库教程](https://gorm.cn/zh_CN/docs/connecting_to_the_database.html)

GORM 官方支持的数据库类型有： MySQL, PostgreSQL, SQlite, SQL Server，我这里使用的`MySql`，首先需要下载需要的驱动。其他数据库根据需要的驱动进行下载。

```bash
go get gorm.io/driver/mysql
go get gorm.io/gorm
```

### 1.2、连接数据库

#### 配置`dsn`

`dsn`代表数据库连接字符串（Data Source Name），它用于指定连接到数据库的详细信息。`dsn`通常包含以下信息：

1. 数据库类型：比如`mysql`表示MySQL数据库。
2. 用户名和密码：用于进行身份验证和授权访问数据库。
3. 主机和端口：指定数据库服务器的主机地址和端口号。
4. 数据库名称：指定要连接的具体数据库实例名称。

需要在`mysql`中建立一个名叫`gorm`的数据库，即`dsn`中的`Dbname`字段。

```go
package main

import (
	"fmt"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"gorm.io/gorm/logger"
)
var DB *gorm.DB
func init() {
	username := "root"         //账号
	password := "xxxx"         //密码,此处填写你的数据库密码
	host := "127.0.0.1"        //数据库地址，可以是Ip或者域名
	port := 3306               //数据库端口
	Dbname := "gorm"           //数据库名
	timeout := "10s"           //连接超时，10秒

	var mysqlLogger logger.Interface

	mysqlLogger = logger.Default.LogMode(logger.Info)

	// root:root@tcp(127.0.0.1:3306)/gorm?
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?charset=utf8mb4&parseTime=True&loc=Local&timeout=%s", username, password, host, port, Dbname, timeout)
	//连接MYSQL, 获得DB类型实例，用于后面的数据库读写操作。
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
		SkipDefaultTransaction: true,
		//NamingStrategy: schema.NamingStrategy{
		//	//TablePrefix:
		//	SingularTable: false,
		//	NoLowerCase:   false,
		//},
		Logger: mysqlLogger,
	})
	if err != nil {
		panic("连接数据库失败, error=" + err.Error())
	}
	// 连接成功，输出连接成功信息以及连接的数据库对象信息
	fmt.Println("连接数据库成功,连接的数据库对象:", db)
	DB = db
}

func main() {

}
```

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825163555.png)

### 1.3、其他配置

#### 1、跳过默认事务

跳过默认开启事务模式。

```go
db, err := gorm.Open(mysql.Open("gorm.db"), &gorm.Config{
  SkipDefaultTransaction: true,
})
```

#### 2、修改GORM的命名策略

可以修改如图的配置信息。

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825164649.png)

```go
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
  NamingStrategy: schema.NamingStrategy{
    TablePrefix:   "test_",  // 表名前缀
    SingularTable: false, // 单数表名
    NoLowerCase:   false, // 关闭小写转换
  },
})
```

#### 3、显示日志

```go
var mysqlLogger logger.Interface
// 要显示的日志等级
mysqlLogger = logger.Default.LogMode(logger.Info)
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
  Logger: mysqlLogger, //配置显示日志信息
})
```

并且可以自定义配置日志：

```go
// 创建一个新的日志记录器
newLogger := logger.New(
	log.New(os.Stdout, "\r\n", log.LstdFlags), // 日志输出的目标，这里使用标准输出，并添加换行符和时间戳
	logger.Config{
		SlowThreshold:             time.Second,     // 慢 SQL 阈值，超过该阈值的 SQL 会被认为是慢查询
		LogLevel:                  logger.Info,     // 日志级别，这里设置为 Info，会记录 Info、Warn、Error 级别的日志
		IgnoreRecordNotFoundError: true,            // 忽略 ErrRecordNotFound（记录未找到）错误，不会将其记录为错误日志
		Colorful:                  true,            // 使用彩色打印，日志信息会使用不同的颜色进行区分显示
	},
)
// 打开数据库连接
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{
	Logger: newLogger, // 设置日志记录器为新创建的日志记录器
})

```

## 2、创建表

我在示例代码中使用了默认的MySQL连接配置，需要将`root:password@tcp(127.0.0.1:3306)/database`中的"root"替换为您的MySQL用户名，"password"替换为您的MySQL密码，"database"替换为您想要使用的数据库名称。

### 2.1、定义一张表

```go
type Student struct {
  ID    uint // 默认使用ID作为主键
  Name  string
}
```

### 2.2、生成表结构

```go
// 可以放多个
DB.AutoMigrate(&Student{})
```

### 2.3、修改标签

[索引标签文档](https://gorm.cn/zh_CN/docs/indexes.html)

```go
type Student struct {
  Name string      `gorm:"type:varchar(12);not null;comment:用户名"`
  UUID string      `gorm:"primaryKey;unique;comment:主键"`
  Info StudentInfo `gorm:"embedded;embeddedPrefix:s_"`
}
```

在MySql中可以看到新建的表，由于我没有修改`SingularTable`，所以会自动建成复数表名：

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825171914.png)

也可以在Goland中的Database中关联Mysql中查看**(记得刷新**)：

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825172055.png)

## 3、对单张表进行增删改查

对于数据的增删改查操作，我们可以使用以下方法：

在第二步的基础上新建一张`Student`表

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825172755.png)

### 3.1、添加数据

通过调用Create方法来添加记录，并传入对应的结构体对象。例如：

```go
func main() {
	email := "kennem@foxmail.com"
	//添加记录
	stu1 := Student{
		Name:   "悦乐",
		Age:    20,
		Gender: true,
		Email:  &email,
	}
	err := DB.Create(&stu1).Error
	fmt.Println(err)
}
```

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825173310.png)

打印输出stu1会自动带上id:

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825173634.png)

#### 批量插入

如果要批量插入数据，可以将结构体对象放入切片中，然后调用Create方法。例如：

```go
	var studentList []Student

	for i := 0; i < 10; i++ {
		studentList = append(studentList, Student{
			Name:   fmt.Sprintf("测试%d", i*2+1),
			Age:    i*2 + 1,
			Gender: true,
			Email:  &email,
		})
	}

	err := DB.Create(&studentList).Error
	fmt.Println(err)
```

传入切片，可以批量插入数据：

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825174416.png)

### 3.2、数据查询

进行数据查询时，可以使用Take方法获取单条记录，也可以使用Find方法获取多条记录。

```go
	//单表记录查询
	var student1 Student
	var student2 Student
	var student3 Student

	DB.Take(&student1)
	DB.Take(&student2, 4)                 //根据id查询
	DB.Take(&student3, "name = ?", "测试3") //根据属性值查询

	fmt.Println(student1)
	fmt.Println(student2)
	fmt.Println(student3)
```

查询结果：

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825175238.png)

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825175646.png)

或者可以修改对象的主键属性值：

```go
	var student2 Student
	student2.ID = 4
```

#### 获取查询结果

```go
	count := DB.Find(&student1).RowAffected
	err := DB.Find(&student1).Error
	fmt.Println(count, err)
```

#### 查询多条记录

使用Find方法查询多条记录，并将结果存储在切片中：

用`json`输出多条查询数据

```go
	//查询多条记录
	var studentList []Student

	count := DB.Find(&studentList).RowsAffected
	fmt.Println(count)

	data, _ := json.Marshal(studentList)
	fmt.Println(string(data))
```

#### 根据主键列表

```go
	var studentList []Student
	DB.Find(&studentList, []int{1, 2, 3})
	fmt.Println(studentList)

	var studentList []Student
	DB.Find(&studentList, []int{1, 2, 3})
	DB.Find(&studentList, "name in (?)", []string{"测试1"})
	fmt.Println(studentList)
```

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825193700.png)

### 3.3、数据修改

数据修改可以使用Save方法进行单个记录的更新，也可以使用Update和Updates方法进行批量更新。

#### save更新

可以修改name和age，用selecte可以选择特定属性更新。

```go
	//save更新
	var student Student
	DB.Take(&student, 3)
	fmt.Println(student)
	student.Name = "测试测试测试"
	student.Age = 0
	DB.Select("name").Save(&student)
```

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825200822.png)

#### 批量更新

```go
	//批量更新
	var studentList []Student
	DB.Find(&studentList, []int{4, 5, 8}).Update("gender", false)
```

#### 更新多列

```go
	//updates更新
	var studentList []Student
	DB.Find(&studentList, []int{4, 5, 8}).Updates(Student{Age: 100, Gender: true})
```

或者

```go
	var studentList []Student
	DB.Find(&studentList, []int{4, 5, 8}).Updates(map[string]any{
		"name": "haha",
	})
```

修改结果：

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825202349.png)

### 3.4、数据删除

删除操作可以使用Delete方法进行单个记录的删除。

```go
	//删除
	var student Student
	DB.Delete(&student, 8)

	//先查找再删除
	DB.Take(&student)
	DB.Delete(&student)
```

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230825204813.png)

## 总结

GORM是一个Go语言的ORM库，用于简化数据库操作。在使用GORM之前，首先需要连接数据库。

连接数据库的步骤如下：

1. 安装数据库驱动：根据使用的数据库类型，下载对应的驱动包。
2. 配置数据库连接信息：设置数据库类型、用户名、密码、主机和端口、数据库名称等信息。
3. 使用`gorm.Open`方法连接数据库并获取数据库对象。

连接成功后，可以进行其他配置，如跳过默认事务、修改GORM的命名策略、显示日志等。

创建表的步骤如下：

1. 定义表结构体：使用Go语言定义一个结构体来表示数据库表，结构体的字段可以对应表的列。
2. 调用`DB.AutoMigrate`方法自动生成表结构：在`main()`函数中调用该方法，传入表结构体作为参数，即可自动创建表。

对单张表进行增删改查的步骤如下：

1. 添加数据：使用`DB.Create`方法可以向表中添加一条记录，也可以使用切片批量插入数据。
2. 数据查询：使用`DB.Take`方法可以查询一条记录，可以根据ID或条件进行查询，使用`DB.Find`方法可以查询多条记录，还可以根据ID列表或条件进行查询。
3. 数据修改：使用`DB.Save`方法可以更新单条记录，也可以使用`DB.Updates`方法批量更新记录，可以选择特定属性进行更新。
4. 数据删除：使用`DB.Delete`方法可以删除单条记录，也可以先进行查询再删除。

以上就是使用GORM进行数据库操作的实践流程。
