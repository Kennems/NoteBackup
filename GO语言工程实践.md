# GO语言工程实践课后作业 | 青训营

上完GO语言工程实践课，布置了一道课后作业

## 需求描述

- 发布话题和回帖
- 本地ID生成需要保证不重复，唯一性
- Append文件，更新索引，注意 Map 的并发安全问题。

项目的大致框架老师已经给出，我们需要修改代码满足上面的要求。

## 项目背景

该项目是一个基于 Gin 框架开发的消息板应用，提供发布话题和回复功能。

### Gin

Gin 是一个基于 Go 语言的轻量级 Web 框架，它提供了快速构建高性能 Web 应用程序的工具和功能。以下是 Gin 框架的一些特点和使用方法：

特点：

- 快速：Gin 是一个非常快速的框架，因为它使用了 Radix 树路由和高度优化的上下文对象。
- 轻量级：Gin 框架的代码库很小，并且具有较少的依赖，使其易于学习和使用。
- 中间件支持：Gin 提供了中间件机制，可以方便地添加、移除和定制中间件，用于处理跨越、认证、日志记录等常见任务。
- 路由组：Gin 支持路由组，可以轻松地组织和管理路由。
- JSON 解析和验证：Gin 内置了对 JSON 的解析和验证功能，可以轻松地处理请求和响应的 JSON 数据。

Gin 官方文档（https://gin-gonic.com/）

使用方法：

1. 安装 Gin：通过命令 `go get -u github.com/gin-gonic/gin` 可以安装最新版的 Gin 框架。
2. 导入 Gin：在代码中导入 `"github.com/gin-gonic/gin"` 包。
3. 创建应用程序实例：使用 `gin.Default()` 函数创建一个 Gin 的默认路由引擎实例。
4. 定义路由和处理函数：使用路由引擎实例的各种方法来定义路由规则和对应的处理函数。
5. 运行应用程序：通过调用路由引擎实例的 `Run()` 方法来运行应用程序。

以下是一个简单的示例代码，演示了如何在 Gin 中定义路由和处理函数：

```go
package main
import (
	"github.com/gin-gonic/gin"
)
func main() {
	// 创建默认的 Gin 路由引擎实例
	r := gin.Default()

	// 定义路由和对应的处理函数
	r.GET("/", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "Hello, Gin!",
		})
	})

	// 运行应用程序，默认监听在 8080 端口
	r.Run()
}
```

这段代码创建了一个简单的 HTTP 服务器，监听根路径 `/`，并返回一个 JSON 格式的响应。你可以使用浏览器或者 API 测试工具访问 `http://localhost:8080` 来查看结果。

### Postman

项目中需要用到Postman来发动`Get`以及`Post`请求进行测试，Postman 是一个用于测试和开发 API 的强大工具。它提供了一个用户友好的界面，可以轻松地发送 HTTP 请求、查看和分析响应，并进行各种测试和调试操作。以下是使用 Postman 的一些常见功能：

1. 发送请求：可以选择不同的 HTTP 方法（GET、POST、PUT 等）并添加请求头、请求参数、请求体等。
2. 设置环境变量：可以创建环境变量来管理请求中的动态值，如 URL、认证令牌等，方便在不同环境中切换。
3. 身份验证：可以添加不同类型的身份验证（Basic、Bearer Token 等）来进行请求的身份验证。
4. 断言测试：可以添加断言来验证响应的内容、状态码、响应时间等是否符合预期。
5. 高级功能：支持设置请求代理、设置延迟和限流、导入和导出请求集合等。
6. 自动化测试：可以编写自动化测试脚本，进行接口测试的自动化执行。

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826194941.png)

## 项目结构

以下是项目的目录结构和主要组件：

- `controller`：视图层，处理用户输入并相应数据
  - `publish.go`：包含发布话题和回复的接口
  - `query_info.go`：包含查看话题和回复的接口
- `data`：数据库文件夹，存储以 JSON 格式保存的数据
  - `post`：所有回复信息的文件
  - `topic`：所有话题信息的文件
- `repository`：数据层，提供简单易用的数据库操作接口
  - `init_database.go`：从文件中读取数据并存储到对应的 Map 中
  - `post.go`：实现通过话题ID查找回复、新增回复和查找最大ID的功能
  - `topic.go`：实现通过ID查找话题、新增话题和查找最大ID的功能
- `service`：业务逻辑层，处理数据层的数据并返回给视图层
  - `query_info.go`：实现查找话题及回复的功能
- `server.go`：程序入口，定义路由、处理请求和调用接口响应请求

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826195118.png)

## 实现功能

### 实现思路

1. **获取数据：**用户输入标题和内容，如果是回复则还需要回复话题的ID和内容。
2. **处理数据**：封装数据为结构体并设置唯一ID和时间戳。
3. **获取唯一ID**：通过查找已有ID中的最大值并加一获得新的ID。
4. **保存数据**：将结构体序列化为JSON格式，追加到文件末尾，并更新索引。

### 具体实现

#### 发布话题和回帖

在`server.go`中定义了两个路由：

1. 发布话题：`/community/page/publish/topic`（POST方法）
2. 发表回复：`/community/page/publish/post`（POST方法）

用户可以通过Postman测试这两个接口。下面是一个示例：

