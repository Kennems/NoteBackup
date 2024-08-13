# Gin框架

下载gin

```bash
 go get -u github.com/gin-gonic/gin
```

下载安装postman

## helloworld

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()

	router.GET("/index", func(context *gin.Context) {
		context.String(200, "hello world?")
	})

	router.Run(":8080")
}
```

## 响应

```go
func main() {
	router := gin.Default()
	router.GET("/", func(c *gin.Context) {
		//状态码：
		//200正常相应

		c.String(http.StatusOK, "你好腌")
	})
	router.Run(":80")
}
```

### 响应json

```go
	//json响应结构体
	type UserInfo struct {
		UserName string `json:"user_name"`
		Age      int    `json:"age"`
		password string `json:"-"` //忽略转换为json
	}
	user := UserInfo{"name", 19, "123456"}

	//json响应map
	userMap := map[string]string{
		"user_name": "hehe",
		"age":       "89",
	}
	c.JSON(http.StatusOK, user)
	c.JSON(http.StatusOK, userMap)

	//直接响应json
	c.JSON(http.StatusOK, gin.H{"username": "123", "age": 456})
```

### 响应xml和yaml

返回xml

```go
router.GET("/xml", func(c *gin.Context) {
  c.XML(http.StatusOK, gin.H{"user": "hanru", "message": "hey", "status": http.StatusOK})
})CopyErrorOK!
```

返回yaml

```go
router.GET("/yaml", func(c *gin.Context) {
  c.YAML(http.StatusOK, gin.H{"user": "hanru", "message": "hey", "status": http.StatusOK})
})
```

响应`html`

```go
func _html(c *gin.Context) {
	c.HTML(http.StatusOK, "index.html", gin.H{})
}
```

```go
	router.LoadHTMLGlob("templates/*")
	router.GET("/html", _html)
```

### 文件响应

```go
	//在golang中没有相对文件的路径，只有相对项目的路径
	//网页请求这个静态目录的前缀，第二个参数是一个目录，前缀不能重复
	router.StaticFile("./static/img.png", "./static/img.png")
	//配置单个文件，网页请求的路由，文件的路径
	router.StaticFS("/static", http.Dir("/static/static"))
```

由：`http://127.0.0.1/static/img.png`来响应。

### 重定向

状态码有301和302

301的旧地址被永久删除（不能再访问了），搜索引擎在抓取到新的内容后会用新的url替换旧url，302的旧地址还在（访问时只是暂时转移到新地址），搜索引擎会抓取新的内容后保存旧的url。

```go
    func _redirect(c *gin.Context) {
        c.Redirect(301, "http://www.baidu.com")
    }	
	router.GET("/baidu", _redirect)
```

## 请求

### 查询参数qurey

/query?user=xxx&user=xxx

```go
func _query(c *gin.Context) {
	fmt.Println(c.Query("user"))
	fmt.Println(c.GetQuery("user"))
	fmt.Println(c.QueryArray("user")) //拿到多个相同的查询参数
}
```

### 动态参数param

/param/1/2

```go
func _param(c *gin.Context) {
	fmt.Println(c.Param("user_id"))
	fmt.Println(c.Param("book_id"))
}
	router.GET("/param/:user_id", _param)
	router.GET("/param/:user_id/:book_id", _param)
```

### 表单postForm

可以接收 `multipart/form-data; `和`application/x-www-form-urlencoded`

```go
func _form(c *gin.Context) {
	fmt.Println(c.PostForm("name"))
	fmt.Println(c.PostFormArray("name"))
	fmt.Println(c.DefaultPostForm("addr", "usa"))
	forms, err := c.MultipartForm() //接受所有的form参数
	fmt.Println(forms, err)
}
```

### 原始参数getrawdata

```go
func _raw(c *gin.Context) {
	body, _ := c.GetRawData()
	fmt.Println(string(body))
}
	router.POST("/raw", _raw)
```