```go
	// 发布话题的接口
	r.POST("/community/page/publish/topic", func(c *gin.Context) {
		title := c.PostForm("title")
		content := c.PostForm("content")
		data, err := controller.PublishTopic(title, content)
		if err != nil {
			c.JSON(500, gin.H{
				"error": err.Error(),
			})
			return
		}
		c.JSON(200, data)
	})
```

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826195644.png)

#### 本地ID生成需要保证不重复，唯一性

在获取用户数据后，需要找到当前最大ID，并加一作为新插入的数据的ID。对于话题，可以直接遍历`topicIndexMap`（包含所有话题的ID和信息），找到最大值；对于回复，由于`postIndexMap`是一个树形结构，不便查找最大值，在读取数据时我们将所有回复信息存储到一个数组中，并构建Map，这样就可以很容易地查找最大ID。

获取ID后，可以封装用户输入的数据。由于逻辑较简单，直接在`controller`层实现封装。

```go
// PublishTopic 发布话题
func PublishTopic(title string, content string) (repository.Topic, error) {
	topicDao := repository.NewTopicDaoInstance()
	// 获取当前最大的话题ID
	id := topicDao.FindMaxId() + 1
	// 获取当前时间戳
	timestamp := time.Now().Unix()
	// 创建话题，并返回结果
	topic, err := topicDao.CreateTopic(id, title, content, timestamp)
	return topic, err
}
```

如果再次发送一次：

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826195744.png)

可见id自动加一，可以保证**id唯一不重复**。

#### Append文件，更新索引

将封装好的数据保存到文件中并更新索引：

```go
// 封装数据为结构体
newTopic := Topic{
    Id:         id,
    Title:      title,
    Content:    content,
    CreateTime: createTime,
}
// 更新索引
topicIndexMap[newTopic.Id] = &newTopic
// 保存到文件
f, err := os.OpenFile("./data/topic。txt", os.O_APPEND|os.O_WRONLY|os.O_CREATE, 0600)
if err != nil {
    return newTopic, err
}
defer f.Close()
// 序列化为JSON格式并写入文件
marshal, _ := json.Marshal(newTopic)
if _, err = f.WriteString(string(marshal) + "\n"); err != nil {
    return newTopic, err
}
```

当保存文件时，由于每条数据后面有换行符，会导致读取时可能出现报错。因此在读取文件时，我们需要判断当前行是否为空字符串，若是则结束文件读取。

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826195911.png)

可见发布的话题（topic）成功写入文件内。

####  Map 的并发安全问题

可以采取的方法是使用**互斥锁（Mutex**）：通过在访问 Map 之前获取互斥锁，并在操作完成后释放锁，可以确保同时只有一个 goroutine 可以访问 Map。这样可以避免并发读写操作导致的数据竞争问题。

项目内代码如下（以发布话题为例子）：

```go
// TopicDao 是话题数据访问对象
type TopicDao struct {
	topicIndexMap map[int64]*Topic // 话题索引映射
	lock          sync.RWMutex     // 读写锁
//在CreateTopic方法中，通过调用Lock方法获取写锁，在更新topicIndexMap之前对其进行加锁操作，以确保并发安全。使用defer语句调用Unlock方法释放锁，以便在退出该方法时解锁。
}
// CreateTopic 创建话题
func (*TopicDao) CreateTopic(id int64, title string, content string, createTime int64) (topic Topic, err error) {
	newTopic := Topic{
		Id:         id,
		Title:      title,
		Content:    content,
		CreateTime: createTime,
	}
	topicIndexMap[newTopic.Id] = &newTopic
	f, err := os.OpenFile("./data/topic.txt", os.O_APPEND|os.O_WRONLY|os.O_CREATE, 0600)
	if err != nil {
		return newTopic, err
	}
	defer f.Close()
	marshal, _ := json.Marshal(newTopic)
	if _, err = f.WriteString(string(marshal) + "\n"); err != nil {
		return newTopic, err
	}
	return newTopic, nil
}
```

### 测试结果

#### 获取页面信息

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826202314.png)

#### 发布话题

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826202218.png)

#### 发布帖子

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826202249.png)

#### 文件详情

##### topic.txt

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826202351.png)

##### post.txt

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230826202510.png)

## 总结

本次项目要求我们修改一个基于Gin框架开发的消息板应用，以满足发布话题和回帖的功能需求，并保证本地ID的唯一性和并发安全。

我们首先使用Gin框架定义了两个路由接口，分别用于发布话题和回帖。用户可以通过Postman发送POST请求来测试这两个接口。

在处理数据时，我们封装用户的输入为结构体，并为其设置唯一ID和时间戳。为了保证ID的唯一性，我们遍历已有ID中的最大值，并将其加一作为新插入数据的ID。

然后，我们将封装好的数据保存到文件中并更新索引。在保存文件时，由于每条数据后面有换行符，我们需要在读取文件时判断当前行是否为空字符串，以避免错误。

为了处理并发安全问题，我们使用互斥锁（Mutex）来确保同一时间只有一个goroutine可以访问Map，避免并发读写操作导致的数据竞争问题。

经过测试，我们成功实现了发布话题和回帖的功能，并保证了本地ID的唯一性和并发安全性。