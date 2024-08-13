# Go学习笔记

# 0115 day1

**这是我参与「第五届青训营 」伴学笔记创作活动的第 1 天**

## 1、Go基础语言

#### 什么是go语言？

1）高性能，高并发			2）语法简单

3）丰富的标准库				4）完善的工具链

5）静态链接						6）快速编译

7）跨平台							8）垃圾回收

#### 1）搭建开发环境

1）安装Golang

2）选择一个适合的开发环境如vscode，Goland。或者用云开发环境如gitpod。

#### 2）基础语法

##### 2.1Hello World

```go
package main //基础语法
import ( //导入包
	"fmt"
)
func main(){ //main函数
	fmt.Println("Hello World")
}
```

##### 2.2变量

go语言是一门**强类型语言**，每一个变量都有自己的变量类型。常见的变量类型有：字符串型，整数，浮点型，布尔型。和c++类似。

```go
	var a = "initial" //声明变量方式 var name string = ""
	var b, c int = 1, 2
	var d = true
	var e float64
	f := float32(e) //或者使用 变量 冒号 := 等于值 常量把var改成const 常量没有确定的类型，会根据上下文来自动确定类型
	g := a + "foo"
```

##### 2.3选择、循环语句

选择语句有 if else 和 switch 语句 switch 后可以不加任何变量，循环语句只有 for 语句。

```go
if num := 9; num < 0 { // if-else语句
    fmt.Println(num, "is negative") // 打印输出
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}

for { // 无限循环
    fmt.Println("loop") // 打印输出
    break // 跳出循环
}

for j := 7; j < 9; j++ { // for循环
    fmt.Println(j) // 打印输出
}

switch { // switch语句，后面可不加任何变量
case t.Hour() < 12: // 比较t的小时部分是否小于12
    fmt.Println("It's before noon") // 打印输出
default:
    fmt.Println("It's after noon")
}
```

##### 2.4数组 map range 

数组，map,及range函数都类似Python语言。

```go
var a [5]int // 声明一个一维数组
a[4] = 100   // 给数组的第五个元素赋值为100
fmt.Println(a[4], len(a)) // 输出数组的第五个元素和数组的长度

var twoD [2][3]int // 声明一个二维数组
for i := 0; i < 2; i++ {
    for j := 0; j < 3; j++ {
        twoD[i][j] = i + j // 对数组的每个元素赋值为行索引加列索引的和
    }
}

s := make([]string, 3) // 使用 make 函数声明一个长度为3的切片
s[0] = "a"             // 给切片的第一个元素赋值为 "a"
s[1] = "b"             // 给切片的第二个元素赋值为 "b"
s[2] = "c"             // 给切片的第三个元素赋值为 "c"
fmt.Println(s[2:5])   // 输出切片从索引2到索引4的元素（不包括索引5）
fmt.Println(s[:5])    // 输出切片从索引0到索引4的元素
fmt.Println(s[2:])    // 输出切片从索引2到结束的所有元素

m := make(map[string]int) // 声明一个 map，键类型为 string，值类型为 int
m["one"] = 1              // 向 map 中添加键值对 "one": 1
m["two"] = 2              // 向 map 中添加键值对 "two": 2
fmt.Println(m)           // 输出整个 map

nums := []int{2, 3, 4} // 声明一个切片并初始化
sum := 0

for i, num := range nums { // 使用 range 遍历切片，i 为索引，num 为值
    sum += num
    if num == 2 {
        fmt.Println("index", i, "num", num) // 输出在切片中值为2的元素的索引和值
    }
}
fmt.Println(sum) // 输出切片中所有元素的和
```

##### 2.5函数

```go 
func exists(m map[string]string, k string) (v string, ok bool) { 
    //前面的括号是传入的数据及其类型，后面的括号的return的数据及其类型，写法类似c++
	v, ok = m[k]
	return v, ok
}
```

##### 2.6指针

```go
func add2ptr(n *int) { //类似c++ 
	*n += 2
}
```

##### 2.7结构体 结构方法

```go
type user struct { //结构体定义 同样类似c++
	name     string
	password string
}
func (u *user) resetPassword(password string) { //结构方法
	u.password = password
}
```

##### 2.8错误处理

go语言中习惯的错误处理方法是使用一个单独的返回值来传递错误信息。使用简单的if else 来处理错误。

```go
	u, err := findUser([]user{{"wang", "1024"}}, "wang")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(u.name)
func findUser(users []user, name string) (v *user, err error) { //函数错误处理
	for _, u := range users {
		if u.name == name {
			return &u, nil
		}
	}
	return nil, errors.New("not found") //返回错误信息
```

##### 2.9字符串操作 字符串格式化

string包里有很多实用的处理字符串的工具函数。字符串格式化有很多方法，可以用类似c语言中printf() 的方式来格式化字符串。go语言可以用%v来打印任意类型的变量，%+v打印详细结果，%#v则是最详细的结果。

```go
    a := "hello"
    fmt.Println(strings.Contains(a, "li")) // 检查字符串a是否包含子串"li"，返回布尔值
    fmt.Println(strings.Count(a, "l")) // 统计字符串a中出现子串"l"的次数
    fmt.Println(strings.HasPrefix(a, "he")) // 检查字符串a是否以前缀"he"开头，返回布尔值
    fmt.Println(strings.HasSuffix(a, "llo")) // 检查字符串a是否以后缀"llo"结尾，返回布尔值
    fmt.Println(strings.Index(a, "li")) // 查找子串"li"在字符串a中首次出现的位置，返回索引值，未找到则返回-1
    fmt.Println(strings.Join([]string{"he", "llo"}, "-")) // 将字符串片段["he", "llo"]使用连接符"-"拼接为单个字符串
    fmt.Println(strings.Repeat(a, 2)) // 将字符串a重复2次
    fmt.Println(strings.Replace(a, "e", "E", -1)) // 将字符串a中的所有字母"e"替换为"E"，-1表示替换所有匹配项
    fmt.Println(strings.Split("a-b-c", "-")) // 将字符串按照连接符"-"进行分割，返回切片
    fmt.Println(strings.ToLower(a)) // 将字符串a转换为小写字母形式
    fmt.Println(strings.ToUpper(a)) // 将字符串a转换为大写字母形式
    fmt.Println(len(a)) // 计算字符串a的长度
    fmt.Printf("s=%v\n", s) // 格式化输出字符串变量s
    fmt.Printf("n=%v\n", n) // 格式化输出整数变量n
    fmt.Printf("p=%v\n", p) // 格式化输出任意类型变量p
    fmt.Printf("p=%+v\n", p) // 以更详细的方式格式化输出结构体类型变量p
    fmt.Printf("p=%#v\n", p) // 以最详细的方式格式化输出结构体类型变量p

    f := 3.141592653
    fmt.Println(f) // 打印浮点数f
    fmt.Printf("%.2f\n", f) // 以两位小数格式化输出浮点数f

```

##### 2.10 json处理

对于一个结构体，只需要保证每个字段的第一个字母是大写的，也就是公开字段。那么这个结构体就能用`JSON.marshal()`去序列化，变成一个`json`字符串，系列化后的字符串也能用`JSON.unmarshal()`去反序列化到一个空的变量里面。这样默认序列化出来的字符串的风格是大写字母的，可以在后面用`json tag`等语法修改输出`json`结果里面的字段名。

```go
	a := userInfo{Name: "wang", Age: 18, Hobby: []string{"Golang", "TypeScript"}}
	buf, err := json.Marshal(a) //去序列化
	if err != nil {
		panic(err)
	}
	fmt.Println(buf)
	fmt.Println(string(buf))

	buf, err = json.MarshalIndent(a, "", "\t")
	if err != nil {
		panic(err)
	}
	fmt.Println(string(buf))

	var b userInfo
	err = json.Unmarshal(buf, &b) //反序列化
	if err != nil {
		panic(err)
	}
	fmt.Printf("%#v\n", b)
```

##### 2.11 时间处理 数字解析

time.Now()获取当前的时间，也可以用time.date() 去构造一个带时区的时间，可以用sub得到两个时间点的时间差，用.UNIX()获取时间戳。数字解析则用strconv包下的函数，将字符串转化为数字。

```go
	now := time.Now()
	fmt.Println(now)
	t := time.Date(2022, 3, 27, 1, 25, 36, 0, time.UTC)
	t1 := time.Date(2022, 3, 27, 2, 30, 36, 0, time.UTC)
	fmt.Println(t)
	fmt.Println(t.Year(), t.Month(), t, t.Day(), t.Hour(), t.Minute())
	fmt.Println(t.Format("2006-01-02 15:04:05"))
	diff := t1.Sub(t)
	fmt.Println(diff)
	fmt.Println(diff.Minutes(), diff.Seconds())
	t3, err := time.Parse("2006-01-02 15:04:05", "2022-03-27 01:25:36")
	if err != nil {
		panic(err)
	}
	fmt.Println(t3 == t)
	fmt.Println(now.Unix()) //时间戳
f, _ := strconv.ParseFloat("1.234", 64) //strint convert to float64 下面同理
	fmt.Println(f) //1.234 

	n, _ := strconv.ParseInt("111", 10, 64)
	fmt.Println(n) //111

	n, _ = strconv.ParseInt("0x1000", 0, 64)
	fmt.Println(n) //4096

	n2, _ := strconv.Atoi("123")
	fmt.Println(n2) //123
```

##### 2.12进程信息

go语言中可以用os.argv来得到程序执行的时候的指定的命令行参数。

```go
	fmt.Println(os.Args)
	fmt.Println(os.Getenv("PATH"))
	fmt.Println(os.Setenv("AA", "BB"))

	buf, err := exec.Command("grep", "127.0.0.1", "/etc/hosts").CombinedOutput()
	if err != nil {
		panic(err)
	}
	fmt.Println(string(buf))
```

## 2、GO工程实践

### 猜数字游戏

用时间戳初始化种子生成随机数，在死循环中判断用户输入数字和随机数的大小关系，最后如果等于随机数则退出。

```go
func main() {
	maxNum := 100 // 定义最大数字范围为100

	rand.Seed(time.Now().UnixNano()) // 使用当前时间的纳秒数作为随机种子
	secretNumber := rand.Intn(maxNum) // 生成一个随机的秘密数字

	fmt.Println("please input your guess") // 提示用户输入猜测的数字

	reader := bufio.NewReader(os.Stdin) // 创建用于读取输入的读取器

	for {
		input, err := reader.ReadString('\n') // 从终端读取用户输入的数据
		if err != nil {
			fmt.Println("An error occured while reading input. Please try again")
			continue // 如果读取出错，继续下一次循环
		}

		input = strings.Trim(input, "\r\n") // 去除字符串中的换行符和回车符

		guess, err := strconv.Atoi(input) // 将用户输入的字符串转换为整数
		if err != nil {
			fmt.Println("Invalid input. Please enter an integer value")
			continue // 如果转换出错，继续下一次循环
		}

		fmt.Println("You guess is", guess) // 打印用户猜测的数字

		if guess > secretNumber { // 判断用户猜测的数字与秘密数字之间的大小关系
			fmt.Println("Your guess is bigger than the secret number. Please try again.")
		} else if guess < secretNumber {
			fmt.Println("Your guess is smaller than the secret number. Please try again.")
		} else {
			fmt.Println("Correct, you Legend!") // 猜测正确，输出提示信息
			break // 结束循环
		}
	}
}

```

### 在线词典

运用第三方api以及代码生成工具，对生成代码解读，解析reaponse body,最终打印目标结果。

代码主体由结构体DictRequest和DictResponse，query函数和main函数组成

```go
func query(word string) {
	client := &http.Client{} // 创建 HTTP 客户端

	request := DictRequest{TransType: "en2zh", Source: word} // 创建查询请求
	buf, err := json.Marshal(request) // 将请求结构体转换为 JSON 格式的字节数组
	if err != nil {
		log.Fatal(err)
	}

	var data = bytes.NewBuffer(buf) // 创建存储请求数据的缓冲区
	req, err := http.NewRequest("POST", "https://api.interpreter.caiyunai.com/v1/dict", data) // 创建 HTTP POST 请求
	if err != nil {
		log.Fatal(err)
	}

	// 设置请求头
	req.Header.Set("authority", "api.interpreter.caiyunai.com")
	req.Header.Set("accept", "application/json, text/plain, */*")
	req.Header.Set("accept-language", "en,zh-CN;q=0.9,zh;q=0.8,en-US;q=0.7")
	req.Header.Set("app-name", "xy")
	req.Header.Set("content-type", "application/json;charset=UTF-8")
	req.Header.Set("device-id", "")
	req.Header.Set("origin", "https://fanyi.caiyunapp.com")
	req.Header.Set("os-type", "web")
	req.Header.Set("os-version", "")
	req.Header.Set("referer", "https://fanyi.caiyunapp.com/")
	req.Header.Set("sec-ch-ua", `"Not?A_Brand";v="8", "Chromium";v="108", "Google Chrome";v="108"`)
	req.Header.Set("sec-ch-ua-mobile", "?0")
	req.Header.Set("sec-ch-ua-platform", `"Windows"`)
	req.Header.Set("sec-fetch-dest", "empty")
	req.Header.Set("sec-fetch-mode", "cors")
	req.Header.Set("sec-fetch-site", "cross-site")
	req.Header.Set("user-agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36")
	req.Header.Set("x-authorization", "token:qgemv4jr1y38jyq6vhvi")

	resp, err := client.Do(req) // 发送请求
	if err != nil {
		log.Fatal(err)
	}
	defer resp.Body.Close() // 关闭响应体

	bodyText, err := ioutil.ReadAll(resp.Body) // 读取响应体的数据
	if err != nil {
		log.Fatal(err)
	}

	if resp.StatusCode != 200 { // 检查响应的状态码
		log.Fatal("bad StatusCode:", resp.StatusCode, "body", string(bodyText))
	}

	var dictResponse DictResponse
	err = json.Unmarshal(bodyText, &dictResponse) // 解析响应体的 JSON 数据到结构体中
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(word, "UK:", dictResponse.Dictionary.Prons.En, "US:", dictResponse.Dictionary.Prons.EnUs) // 输出查询结果中的发音信息
	for _, item := range dictResponse.Dictionary.Explanations {
		fmt.Println(item) // 输出查询结果中的释义信息
	}
}

func main() {
	if len(os.Args) != 2 {
		fmt.Fprintf(os.Stderr, `usage: simpleDict WORD example: simpleDict hello`)
		os.Exit(1)
	}

	word := os.Args[1] // 获取命令行参数中的查询词
	query(word) // 调用查询函数进行查询
}

```

最终效果：

![image-20230115193628625](D:\文档\文本\学习笔记\typora_imag\image-20230115193628625.png)

### socks5代理服务器

#### socks5:

SOCKS5 是一个代理协议，在TCP/IP协议通讯中的前端及其和服务器机器之间扮演一个中介角色，使得内部网中的前端机器变得能够访问Internet网中的服务器，或者使通讯更加安全。简而言之，socks5可以使授权用户通过单个端口访问内部所有资源，从而避免严格的防火墙带来的副作用。

#### socks原理:

![image](F:\chrome\image.png)

正常浏览访问一个网站，不经过代理服务器的正常流程为：先和对方的网站建立TCP连接，然后三次握手，握手完之后发起HTTP请求，之后服务返回HTTP响应。

而设置代理服务器流程会变得复杂一些：浏览器和socks5代理建立TCP连接，代理再和真正的服务器建立TCP连接。可以分为四个阶段：握手阶段、认证阶段、请求阶段、realy阶段。

**一、握手阶段。**浏览器会向socks5代理发送请求，包的内容包括一个协议的版本号，支持认证的种类，socks5服务器会先选中一个认证方式，返回给浏览器。

**二、认证阶段。**如果返回的是00的话代表不需要认证，返回其他类型会开始认证流程。

**三、请求阶段。**认证通过后浏览器会向socks5服务器发起请求。主要信息包括版本号，请求的类型，一般主要是connection请求，就代表代理服务器和某个域名或者某个IP地址某个端口建立TCP连接。代理服务器会和真正的后端服务器建立连接，然后返回一个响应。

**四、relay阶段。**此时浏览器会发送正常发送请求，然后代理服务器接收到请求之后，会直接把请求转换到真正的服务器上。然后真正的服务器返回响应也会把请求转发到浏览器这边。

#### TCP echo server

首先写一个简单的TCP echo server，完成一个能够返回输入信息的一个TCP server。

```go
func main() {
	server, err := net.Listen("tcp", "127.0.0.1:1080") //监听端口，会返回一个server
	if err != nil {
		panic(err)
	}
	for { //进入死循环
		client, err := server.Accept()
		if err != nil {
			log.Printf("Accept failed %v", err)
			continue
		}
		go process(client) //启动一个goroutinue
	}
}
func process(conn net.Conn) {
	defer conn.Close()              //函数退出时要把连接关掉，避免资源泄露
	reader := bufio.NewReader(conn) //创建一个带缓冲的只读流，减少底层系统调用的次数
	for {
		b, err := reader.ReadByte() //readbyte读取单个字节，再把这一个此节写进去连接
		if err != nil {
			break
		}
		_, err = conn.Write([]byte{b})
		if err != nil {
			break
		}
	}
}
```

此时就可以简单做一个测试，运行go程序后，可以在终端中输入

```
nc 127.0.0.1 1080
```

即可得到输入的内容。（nc为netcat，为Linux自带命令，Windows下需要下载一个程序后才可使用。）

#### Auth

添加以下内容。

```go
const (
	socks5Ver  = 0x05   // SOCKS5协议版本号
	cmdBind    = 0x01   // 绑定命令
	atypIPV4   = 0x01   // IPv4地址类型
	atypeHOST  = 0x03   // 域名地址类型
	atypeIPV6  = 0x04   // IPv6地址类型
)

func auth(reader *bufio.Reader, conn net.Conn) (err error) {
	ver, err := reader.ReadByte() // 读取协议版本号
	if err != nil {
		return fmt.Errorf("read ver failed: %w", err)
	}
	if ver != socks5Ver { // 检查协议版本号是否为SOCKS5
		return fmt.Errorf("not supported ver: %v", ver)
	}

	methodSize, err := reader.ReadByte() // 读取认证方法数量字段
	if err != nil {
		return fmt.Errorf("read methodSize failed: %w", err)
	}
	method := make([]byte, methodSize)
	_, err = io.ReadFull(reader, method) // 读取认证方法字节
	if err != nil {
		return fmt.Errorf("read method failed: %w", err)
	}

	log.Panicln("ver", ver, "method", method) // 输出读取的协议版本号和认证方法

	// 发送响应数据，表示服务器支持无需认证的方法
	_, err = conn.Write([]byte{socks5Ver, 0x00})
	if err != nil {
		return fmt.Errorf("write failed: %w", err)
	}

	return nil
}
```

#### 请求阶段

试图读取到 携带 URL 或者 IP 地址+端口的包，然后把它打印出来。实现一个connect函数，同样在process里面去调用。浏览器会发送一个包，包里面包括六个字段：1、version版本号；2、command，代表请求的类型，我们只支持connection请求，也就是让代理服务建立新的TCP连接；3、RSV保留字段，不理会；4、atype，目标地址类型，可能是IPV4或者IPV6；5、addr，地址长度根据atype的类型不同而不同。6、port端口号，两个字节，我们需要逐个去读取这些字段。

#### relay阶段

标准库的io.copy可以实现一个单向数据转发，双向数据转发需要启动两个goroutinue。还有一个问题为connect（）函数会立刻返回，返回后即关闭。我们需要等待任意一个方向copy出错的时候再返回connect（）函数。这里可以用标准库中的一个context机制，用context连接with cancel来创建一个context。最后等待ctx.Done（），只要cancel被调用，ctx.Done()就会立刻返回，然后再上面的goroutinue里面调用一次cancel即可。

#### 最终测试

运行程序再执行curl命令可以得到成功返回的内容。或者在switchomega插件中得到代理服务器里显示的浏览器版本的域名和端口。

## day1总结

​	青训营的课程节奏比较快，不过有了其他语言的基础学习go语言还是比较容易的，第一天学习了go的语言特性，基础语法，并用go语言开发了三个小工程，凸显了go语言使用的场景，对于go语言由丰富的标准库，完善的工具链，高性能，高并发的的特性。小工程代码量不多的情况下实现了很实用的功能。相比于其他语言有很多实用函数需要学习，也可以更好面对很多应用场景，所以学习难度也会加大，需要多敲代码熟悉语言功能。

# 0116 day2 Go语言进阶与依赖管理

## 01并发-并行

Go可以充分发挥多核优势，高效运行。

协程：用户态，轻量级线程，栈KB级别。

线程：内核态，线程跑多个协程，栈MB级别。

Go语言一次可以创建上万个协程。

#### 1.1 Goroutine

创建协程。

#### 1.2 CSP （Communication Sequential Processes)

Go提倡通过**通信共享内存**而不是**通过共享内存而实现通信**。

#### 1.3 Channel

make(chan 元素类型，[] )

无缓冲通道 make(chan int)

有缓冲通道 make(chan int,2)

#### 1.4 并发安全 Lock

并发安全有一定概率影响结果。

#### 1.5 Wait Group

Sync包下的Wait Group实现并发任务同步。

## 02 依赖管理

学会站在巨人的肩膀上。

### 2.1依赖管理演进

##### 1）GOPATH

问题：无法实现package的多版本控制。

##### 2）Go Vendor

解决了多个项目需要同一个package依赖的冲突问题。

弊端：无法控制依赖的版本。更新项目又可能出现以来冲突，导致编译出错。

##### 3）Go Module

定义版本规则和管理项目依赖关系。

### 2.2 依赖管理三要素

1.配置文献，描述依赖。 go.mod

2.中心仓库管理依赖库。Proxy

3.本地工具。go get/mod 

### 2.3 go.mod

#### 2.3.1依赖配置 - go.mod



#### 2.3.2 依赖配置 - version

语义化版本 

基于commit伪版本

#### 2.3.3 依赖配置- indirect

A->B->C  

A->B直接依赖

A->C间接依赖

#### 2.3.4 依赖配置- imcompatible

主版本2+模块会在模块路径增加/vN后缀。

对于没有go.mod文件并且主版本2+的依赖，会+incompatible

#### 2.3.4 依赖配置-依赖图

#### 2.3.5 依赖分发-回源

无法保证构建稳定性

无法保证依赖可用性

增加第三方压力

#### 依赖分发 -Proxy

稳定+可靠

#### 2.3.6 依赖分发 - 变量 GOPROXY



#### 2.3.7 - go get 



#### 2.3.8 - go mod 

1）init 初始化，创建go.mod文件

​	  download 	下载模块到本地缓存

​	  tidy 				增加需要的依赖，删除不需要的依赖

2）依赖管理三要素

1.配置文件，描述依赖	go.mod

2.中心仓库管理依赖库	Proxy

3.本地工具						go get/mod 

## 03测试

#### 1）回归测试

#### 2）集成测试

#### 3）单元测试

测试单元的输出和期望进行校对，

规则

所有测试文件以_test.go结尾

func TestXxx(*testing.T)

初始化逻辑放到TestMain中

##### 覆盖率

一般覆盖率：50%~60%，较高覆盖率80%。

测试分支相互独立，全面覆盖。

测试单元粒度足够小，函数单一职责。

外部依赖=》稳定&幂等

#### 文件处理-Mock

#### 基准测试

### 04项目实践

过程

#### 需求描述

#### 项目拆解

#### 代码设计

#### 测试运行

## day2总结

课程的节奏还是很快的。今天的课程主要讲解了实践中项目需要注意的问题，Go语言特性对项目的中的运用，项目测试的重要性及常用方法，项目实践的四个步骤。Go语言多协程的特性保证了项目的高效性；对依赖管理了解熟悉了Go语言对项目逐渐完善的过程，形成了Go Module的管理关系；实际开发中测试是必不可少的，集中讲解了测试的常用方法；最终拆解了掘金一般文章项目的整个需求，并对其分析了整个项目实践的过程。今天的课程主要是对项目方面的讲解，对Go语言实践开发有很大帮助。

# 0117 day3 Go 高质量编程与性能调优(1)

## 01高质量编程

什么是高质量——编写的代码能够达到正确可靠，简洁清晰的目标可称之为高质量代码

边界问题

异常处理

易读易维护

### 1.1简介

编程原则：**简单性**  **可读性  生产力**

### 1.2 编码规范

代码格式 注释 命名规范 控制流程 错误和异常处理

**公共符号始终要注释**

#### **代码格式**：

gofmt goimports 

#### **注释应该做的：**

注释应该解释代码的作用

注释应该解释代码如何做的

注释应该解释代码实现的原因

注释应该解释代码什么情况会出错

#### **小结**：

代码是最好的注释

注释应该提供代码未表达出的上下文信息。

#### **命名规范**：

##### 变量名

简洁胜于冗长

缩略词全大写，当其位于变量开头不需要导出时，使用全小写。

变量距离其被使用的地方越远，则需要携带越多的上下文信息。

##### 函数名

函数名不携带包名的上下文信息，因为包名和函数名总是成对出现的。

函数名尽量简短。

当名为foo的包某个函数返回类型Foo时，可以省略类型信息而不导致歧义。

当名为foo的包某个函数返回类型T时，可以在函数名中加入类型信息。

##### 包名

只由小写字母组成。不包含大写字母和下划线等字符。

简短并包含一定的上下文信息。如schema,task等。

不要与标准库同名。如不要使用sync或者strings

以下规则尽量满足，以标准库包名为例子。

不使用常用变量作为包名。

使用单数而不是复数。

谨慎地使用缩写。

#### 控制流程

避免嵌套，保持正常流程清晰

尽量保持正常代码路径为最小缩进

#### 错误和异常处理

简单错误

错误的Wrap和Unwrap

错误判定

不建议在业务代码中使用Panic

recover只能在被defer的函数中使用

error尽可能提供简明的上下文信息链，方便定位问题。

## 02性能调优实战

Benchmark

Slice 

slice 预分配内存

尽可能在使用make()初始化切片时提供容量信息

大内存未释放

map预分配内存

字符串处理 

​	使用strings.Builder,+拼接性能最差，bytes.Buffer与strings.Builder相近，strings.Buffer更快

使用空结构体节省内存

atomic包

算法问题 性能调优问题  没有明确边界

### 性能调优原则：

要依靠数据不是猜测

要定位最大瓶颈而不是细枝末节

不要过早优化

不要过度优化

### 性能分析工具 pprof

pprof-排查实战。

**CPU**

**heap-堆内存**

**goroutine-协程**

mutex-锁

block-阻塞

### pprof-采样过程和原理

CPU：

采样对象：函数调用和它们占用的时间。

采样率：100次/秒，固定值。

采样时间：从手动启动到手动结束。

Heap-堆内存

采样程序通过内存分配器在堆上分配和释放的内存，记录分配/释放的大小和数量。

采样率：每分配512KMB记录一次，可在运行开头修改，1为每次分配均记录。

采样时间：从程序运行开始到采样时。

采样指标：alloc_space, alloc_objects, inuse_space, inuse_objects。

计算方式：inuse = alloc - free

Goroutine-协程&Thread Create-线程创建

**Goroutine**

​	记录所有用户发起并且在运行中的goroutine（即入口非runtime开头的）

​	runtime.main 的调用栈信息

**Thread Create**

​	记录程序创建的所有系统线程的信息

Block-阻塞 & Mutex-锁

阻塞操作

​	采样阻塞操作的次数和耗时

​	采样率:阻塞耗时超过阈值的才会被记录，1为每次阻塞均记录

锁竞争

​	采样争抢的次数和耗时

​	采样率：只记录固定比例的锁操作，1为每次加锁均记录

### 性能调优案例

业务服务优化

基础库优化

Go语言优化

#### 流程

建立服务性能评估手段

分析性能数据，定位性能瓶颈

重点优化项改造

优化效果验证

# 0119 day5 Go性能优化及自动内存管理

#### 性能优化是什么？

提升软件系统能力，减少不必要的小号，充分发掘计算机算力。

#### 为什么要做性能优化？

用户体验：带来用户体验的提升

资源高效利用，降低成本，提高效率。

**业务层优化**：针对特定场景，具体问题，具体分析

**语言运行时优化**：解决更通用的性能问题

**数据驱动**：自动化性能分析工具——pprof

## 01自动内存管理

动态内存：程序在运行时根据需求动态分配的内存：malloc()

自动内存管理（垃圾回收）：由程序语言的运行时系统管理动态内存

Mutator:业务线程，分配新对象，修改对象指向关系。

Collector:GC线程，找到存活对象，回收死亡对象的内存空间。

Serial GC：只有一个collector

Parallel GC:只是多个collectors同时回收的GC算法。

Concurrent GC：mutators和collectors可以同时执行。

​	Collectors必须感知对象的指向关系的改变。

评价GC算法：

- ​	安全性（Safety):不能回收活的对象。（基本要求）
- 吞吐率（Throughput):1-GC时间/程序执行总时间 （花在GC上的时间）
- 暂停时间（Pause time）：stop the world(STW) （业务是否感知）
- 内存开销（Space overhead) GC元数据开销

**追踪垃圾回收（Tracing garbage collection)**

对象被回收条件：指针指向关系不可达的对象

![](F:\chrome\f807d160-4645-4ca9-aa44-e70bd5c6a8e7.png)

标记根对象

- 静态变量，全局变量，常量，线程栈等。

标记：找到可达对象

- 求指针指向关系的传递闭包：从根对象出发，找到所有可达对象

清理：所有不可达对象

- 将存活对象复制到另外的内存空间（Copying GC)
- 将死亡对象的内存标记为“可分配”（Mark-sweep GC)
- 移动并整理存活对象（Mark-compact GC）
- 根据对象的生存周期，使用不同的标记和清理策略。

年轻代（Young generation）

- 常规的对象分配
- 由于存活对象很少，可以采用copying collection
- GC吞吐率很高

老年代（Old generation）

- 对象趋向于一直活着，反复赋值开销较大
- 可以采用mark-sweep collection

**引用计数（Reference counting）**

![](F:\chrome\download.png)

- 每个对象由一个与之关联的引用数目
- 对象存活的条件：当且仅当引用数大于0
- 优点：
  - 内存管理的操作被平摊到程序执行过程中
  - 内存管理不需要了解runtime的实现细节；C++智能指针（smart pointer)

​	缺点：

- 维护引用计数的开销较大：通过原子操作保证队引用计数操作的原子性和可见性。
- 无法回收环形数据结构——weak reference
- 内存开销：每个对象都引入的额外内存空间存储引用数目
- 回收内存时已然可能引发暂停

## 02Go内存管理及优化

### Go内存分配

#### **分块**

目标为对象在heap上分配内存。

提前将内存分块

- 调用系统调用的mmap()向OS申请一大块内存。
- 先将内存划分为大块，称为 mspan
- 再将大块继续划分为特定大小的小块，用于对象分配。
- noscan mspan:分配不包含指针的对象——GC不需要扫描
- scan mspan:分配包含指针的对象——GC需要扫描

对象分配：根据对象的大小，选择最合适的块返回。

#### 缓存

![](F:\chrome\43636c0d-4955-4e79-a64d-59517e5d8849.png)

- TCMalloc: thread caching 

- 每个p包含一个mcache用于快速分配，用于为绑定于p上的g分配对象
- mcache管理一组mspan
- 当mcache中的mspan分配完毕，向吗central申请带有未分配快的mspan
- 当mspan中没有分配的对象，mspan会被缓存在mcentral中，而不是立刻释放并归还给OS.

内存管理优化

- 对象分配是非常高频的操作：每秒分配GB级别的内存。
- 小对象占比较高
- Go内存分配比较耗时

### 优化方案:Balanced GC

- 每个g都绑定一块内存（1kb)，称作goroutine allocation buffer (GAB)
- GAB用于noscan类型的小对象分配：< 128B
- 使用三个指针维护GAB:base,end,top
- Bump pointer(指针碰撞) 风格对象分配
  - 无需和其他分配请求互斥
  - 分配动作简单高效

GAB对于Go内存管理来说是一个大对象

本质：将多个小对象的分配合并成一次大对象的分配

问题：GAB的对象分配方式会导致内存被延迟释放

## 03编译器和静态分析

![](F:\chrome\7526dde6-d551-42a8-b7cb-3597518fbfab.png)

- 重要的系统软件
- 分析部分（前端 front end）
- 综合部分（后端back end）

静态分析：不执行程序代码，推导程序的行为，分析程序的性质。

控制流（Control flow）：程序执行的流程

数据流（Data flow）：数据在控制流上的传递

通过分析控制流和数据流，我们可以知道更多关于程序的性质（properties），根据这些性质优化代码。

**过程内分析**：仅在过程内部进行分析

**过程间分析**：考虑过程调用时参数传递和返回值的数据流和控制流

## 04Go编译器优化

编译器优化：用户无感知，重新编译即可；通用性优化

### 函数内联（Inlining）：

将被调用函数的函数体（callee）的副本替换到调用位置（caller），同时重写代码以反映参数的绑定。

优点：消除函数调用开销，例如传递参数，保存寄存器等；将过程间分析转化为过程内分析。

缺点：函数体变大，instruction cache不友好；编译生成的Go镜像变大。

函数内联在大多数情况下是正向优化。

### Beast Mode

Go函数内敛受到的限制较多。

Beast Mode调整函数内联的策略，是更多函数被内联。

- 降低了函数调用的开销
- 增加了其他优化的机会：**逃逸分析**

逃逸分析：分析代码中指针的动态作用域（指针在何处可以被访问）

Beast mode:函数内联拓展了函数边界，更多对象不逃逸。

优化：为逃逸的对象可以在栈上分配

- 对象在栈上分配和回收很快：移动sp
- 减少在heap上的分配，降低GC负担

## day5总结

今天主要学习了Go语言内部自动内存管理以及应用于性能优化的主要方式，字节跳动内部的优化方法。程序在实现众多方法后性能和效率是十分重要的，Go语言作为后端开发语言有着自动内存管理以及优化方案GC等特性，在性能调优方面有着独特的优势。性能调优中着重讲解了Beast Mode，其具有降低开销，增加优化的功能，对优化CPU占用率有显著效果。

# day6 Go框架三件（Web/RPC/ORM）

Gorm 强大的ORM框架

Kitex Golang 微服务RPC框架

Hertz 字节内部的HTTP框架

## 三件套基础使用

## 三件套

## Grom

Gorm文档 https://gorm.cn/

### Gorm创建数据

### Gorm查询数据

### Gorm更新数据

实际工程中引用软删，Gorm提供了gorm.DeletedAt用于帮助用户实现软删：拥有软删除能力的Model调用Delete时，记录不会被从数据库中真正删除。但Gormandize会将DeleteAt置为当前时间，并且你不能在通过正常的查询方式找到该记录。

使用Underscoped可以查询到被软删的数据。

### Gorm事物

Gorm提供里Begin，Commit，Rollback方法用于使用事务。

Gorm提供里Transaction方法用于自动提交事务，避免用户漏写Commit，Rollback

Gorm Hook 在提供了CURD的Hook能力。Hook是在创建、查询、更新、删除等操作之前，之后自动调用的函数。

如果任何Hook返回错误，Gorm将停止后续的操作并回滚事务。

### Gorm性能提高

对于写操作（创建、更新、删除），为了确保数据的完整性，Gorm会将它们封装在事务内运行。但这会降低性能，你可以使用SkipDefaultTransaction关闭默认事务。

使用PrepareStmt缓存预编译语句可以提高后续调用的速度。

### Gorm生态

Gorm拥有非常丰富的扩展生态。

## Kitex

使用IDL定义服务和接口

如果需要进行RPC，就需要知道对方的接口是什么，需要传什么参数，同时也需要直到返回值是什么样的。就需要通过IDL来约定双方的协议。

基本使用 

服务默认监听8888端口。

创建Client

发起请求

目前Kitex的服务注册与发现已经对接了主流了服务注册和发现中心，如ETCD，Nacos。

### Kitex服务注册与发现

### Kitex生态

## Hertz

基本使用

Hertz提供了GET,POST,PUT,DELETE ANY等方法注册路由。

Hertz提供了路由组（Group)的能力，用于支持路由分组。

Hertz提供了参数路由和通配路由，路由的优先级为：静态路由-》命名路由-》通配路由

Hertz提供了Bind，Validate，BindAndValidate函数用于进行参数绑定。

Hertz的中间件主要分为客户端中间件与服务端中间件。

Hertz提供了HTTP Client用于帮助用户发送HTTP请求。

### Hertz代码生成工具

Hertz提供里代码生成工具HZ，通过定义IDL(inteface description language)文件即可生成树对应的基础服务代码。

### Hertz性能

网络库Netpoll，Json编解码Sonic，使用sync.Pool复用对象协议层数据解析优化。

### Hertz生态

Hertz拥有丰富的拓展生态。

## 实战案例介绍

通过使用Hertz，Kitex，Gorm搭建出具有有定业务逻辑的后端API项目。

## 总结

这次课程学习了Go开发框架三件套，由字节跳动内部开发经验总结得来，通过三件套可以更轻松应对开发中的代码需求。这些框架在项目中都是需要应用的，Gorm, Kitex, Hertz分别是ORM, RPC, HTTP框架，能够更好完善项目需求。

# day7 规则引擎设计与实践

## 规则引擎

规则引擎：规则引擎是一种嵌入在应用程序中的组件，实现了将业务决策从应用程序中分离出来，并使用预定义的语义模块编写业务决策。接收数据输入，解释业务规则，并根据业务规则做出业务决策。

可以解决开发人员重复编码的问题。

业务决策与服务本身解耦，提高服务的可维护性，缩短开发路径，提高效率。

接受数据输入:支持接收使用预定义的语义编写的规则作为策略集。

规则理解：能偶按照预先定义的词法，语法，优先级，运算符等正确理解业务规则所表达的语义。

规则执行：根据执行时输入的参数对策略集中的规则正确的解释和执行。

### 应用

风控对抗 ； 活动策略运营 ；数据分析和清洗

## 编译原理及基本概念

规则引擎编译原理：理解（词法、语法分析），执行（抽象语法树），输入输出（参数注入，类型检查）

### 词法分析（Lexical Analysis)

词法分析：把源代码字符串转换为词法单元（Token）这个过程。

识别（Token） 有限自动机（Finite-State Automation)

### 语法分析（Syntax Analysis）

在词法分析的基础上，识别表达式的语法结构的过程。

### 抽象语法树（Abstract Syntax Tree）

### 上下文无关语法 Context-Free Grammar

语言句子无需考虑上下文，就可以判断正确性，可以使用巴科斯范式（BNF）来表达。

产生式：一个表达式可以由另外已知类型的表达式或符号推导产生。

### 递归下降算法（Recursive Descent Parsing）

递归下降算法就是自顶向下构造语法树，不断的对Token进行语法展开（下降），展开过程中可能会遇到递归的情况。

### 类型检查

#### 类型综合：

根据子表达式的类型构造出父表达式的类型。根据A类型和B类型判断A+B类型。

#### 编译时检查&运行时检查：

类型检查可以发生在表达式编译阶段，即语法树构造阶段，也可以发生在执行阶段。

- 编译时：需要提前声明参数的类型，在构建语法树的过程中进行类型检查。
- 执行时：可以根据执行时的参数输入的值类型，在执行过程中进行类型检查。

## 今日总结

对于实际业务中，规则引擎是一种可以解决开发人员重复编码的利器。运用编译原理的相关知识，词法、语法分析，抽象语法树，以及递归下降算法，类型检查，即可以解析实际字符串内容实现快速便捷的业务决策。

# day8 架构初探 - 谁动了我的蛋糕

## 什么是架构？

架构：软件架构，有关软件整体结构与组件的抽象描述；用于直到软件系统各个方面的设计。即为在软件的众多实现方法中在方法选择上起着至关重要的指导作用。

### 单机架构

把所有功能都实现在一个进程里，并部署在一台机器上。

优点：简单

问题：运维需要停服

### 单体、垂直应用架构|垂直切分架构

单体架构：分布式部署

垂直应用架构：按应用垂直切分的单体

优点：水平扩容 ；运维不需要停服

问题：职责太多，开发效率不高 ；爆炸半径大

### SOA、微服务|水平切分

SOA（Service-Oriented Architecture）

1. 将应用的不同功能抽象为服务
2. 定义服务之间的通信标准

微服务架构：SOA 的去中心化演进方向

架构演进初衷：

- 需求量越来越大，终归要增加人手
- 越做越复杂，终归要分合作

架构演进思路：

- 垂直切分
- 水平切分

## 企业级后端架构剖析

### 云计算

云计算值通过软件自动化，提供计算资源的服务网络，现代互联网大规模熟悉分析和存储的基石。

基础：

- 虚拟化技术
- 编排方案 

架构：

- IaaS(Infrastructure as a Service)
  - 买房子 vs 房屋租赁平台


- PaaS(Platform as a Service)
  - 清包 vs 全包
- SaaS(Software as a Service)
  - 从零培训 vs 雇佣培训过的师傅
- FaaS(Function as a Service)
  - 纯手工 vs 蛋糕机批量生产

### 云原生：

云原生技术为组织（公司）在公有云，自由云，混合云等新型的动态环境中构建和运行可弹性的拓展提供里可能。

#### 弹性计算资源

- 服务资源调度
  - 微服务 ；大服务

- 计算资源调度
  - 在线 ；离线

- 消息队列
  - 在线 ； 离线

类型：

- 经典
- 关系型数据库
- 元数据
- NoSQL

将存储资源当成服务。

#### DevOps

云原生时代软件交付的利器，贯穿整个软件开发周期。结合自动化流程，提高软件开发，交付效率。

#### 微服务架构

通信标准：

- HTTP（RESTful API）
- RPC(Thrift gRPC)

#### 服务网格（Service Mesh）

- 微服务之间通讯的中间层
- 高性能网络代理
- 业务代码与治理解耦

相比较于RPC/HTTP框架：

- 异构系统治理统一化
- 与业务进程解耦，生命周期易管理。

## 后端架构的挑战——问题

挑战

- 基础设施层面
  - 物理资源是有限的
    - 机器
    - 带宽
  - 资源利用率受制于部署服务
- 用户层面
  - 网络通信开销较大
  - 网络抖动导致运维成本提高
  - 异构环境下，不同实例资源水位不均

### 离在线资源并池

核心收益：

- 降低物理资源成本
- 提供更多的弹性资源，增加收入

在线业务的特点：

- IO密集型为主
- 潮汐性，实时性

离线业务的特点

- 计算密集型占多数
- 非实时性

### 自动扩缩容

核心收益：

- 降低业务成本

- 利用在线业务潮汐性自动扩缩容

### 微服务亲合性部署

核心收益：

- 降低业务成本
- 提高服务可用性

- 将满足亲合性条件的容器调度到一台宿主机
- 微服务中间件与服务网格通过共享内存通信
- 服务网格控制面实施灵活，动态的流量调度

### 流量治理

核心收益：

- 提高微服务调用容错性
- 容灾
- 进一步提高开发效率，DevOps发挥到极致。

解决思路：基于微服务中间件 & 服务网格的流量治理

- 熔断、重试
- 单元化
- 复杂环境（功能，预览）的流量调度

### CPU水位负载均衡

核心收益：

- 打平异构环境算力差异
- 为自动扩缩容提供正向输入

- IaaS
  - 提供资源探针
- 服务网络
  - 动态负载均衡

## 后端架构实战

### 问题提炼

- 服务网络数据面
  - 支持带权重的负载均衡策略
- 注册中心存储了所有容器的权重信息
- 宿主能提供
  - 容器的资源使用情况
  - 物理资源信息（CPU型号）

#### 自适应静态权重

方案:

采集宿主机物理资源信息

调整容器注册的权重

优势

- 复杂度低
- 完全分布式，可用性高
- 微服务中间件无适配成本

缺点：

- 无紧急回滚能力
- 缺乏进行时自适应能力

#### 自适应动态权重AIpha

方案：

- 容器动态权重的自适应调整
- 服务网络的服务发现 & 流量调度能力

演进方向：

- 解决无法紧急回滚的问题
- 运行时权重自适应

缺点

- 过度流量倾斜的可能会有异常情况

方案：

- 服务网格上包RPC指标

演进方向

- 极端场景的处理称为可能

缺点：

- 时序数据库压力较大
- 动态权重决策中心职责越来越多，迭代-》变更-》风险

#### 自适应动态权重Release

演进方向：

- 微服务化
- 引入消息队列削峰，解耦
- 离在线链路切分
- 梳理强弱依赖

## 今日总结

今天主要学习了软件开发初期架构选择以及后期架构优化相关的知识，以蛋糕为例，形象的解释了软件架构方面的演进。

# day9  分布式理论

### 分布式：

分布式系统是计算机程序的集合，这些程序利用跨多个独立计算节点的计算资源来实现共同的目标。可以分为分布式计算，分布式存储，分布式数据库等。

优势：

1、去中心化

2、低成本

3、弹性

4、资源共享

5、可靠性

挑战：

1、普遍的节点故障

2、不可靠的网络

3、异构的机器与硬件环境

4、安全性

### 常见的分布式系统

分布式存储：

1、Google File System （GFS）

2、Ceph

3、Hadoop HDFS 

4、Zookeeper

分布式数据库：

1、Google Spanner

2、TiDBi

3、HBase

4、MongoDB

分布式计算：

1、Hadoop

2、Spark

3、YARN

## 系统模型

故障模型

- Byzantine failure ：节点可以任意篡改发送给其他节点的数据
- Authentication detectable byzantine failure (ADB) : Byzantine failure的特例；节点可以篡改数据，但不能伪造其他节点的数据
- Performance failure ： 节点未在特定时间段内收到数据，即时间太早或太晚
- Omission failure : 节点收到数据的时间无限晚，即收不到数据
- Crash failure：在omission failure的基础上，增加了节点停止响应的假设，也即持续性地omission failure 
- Fail-stop failure：在Crash failure的基础上增加了错误可检测的假设

![image-20230202115656587](D:\文档\文本\学习笔记\typora_imag\image-20230202115656587.png)

### 拜占庭将军问题

两个将军：

结论：两将军问题被证实无解的电脑通信问题，两支军队理论上永远无法达成共识。

TCP三次握手是在两个方向确认包的序列号，增加了超时重试，是两将军问题的一个工程解。

更普适的场景下的拜占庭将军问题。

### 共识和一致性

最终一致性和线性一致性。

如果要保证“线性”一致性，多个节点间势必需要进行协商，以寻求一致。正要增加了延迟，系统可用性便会受损。

### 时间和时间顺序

## 基础理论

### CAP理论

![image-20230202132032682](D:\文档\文本\学习笔记\typora_imag\image-20230202132032682.png)

CAP理论往往运用于数据库领域，同样可以适用于分布式存储方向

CA : 放弃分区容错性，加强一致性和可用性，其实就是传统的单机数据库的选择

AP : 放弃一致性（强一致性），追求分区容错性和可用性，例如一些注重用户体验的系统。

CP : 放弃可用性，追求一致性和分区容错性，例如与钱财安全相关的系统。

无法同时满足CAP三种选项。

### ACID理论

事务时数据库系统中非常重要的概念，它是数据库管理系统执行过程中的一个逻辑单元，它能够保证一个事务中的所有操作要么全部执行，要么全都不执行。

数据库事务拥有四个特征ACID，即分别是原子性（Atomicity），一致性（Consistency），隔离性（Isolation）和持久性（Durability）。

**原子性（A）**：原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚。

**一致性（C）**：一致性是指事务性必须使数据库从一个一致性变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。

隔离性（I）：隔离性是当多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事件之间要相互隔离。

持久性（D）：持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失事务的操作。

### BASE理论

Base理论是对CAP中一致性和可用性权衡的结果，其来源于对大型互联网分布式实践的总结，是基于CAP定理逐步演化而来的。

核心思想为

Basically Available（基本可用）：假设系统出现了不可预知的故障，但还是能用，相比较正常的系统而言：响应时间上的损失，或功能上的损失。

Soft state（软状态）：允许系统中的数据存在中间状态，并认为该状态不影响系统的整体可用性，即允许系统在多个不同节点的数据副本存在数据延时。

Eventually consistent（最终一致性）：系统能够保证在没有其他新的更新操作的情况下，数据最终一定能偶达到一致的状态，因此所有客户端对系统的数据访问最终都能够获取到最新的值。

## 分布式事务

### 两阶段提交（Two-phase Commit）

为了使基于分布式系统架构下的所有节点在进行事务提交时保持一致性而设计的一种演算法。

三个假设：

1、引入协调者（Coordinator）和参与者（Participants），互相进行网络通信。

2、所有节点都采用预写式日志，且日志被写入后即被保持在可靠的存储设备上。

3、所有节点不会永久性损坏，即使损坏后仍然可以恢复。

可能出现的情况：

1. Coordinator不宕机，Participants宕机。需要进行回滚操作。
2. Coordinator宕机，Participants不宕机。可以起新的协调者，待查询状态后，重复二阶段提交。
3. Coordinator宕机，Participants宕机。无法确认状态，需要数据库管理员的接入，防止数据库进入一个不一致的状态。

注意的问题：

1、性能问题

两阶段提交需要多次结点间的网络通信，耗时过大，资源需要进行锁定，徒增资源等待时间。

2、协调者单点故障问题

如果事务协调者节点宕机，需要另起新的协调者，否则参与者处于中间状态无法完成事务。

3、网络分区带来的数据不一致

一部分参与者收到了Commit消息，另一部分参与者没收到Commit消息，会导致了节点之间的数据不一致。

### 三阶段提交

将两阶段提交中的Prepare阶段，拆成两部分：Can Commit和PreCommit机制。

解决了：单点故障问题 ；阻塞问题

另外引入超时机制，在等待超时之后，会继续进行事物的提交。

### MVCC

MVCC是一种并发控制的方法，维持一个数据的各个版本使读写操作没有冲突。所以既不会阻塞写，也不会阻塞读。MVCC为每个修改保存一个版本，和事务的时间戳相关联。可以提高并发性能，解决脏读的问题。

提供他物理时钟，或者使用时间戳预言机（TSO），采用中心化的授时方式，所有协调者向中心化节点获取时钟。优点是算法简单，实现方便，但需要每个节点都与它进行交互，会产生一些网络通信的成本。TSO的授时中就需要考虑低延迟，高性能以及更好的容错性。

## 共识协议

### Quorum NWR模型

N：在分布式存储系统中，有多少份备份数据

W：代表一次成功的更新操作要求至少有w分数据写入成功

R：代表一次成功的读数据操作要求至少有R份数据成功读取。

为了保证强一致性，需要保证 W+R>N

Quorum NWR模型将CAP的选择交给用户，是一种简化版的一致性模型。

### RAFT协议

Raft协议是一种分布式一致性算法（共识算法），即使出现部分节点故障，网络延时等情况，也不影响各节点，进而提高系统的整体可用性。Raft是使用较为广泛的分布式协议。一定意义上讲，RAFT也使用了Quorum机制。

Leader-领导者，通常一个系统中是一主（Leader）多从（Follower）。Leader负责处理所有的客户端请求，并向Follower同步请求日志，当日志同步到大多数节点上后，通知Follower提交日志。

Follower-跟随者，不会发送任何请求。接受并持久化Leader同步的日志，在Leader告知日志可以提交后，提交日志。当Leader出现故障时，主动推荐自己为Candidate。

Candidate-备选者，Leader选举过程中的临时角色。向其他节点发送请求投票信息。如果获得大多数选票，则晋升为Leader。

Leader选举过程：

- 在一个任期内每个参与者最多投一票（持久化）
- 要成为Leader，必须拿到多数投票

Log Replication过程：

新Leader产生，Leader和Follower不同步，Leader强制覆盖Followers的不同步的日志。

![image-20230202184910336](D:\文档\文本\学习笔记\typora_imag\image-20230202184910336.png)

### Paxos协议

## 分布式实践

MapReduce

分布式KV

## 今日总结：

今日学习当今应用广泛的分布式理论，对于多个机群合作以提供更好性能提供了知识理论，从常见的分布式系统到系统模型，和分布式的基础理论，最后是共识协议。

# day10  微服务架构原理及特征

## 微服务架构

### 单体架构

优势：1、性能最高 2、冗余量小

劣势：1、debug困难 2、模块相互影响 3、模块分工、开发流程

### 垂直应用架构

按照业务线垂直划分

优势：1、业务独立开发维护

劣势：1、不同业务存在冗余 2、每个业务还是单体

### 分布式架构

抽出业务无关的公共模块

优势：1、业务无关的独立服务

劣势：1、服务模块bug可导致全站瘫痪 ；2、调用关系复杂 ； 3、不同服务冗余

### SOA架构

面向服务

优势：1、服务注册

劣势：1、整个系统设计是中心化的 ；2、需要从上至下设计 ；3、重构困难 

### 微服务架构

彻底的服务化

优势：1、开发效率 ；2、业务独立设计 ； 3、自下而上 ； 4、 故障隔离 

劣势：1、治理、运维难度 ； 2、观测挑战 ； 3、安全性 4、分布式系统 

核心要素：服务治理；可观测性； 安全 。

### 基本概念：

服务（service）：一组具有相同逻辑的运行实体。

实例（instance）：一个服务中，每个运行实体即为一个实例。

实例与进程的关系：实例与进程之间没有必然对应关系，可以一个实例对应一个或多个进程（反之不常见）

集群（cluster）：通常指服务内部的逻辑划分，包含多个实例。

常见的实例承载形式：进程，VM，k8s pod...

有状态/无状态服务：服务的实例是否存储了可持久化的数据（磁盘文件）。

服务间通信：

对于单体服务，不同模块通信只是简单的函数调用。

对于微服务，服务间通信意味着网络传输。

服务注册及发现：

问题：在代码层面，如何指定调用一个目标服务的地址（ip:port) ?

固定地址、DNS 有问题

解决思路：新增一个统一的服务注册中心，用于存储服务明道服务实例的映射。

### 服务实例上线及下线过程

### 流量特征

- 统一网关入口
- 内网通信多数采用RPC
- 网状调用链路

## 核心服务治理功能

### 服务发布（deployment）

即指让一个服务升级运行新的代码的过程。

服务发布的难点：

1. 服务不可用
2. 服务抖动
3. 服务回滚

蓝绿部署：简单稳定，需要两倍资源

灰度发布（金丝雀发布）：

### 流量治理

微服务架构下，我们可以基于地区、集群、实例、请求等维度，对端到端流量的路由途径进行精确控制。

### 负载均衡（Load Balance）

负责分配请求在每个下游实例上的分布

常见的LB策略：

- Round Robin
- Random
- Ring Hash 
- Least Request
- ......

### 稳定性治理

- 限流
- 熔断
- 躲在保护
- 降级  

## 字节跳动服务治理实践

### 重试的意义

重试可以避免掉偶发的错误，提高SLA(Service-Level Agreement)

降低错误率

降低长尾延时

容忍暂时性错误

避开下游故障实例

### 重试的难点

幂等性 ；重试风暴 ； 超时设置。

### 重试策略

限制重试比例：设定一个重试比例阈值（1%），重试次数占所有请求比例不超过该阈值。

防止链路重试：链路层面的防重试风暴是限制每层都发生重试，理想情况下只有最下一层发生重试。可以返回特殊的status表明 “请求失败，但别重试” 。

Hedged requests 

重试效果验证

## 今日总结：

今天主要学习了微服务架构的基本组件、工作原理、流量特征，微服务架构中核心的服务治理功能，最后介绍了字节内部微服务架构在实际应用中的稳定性治理经验和对充实策略的探索。

# day11 手把手教你系统设计——秒杀系统

## 系统设计方法论

系统：关联的个体按照某种特定规则运作则称工作的整体

设计：设想和计划

为了达成某种目的，通过个体组成整体的过程

如何做系统设计？

场景分析（Scenario）

存储设计（Storage）

服务设计（Service）

可拓展性（Scale）

发现系统的瓶颈？

火焰图分析 ； 链路追踪 ； 性能测试

保证可用性和稳定性？

链路梳理 ； 可观测性 ； 全链路测试 ； 稳定性控制 ； 容灾演练

## 电商介绍

商品：具有交易价值和属性的信息载体

SPU（Standard Product Unit）

SKU（Stock Keeping Unit ）

秒杀业务特点：

瞬时流量高

读多写少

实时性要求高

秒杀的挑战

高性能 ；资源成本 ；扩展性 ；防止超卖 ；反欺诈 ； 流量管理 ； 鲁棒性

如何设计秒杀系统

### 场景（scenario）

功能：

- 秒杀活动发布
- 秒杀商品详情
- 秒杀下单

并发：

- 万人参与秒杀
- QPS(Queries Per Second) 1w+
- TPS(Transactions Per Second) 1k+

### 存储（Storage）

![image-20230205135215299](D:\文档\文本\学习笔记\typora_imag\image-20230205135215299.png)

服务（Service）

子服务：

- 用户服务
- 风控服务
- 活动服务
- 订单服务

基础组件

- ID生成器
- 缓存组件
- MQ组件
- 限流组件

### 扩展（Scale)

系统架构图

![image-20230205135741668](D:\文档\文本\学习笔记\typora_imag\image-20230205135741668.png)

## 课程实践

秒杀项目流程图

![image-20230205210748046](D:\文档\文本\学习笔记\typora_imag\image-20230205210748046.png)

代码整体为common定义一些常量，迭代，结构体，config中为redis的一些配置，controller为SpringBoot中常规则的controller，convertet做数据转化，dao目录为数据操作，entity数据库实体，enums枚举，handler异常处理，limit先六组组件，lock分布式组件，model数据模型层，mq生产者和消费者，service业务逻辑层。

业务逻辑：

1、下单请求上传（用户id,活动id,活动spu id,活动sku id, 数量，价格，总价，收货地址id,  收货地址。）形成一个model，用model完成下面操作。

2、id生成器生成一个活动id，写入数据库，取商品spuid放入set中，查询商品是否存在，校验合法性。

3、扣库存，数据库操作检验于库存大小关系，防止超卖，成功后缓存放入redis

活动详情接口：同样缓存到redis内，先获取id，正常返回数据。

下单接口：首先登录验证（mock)，进行校验活动和商品状态，从后端获取秒杀价格，

项目主要优化途径：商品详情缓存到redis内.

## 今日总结

今天学习了商品秒杀项目的编写，从场景、存储、扩展三个方面来考虑设计电商系统，学习了整体项目的开发和面对秒杀场景下高效率需求的优化，增强了思考业务逻辑的能力。

# day12 黑灰产监控与防御

黑灰产：诈骗 ；薅羊毛 ；黄牛 ；博彩 ；引流 ；跑分 ；木马 ；钓鱼 ；病毒 ；拖库 ；盗号 ；勒索软件

黑灰产业发展趋势现今规模规模：

- 规模化：产业逐渐规模化，批量化实现攻击

- 组织化：形成以工作室的形式，内部有明确分工 

- 平台化：形成平台级别的爬虫，攻击成本大大降低

中国互联网黑灰产产业链较庞大

部分产业链部分

![image-20230206120059321](D:\文档\文本\学习笔记\typora_imag\image-20230206120059321.png)



## 常见的黑产技术分析

银行业务逻辑漏洞

人脸识别对抗

用户的定位分布

技术漏洞，业务逻辑漏洞都有可能产生难以预计的结果。

## 安全防护体系的建设

- 事前：情报监控：暗网、贴吧、TG、破解论坛；SDLC；        漏洞扫描    

- 事中：渗透测试 ；威胁感知：用户行为异常、接口数据异常、恶意流量检测；风控/安全策略    

- 事后：威胁建模；攻击溯源  

## 防范措施

1. 防微杜渐：尽量防止本单位敏感信息泄露在公共信息平台；对供应链信息进行严格管理；定期开展网络安全意识教育，加强对个人信息的管理，强化对个人口令的安全使用。
2. 收缩战线：让攻击面缩到最小，缩小防守半径，防守单位应加强对互联网暴露面的梳理，清理暴露在互联网上的测试环境、后台界面、远程维护端口以及和下级单位、业务合作单位等相关单位的联网边界资产等；
3. 守护核心：防守单位应针对集权类设备和核心资产进行最小化权限设置和管理，核心资产可设置白名单机制，加强访问行为的管控和监测分析，加强对核心资产访问的日志审计和监测预警，确保核心资产的安全可控。
4. 全方位监控：构建全方位的网络安全监测和运营体系，结合威胁情报持续进行网络安全监测分析和响应。构建网络安全主动防御体系，收集相关情报，全面部署网络安全威胁监测，通过专业的分析人员持续发现网络安全威胁，并进行及时的响应，实战化攻防演习作为检验网络安全保障体系。

# day13 定时任务发展之路

## 分布式定时任务

Windows批处理：.bat文件或者使用任务计划程序功能。

Linux命令：Cronjob

单机定时任务：

跨平台：Timer， Ticker

执行多个任务：ScheduledExecutorService

任务调度：Quartz

#### 分布式定时任务：

- 平台管理化
- 分布式部署
- 支持海量数据

定时任务是指系统为了自动完成特定任务，实时、延时、周期性完成任务调度的过程。

分布式定时任务是把分散的、可靠性差的定时任务纳入统一的平台，并实现集群管理调度和分布式部署的一种定时任务的管理方式。

#### 按触发时机分类：

- 定时任务：特定时间触发
- 延时任务：延时触发
- 周期任务：固定周期时间，或固定频率周期调度触发

#### 特点：

- 自动化：全自动完成定时任务的调度和执行
- 平台化：基于平台化的思维管控一系列的分布式定时任务
- 分布式：在分布式系统环境下运行任务调度，突破单机定时任务的性能瓶颈
- 伸缩性：采用集群方式部署，可以随时按需扩缩容
- 高可用：单点故障不影响最终结果，可以做到故障转移

#### 执行方式：

- 单机任务：随机触发一台机器执行任务，适用于计算量小、并发度低的任务
- 广播任务：广播到所有机器上执行同一任务
- Map任务：一个任务可以分数多个子任务，每个子任务负责一部分的计算。
- MapReduce任务：在Map任务的基础上，还可以对所有子任务的结果做汇总计算，是用于计算量大，并且需要对子任务结果做汇总的任务。

## 核心架构：

分布式定时任务核心要解决触发、调度、执行三个关键问题

- 触发器：Trigger，解析任务， 生成触发事件
- 调度器：Scheduler， 分配任务， 管理任务生命周期
- 执行器：Executor， 获取执行任务单元，执行任务逻辑

还需要提供一个控制台（Admin），提供任务管理和干预的功能。

### 控制台：

- 任务：Job， 任务元数据
- 任务实例：Jobinstance，周期任务会发生多个任务实例
- 任务结果：JobResult， 任务实例运行的结果
- 任务历史：JobHistory， 用户可以修改任务信息，任务实力对应的任务元数据可以不同，因而使用任务历史存储。

任务元数据（Job）是用户对人物属性定义，包括任务类型调度时机，执行行为等。

任务实例（Job instance）是一个确定的Job的一次运行实例。

### 触发器：

核心职责：给定一系列任务，解析他们的触发规则，在规定的时间点触发任务的调度

设计约束：

- 需要持支大量任务
- 需要支持秒级的调度
- 周期任务需要多次执行
- 需要保证秒级扫描的高性能，并避免资源浪费

方案：

1、定期扫描+演示消息

2、时间轮

高可用的解决思路：

- 存储上，不同国别，业务做资源隔离
- 运行时，不同国别，业务分开执行
- 部署时，采用多级放集群化部署，避免单点故障，通过数据库锁或分布式锁保证任务只被触发一次。

### 调度器

#### 节点选择

随即节点执行：选择集群中一个可用的执行阶段执行调度任务。适用场景：定时对账。

广播执行：在集群中所有的执行节点分发调度任务并执行。适用场景：批量运维。

分片执行：按照用户自定义分片逻辑进行拆分，分发到集群中不同节点并行执行，提升资源利用效率。使用场景：海量日志统计。

任务名片：通过任务分片来提高任务执行的效率和资源利用率。

任务编排：使用有向无环图DAG（Directed Acyclic Graph）进行可视化任务编排。

故障转移：确保部分执行单元任务失败时，任务最终失败。

高可用：调度器可以集群部署，做到完全的无状态，靠消息队列的重试机制保障任务一定会被调度。

### 执行器

基于注册中心，可以做到执行器的弹性扩缩容。

## 业务应用

所有需要定时、延时、周期性执行任务的业务场景，都可以考虑使用分布式定时任务。

## 今日总结：

今天学习了面对过年发红包等应用场景下，分布式定时任务的使用方式和整体架构。了解了分布式定时任务的相关知识，在海量请求下做到安全可用的机制。

# day14 消息队列

面对：

- 系统奔溃
- 服务处理能力有限
- 链路耗时长尾
- 日志如何处理

解决方案：

- 解耦
- 削峰
- 异步
- 日志处理

消息队列（MQ），指保存消息的一个容器，本质是某个队列。但这个队列需要支持高吞吐，高并发，并且高可用。

## 消息队列-Kafka

创建集群-》新增Topic-》编写生产者逻辑-》编写消费者逻辑。

Topic：逻辑队列，不同Topic可以建立不同的Topic。

Cluster：物理集群，每个集群中可以建立多个不同的Topic。

Consumer：消费者，负责消费Topic中的消息。

ConsumerGroup：消费者组，不同组Consumer消费进度互不干涉。

Offset：消息在Partition内的相对位置信息，可以理解为唯一ID，在Partition内部严格递增。

每个分片有多个Replica，Leader Replica将会从ISR中选出。

## Kafka架构

![image-20230209195211461](D:\文档\文本\学习笔记\typora_imag\image-20230209195211461.png)

ZooKeeper：负责存储器群员信息，包括分区信息等。

### Producer-批量发送

### Producer-数据压缩

通过压缩，减少消息大小，目前支持Snappy，Gzip，LZ4，ZSTD压缩算法。

### Broker-数据的存储

### Broker-消息文件结构

数据路径：/Topic/Partition/Segment/（log|index|timeindex|...)

### Broker-磁盘结构

移动磁头找到对应磁道，磁盘转动，找到对应扇区，最后写入。寻到成本较高，因此顺序写可以减少寻道所带来的时间成本。

### Broker-顺序写

采用顺序写的方式进行写入，以提高写入效率。

### Broker-如何找到消息

Consumer通过发送FetchRequest请求消息数据，Broker会将指定Offset处的消息，按照时间窗口和消息大小窗口发送给Consumer。

### Broker偏移量索引文件

二分找到目标文件。

### Broker时间戳索引文件

二分找到小于目标时间戳最大的索引位置，再通过寻找offset的方式找到最终数据。

### Broker-传统数据拷贝

### Broker-零拷贝

### Consumer-消息的接收端

### Consumer-Low Level

通过动手进行分配，哪一个Consumer消费哪一个Partition完全由业务来决定。

### Consumer-Hign Level

### Consumer Rebalance

### Kafka问题总结

1. 运维成本高
2. 对于负载不均衡的场景，解决方案复杂
3. 没有自己的缓存，完全依赖Page Cache
4. Controller和Coordinator和Broker在同一进程中，大量IO会造成其性能下降。

## 消息队列-BMG

兼容Kafka协议，存算分离，云原生消息队列。

架构图

![image-20230209203313561](D:\文档\文本\学习笔记\typora_imag\image-20230209203313561.png)

运维操作对比

![image-20230209203403154](D:\文档\文本\学习笔记\typora_imag\image-20230209203403154.png)

### HDFS写文件流程

随机选择一定数量的DataNode进行写入。

### Broker-Partition 状态机

![image-20230209203945905](D:\文档\文本\学习笔记\typora_imag\image-20230209203945905.png)

保证对于任意分片在同一时刻只能在一个Broker上存活。

### Broker-写文件流程

![image-20230209204043742](D:\文档\文本\学习笔记\typora_imag\image-20230209204043742.png)

### Broker-写文件 Failover

如果DataNode节点挂了或者其他原因导致我们写文件失败。

### Proxy

### 多机房部署

### 泳道消息

BOE：Bytedance Offline Environment, 是一套完全独立的线下机房环境。

PPE：Product Preview Environment， 产品预览环境。

BOE：多人同时测试，需要等待上一个人测试完成。每多一个测试人员，都需要重新搭建一个相同配置的Topic，造成人力和资源的浪费。

PPE：对于PPE的消费者来说，资源没有生产环境多，所以无法承受生产环境的流量。

解决主干泳道隔离问题以及用到资源重复创建问题。

### Databus

- 简化消息队列客户端复杂度
- 解耦业务与Topic
- 缓解集群压力，提高吞吐。

### Mirror

使用Mirror通过最终一致的方式，解决跨Region读写问题。

### Index

直接在BMQ中将数据结构化，配置索引DDL，异步构建索引后，通过Index Query服务读出数据。

### Parquet

Apache Parquet是Hadoop生态圈中一种新型列式存储格式，它可以兼容Hadoop生态圈中大多数计算框架（Hadoop，Spark等），被多种查询引擎支持（Hive，Impala，Drill等）。

直接在BMQ中将数据结构化，通过Parquet Engine，可以使用不同的方式构建Parquet格式文件。

## 消息队列-RocketMQ

### RocketMQ高级特性

#### 事务场景

#### 事务消息

#### 延迟发送

#### 延迟消息

#### 处理失败

#### 消费重试和死信队列

## 今日总结

今天学习了什么是消息队列，消息队列的应用场景，常用的消息队列——BMG和RocketMQ及其特性，完善了自身对高吞吐，高并发，高可用需求的解决能力。

# day16 带你认识存储的本质 - 状态

## 01、经典案例

数据的产生

数据的流动

数据的持久化

## 02、存储&数据库简介

存储系统特点

- 性能敏感
- 既“简单”又“复杂”
- 容易受硬件影响

![](D:\文档\文本\学习笔记\typora_imag\8e6c87e4-94df-49fe-93c4-fea8a7047211.png)

- 缓存很重要，贯穿整个存储体系
- 拷贝很昂贵，应该尽量减少
- 硬件设备五花八门，需要有抽象统一的接入层

### RAID技术

Redundant Array of Inexpensive Disks 

### 数据库

#### 关系型：

关系=集合=任意元素组成的若干有序偶对反应了事物间的关系。

关系代数=对关系做运算的抽象查询语言

SQL=一种DSL=方便人类阅读的关系代数表达形式

##### 特点：

- 结构化数据友好
- 支持事务（ACID）
- 支持复杂查询语言

##### 非关系型数据库特点：

- 版结构化数据友好
- 可能支持事务（ACID）
- 可能支持复杂查询语言

#### 结构化数据管理：

关系型数据库以表形式管理

非关系型数据库则需要自行定义，管理结构

#### 事物具有：

-  Atomicity, 事务内的操作要么全做，要么不做
- Consistency， 事务执行前后，数据状态是一致的
- Isolation， 可以隔离多个并发事务，避免影响
- Durability，事务一旦提交成功，数据保证持久性

#### 复杂查询能力：

查询能力更加灵活简介

Everything is D(omain) S(pecific) L(anguage)

## 03、主流产品剖析

### 3、1 单机存储系统

单机存储=单个计算机节点上的存储系统，一般不涉及网络交互。

Linux文件管理，文件系统繁多，但都遵循VFS的统一抽象接口。

Linux文件系统的两大数据结构：Index Node & Directory Entry

#### Key-Value存储

常见数据结构：LSM-Tree， 某种程度上牺牲读性能，追求写入性能。

### 3、2分布式存储系统-HDFS

HDFS：堪称大数据时代的基石

HDFS核心特点：

- 支持海量数据存储
- 高容错性
- 弱POSIX语义
- 使用普通x86服务器，性价比高

Cephalexin：开源分布式存储系统里的**万金油**

核心特点：

- 一套系统支持对象接口、块接口、文件接口、但一切皆是对象
- 数据写入采用主备复制模型
- 数据分布模型采用CRUSH（HASH+权重+随机抽签）算法

### 3、3单机数据库-关系型数据库

Oracle，MySQL，PostgreSQL

### 3、4单机数据库-非关系型数据库

MongoDB

- 面向文档存储
- 文档可序列化成JSON/BSON，支持嵌套
- 存在collection，collection=文档的集合
- 存储和构建索引能力依赖wired Tiger引擎
- 4.0后开始支持事务（多文档、跨分片多文档等）
- 常用client/SDK交互，可通过插件转移支持弱SQL

Redis

- 数据结构丰富（hash表，set，zset，list）
- C语言实现，超高性能
- 主要基于内存，但支持AOF/RDB持久化
- 常用redis-cli多语言SDK交互

Elasticsearch

- 面向文档存储
- 文档可序列化成JSON，支持嵌套
- 存在index，index=文档的集合
- 存储和构建索引能力依赖Lucene引擎
- 实现了大量搜索数据结构&算法
- 支持RESTFUL API，也支持弱SQL交互

### 3、5分布式数据库

#### 解决容量问题 

存储节点池化，动态扩缩容

#### 解决弹性问题

动态扩缩容可解决

#### 解决性价比

同样是池化技术

## 04、新技术演进

软件架构变更

AI增强

新硬件革命

### SPDK（Storage Performance Development Kit）

Bypass OS kernel已经成为一种趋势

Kernel Space -》UserSpace

中断-》轮询

无锁数据结构

### AI & Storage

AI领域相关技术，如Machine Learning在很多领域，如推荐、风控、视觉领域证明了有效性。

### 高性能硬件

1. RDMA网络
2. Persistent Memory
3. 可编程交换机
4. CPU/GPC/DPU

## 今日总结：

今日从宏观角度了解了从存储系统到数据库系统再到分布式结构数据库系统，了解学习了整个数据存储过程中遇到的问题和解决方案，以及新技术演进的发展趋势，学习到了数据存储的思维。

# day17 MySQL - 深入理解RDBMS

关系型数据库

## 01、经典案例

红包雨

### RDBMS

- 事务（Transaction）：是由一组SQL语句组成的一个程序执行单元（Unit），它需要满足ACID特性。

#### 事物具有（ACID）：

-  Atomicity, 事务内的操作要么全做，要么不做
-  Consistency， 事务执行前后，数据状态是一致的
-  Isolation， 可以隔离多个并发事务，避免影响
-  Durability，事务一旦提交成功，数据保证持久性

高并发

高可靠/高可用

## 02、发展历史

人工管理

文件系统

### DBMS

#### 网状模型

#### 层次模型

#### 关系模型

![image-20230213112627328](D:\文档\文本\学习笔记\typora_imag\image-20230213112627328.png)

### SQL（Structured Query Language）语言

- 语法风格接近自然语言
- 高度非或称话
- 面向集合的操作方式
- 语言简洁、易学易用

## 03、关键技术

SQL语句的一生

![image-20230213114736178](D:\文档\文本\学习笔记\typora_imag\image-20230213114736178.png)

### SQL引擎-Parser

解析器（Parser）一般分为分析（Lexical anslysis），语法分析（Syntax anslysis），语义分析（Semantic analyzer）等步骤

### SQL引擎-Optimizer

基于规则的优化（RBO Rule Base Optimizer）

Scan优化

- 唯一索引
- 普通索引
- 全表扫描

数据库索引：是数据库管理系统中辅助数据结构，以协助快速查询，更新数据库表中数据。目前数据库中最常用的索引是通过B+树实现的。

### SQL引擎-Executor

火山模型

![image-20230213133040787](D:\文档\文本\学习笔记\typora_imag\image-20230213133040787.png)

优点：

每个算子独立抽象实现，相互之间没有耦合，逻辑结构简单；

缺点：

每计算一条数据有多次函数调用开销，导致CPU效率不高

向量化 ； 编译执行 LLVM动态编译技术

###  存储引擎-InnoDB

![](D:\文档\文本\学习笔记\typora_imag\download (2).png)

### 存储引擎-Buffer Pool

MySQL中的优化思路为：对数据进行冷热分离，将LRU链表分成两部分，一部分用来存放冷数据，即刚从磁盘读进来的数据，另一部分用来存放热点数据，即进场被访问到的数据。

### 存储引擎-Page

![image-20230213134928676](D:\文档\文本\学习笔记\typora_imag\image-20230213134928676.png)

### 存储引擎-B+ Tree

页面内：

页目录中使用二分法快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可快速找到指定的记录。

从根到叶：

中间节点存储

### 事务引擎-Atomicity与Undo Log

Undo Log是逻辑日志，记录的是数据的增量变化，利用Undo Log可以进行事务回滚，从而保证事务的原子性。同时也实现了多版本并发控制（MVCC），解决读写冲突和一致性读的问题。

### 事务引擎-Isolation与锁

MVCC（Mutil Version Concurrency Control）的意义：

- 读写互不阻塞
- 降低死锁概率
- 实现一致性读

Undo Log在MVCC的作用

- 每个事务有一个单增的事务ID
- 数据页的行记录中包括了DB_ROW_ID，DB_TRX_ID，DB_ROLL_PTR；
- DB_ROLL_PTR将数据行的所有快照记录都通过链表的数据结构串联了起来。

### 事务引擎-Durability与Redo Log

保证事务结束后对数据修改永久的保存：

方案一：事务提交前页面写盘

方案二：WAL（Write-ahead logging）

redo log是物理日志，记录的是页面的变化，它的作用是保证事务持久化。如果数据写入磁盘前发生故障，重启MySQL后会根据redo log重做。

## 04、企业实践

春节红包雨挑战

大流量-Sharding

扩容

代理连接池

稳定性&可靠性：

3AZ高可用  、  HA管理

![image-20230213142313321](D:\文档\文本\学习笔记\typora_imag\image-20230213142313321.png)

## 今日总结

今天学习了RDBMS的整个历史、框架、和企业实践的相关分析。详细学习了关系型数据库MySQL的关键技术，其能快速查询事务，存储事务，并且能保证事务的ACID性质，可以比较好得应对红包雨这样的业务需求，对大流量需求进行稳定性和可靠性的技术支持，并且在活动结束后实现扩缩容保证服务始终能够应对需求又能保证服务成本。

# day18 Redis - 大厂程序员是怎么用的

面对业务压力，使用Redis数据分冷热，将热数据存储到内存中。

## Redis基本工作原理

RESP协议

- 数据从内存中读写
- 数据保存到硬盘上防止重启数据丢失
  - 增量数据保存到AOF文件
  - 全量数据RDB文件
- 单线程处理所有操作命令，顺序执行

## Redis应用案例

### 1、连续签到

String数据结构

数据结构-sds

- 可以存储字符串、数字、二进制数据
- 通常和expire配合使用
- 场景：存储技术、Session

### 2、消息通知

用list作为消息队列，List数据结构Quick list，Quick list由一个双向链表和list pack实现。

### 3、计数

一个用户有多项技术需求，可通过hash结构存储。

Hash数据结构dict

- **rehash**：rehash操作是将ht[0]中的数据，全部迁移到ht[1]中。数据量小的场景下，直接将数据从ht[0]拷贝到ht[1]速度是较快的。数据量大的场景，例如存有上百万的KV时，迁移过程将会明显阻塞用户请求。
- **渐进式rehash**：为避免出现这种情况，使用了rehash方案。基本原理就是，每次用户访问时都会**迁移少量数据**。将整个迁移过程，平摊到所有的访问用不请求过程中。

### 4、排行榜

积分变化时，排名要实时变更。

- 结合dict后，可实现通过key操作跳表的功能。

zet数据结构 zskiplist

- 查找数字7的路径，head,3,3,7
- 结合dict后，可实现通过key操作跳表的功能

### 5、限流

- 要求1秒内放行的请求为N，超过N则禁止访问

string数据结构

### 6、分布式锁

并发场景，要求一次只能有一个协程执行。执行完成后，其他等待中的协程才能执行。

可以使用redis的setnx实现，利用了两个特性

- Redis是单线程执行命令
- setnx只有未设置过才能执行成功

## Redis使用注意事项

### 1、大Key、热Key

#### 大key的定义

string类型 中value的字节数大于10KB即为大key

Hash/Set/Zset/list等复杂数据结构类型 元素个数大于5000个或者总value字节数大于10MB即为大Key

大Key的危害

- 读取成本高
- 容易导致慢查询（过期、删除）
- 主从复制异常、服务阻塞 无法正常响应请求

业务侧使用大Key的表现

- 请求Redis超时报错

#### 消除大Key的方法

##### 1、拆分

将大Key拆分为小Key。例如一个String拆分为多个String

##### 2、压缩

将value压缩后写入redis，读取时解压后再使用。压缩算法可以是gzio、snappy、lz4等。通常情况下，一个压缩算法压缩率高，则解压耗时就长。需要对实际数据进行测试后，选择一个合适的算法。如果存储的是JSON字符串，可以考虑使用Message Pack进行序列化。

##### 3、对于集合类结构hash,list,set,zset

(1) 拆分：可以hash取余、位掩码的方式决定放在哪个key中。

(2)区分冷热：如榜单列表场景使用zset，只缓存前10页数据，后续数据走db

#### 热key的定义

用户访问一个key的**QPS**特别高，导致Server实例出现CPU负载突增或者不均的情况。热key没有明确的标准，QPS超过500就有可能被识别为热Key。

#### 解决热Key的方法

##### 1、设置localcache

在访问Redis前，在业务服务侧设置**Localcache**，降低访问Redis的QPS。LocalCache中缓存过期或未命中，则从Redis中将数据更新到LocalCache。Java的Guava、Golang的Bigcache就是这类LocalCache。

##### 2、拆分

将key:value这一个热Key复制写入多份，例如key1：value,key2:value, 访问的时候访问多个key，但value是同一个，因此将QPS分散到不同实例上，降低负载。代价是：更新时需要更新多个key，存在数据短暂不一致的风险。

##### 使用Redis代理的热Key承载能力

字节跳动的Redis访问代理就具备热Key承载能力。本质上是结合了“热Key发现”，“LocalCache”两个功能。

### 2、慢查询场景

容易导致Redis慢查询的操作

1）批量操作一次性传入过多的key/value，如mset/hmset/sadd/zadd等O(n)操作，建议单批次不要超过100，超过100之后性能下降明显。

2）zset大部分命令都是O(log(n))，当大小超过5k以上时，简单的zadd/zrem也可能导致慢查询

3）操作的单个value过大，超过10kb。也即，避免使用大key

4）对大key的delete/expire操作也可能导致慢查询，Redis4.0之前不支持异步删除unlink，大key删除会阻塞Redis

### 3、缓存穿透、缓存雪崩

缓存穿透：热点数据查询绕过缓存，直接查询数据库

缓存雪崩：大量缓存同时过期

#### 缓存穿透的危害

（1）查询一个一定不存在的数据

通常不会缓存不存在的数据，这类查询请求都会直接打到db，如果有系统bug或认为攻击，那么容易导致db响应慢甚至宕机。

（2）缓存过期时

在高并发场景下，一个热key如果过期，会有大量请求同时击穿至db，容易影响db性能和稳定。同一时间有大量key过期时，也会导致大量请求落到db上，导致查询变慢，甚至出现db无法响应新的查询。

如何减少缓存穿透

（1）缓存空值

如一个不存在的userID。这个id在缓存和数据库中都不存在。则可以缓存一个空值，下次再查缓存直接返回空值。

（2）布隆过滤器

通过boom filter算法来存储合法key，得益于该算法超高的压缩率，只需占用极小的空间就能存储大量key值。

## 今日总结

今天学习了Redis的使用。学习了Redis在互联网发展过程出现的背景，Redis在工作中使用的案例和注意事项，完善了整个数据库中快速查询的情况下的需求。

# day19 ClickHouse - 你没有见过的列存储

## 01、数据库基本概念

- OLTP数据库：OLTP（Online transactional procession）数据库是一种高速分析数据库，专为多个用户**执行大量事务而设计。**
- OLAP数据库：OLAP（Online analytical procession）数据库旨在同时分析多个数据维度，帮助团队更好地**理解其数据中的复杂关系**。

### OLAP数据库

- 大量数据的读写，PB级别的存储
- 多维分析，复杂的聚合函数
- 窗口函数，自定义UDF（User Define Fucntion）
- 离线/实时分析

## SQL

一种编程语言，目前几乎所有的关系型数据库都是用SQL**（Structured Query Language）**编程语言来查询、操作和定义数据，进行数据访问控制。

一个简单的SQL查询包含SELECT关键词，型号（“*”）也可以用来指定查询也可以用来指定查询应当返回查询表所有字段，可选的关键词和子句。

From子句指定了选择的数据表。FROM子句也可以包含JOIN二层子句来为数据表的连接设置规则。

WHERE子句后接一个比较谓词以限制返回的行。WHERE子句仅保存返回结果里使得比较谓词的值为True的行。

Group By子句用于将若干含有相同值的行合并。

Group By通常与SQL聚合函数连用，或者用于清除数据重复的行。Group By子句要用在WHERE子句之后。

### SQL的优点

1、标准化，ISO和ANSI是长期建立使用的SQL标准库标准

2、高度非过程化，用SQL进行数据操作，用户只需提出“做什么”，而不必指明“怎么做”，因此用户无需了解存取路径，存取路径的选择以及SQL语句的操作过程由系统自动完成。不但减轻用户负担，而且有利于提高数据独立性。

3、以同一种语法结构提供两种使用方式，用户可以在终端上直接输入SQL命令对数据库进行操作。作为嵌入式语言，SQL语句能够嵌入到高级语言（C，C#，JAVA）程序中，供程序员设计程序时使用。在两种不同的使用方式下，SQL的语法结构基本上是一致的。

4、语言简洁，易学致用：SQL功能极强，但由于设计巧妙，语言十分简洁、完成数据定义、数据操纵、数据控制的核心只用了9个动词：CREATE、ALTER、DROP、SELECT、INSERT、UPDATE、DELETE、GRANT、REVOKE。

### 数据库架构

![image-20230216170203549](D:\文档\文本\学习笔记\typora_imag\image-20230216170203549.png)

Parser：词法分析、语法分析、生成AST树（Abstract syntax tree）

Analyzer：变量绑定、类型推导、语义检查、安全、权限检查

、完整性检查等，为生成计划做准备。

Optimizer：为查询生成性能最优的执行计划，进行代价评估。

Executor：将执行计划翻译成可执行的物理计划并驱动器执行。

### 存储引擎

#### 1、管理内存数据结构

- 索引
- 内存数据
- 缓存
- Querycache 
- Datecache 
- Indexcache 

#### 2、管理磁盘数据

- 磁盘数据的文件格式

- 磁盘数据的增删改查

#### 3、读写算子

- 数据写入逻辑

- 数据读取逻辑

## 02、列式存储

### 列式存储的优点

- 数据压缩可以使读的数据量更少，在IO密集型计算中获得更大的性能优势
- 相同类型压缩效率更高
- 排序之后压缩效率更高
- 可以针对不同类型使用不同的压缩算法

#### Run-length encoding

- 压缩重复的数据
- 可以在压缩数据上直接计算

#### Delta encoding

- 将数据存储为连续数据之间的差异，而不是直接存储数据本身
- 特定算子也能直接在压缩数据上计算

#### 数据选择：

- 可以选择特定的列作计算而不是读所有列
- 对聚合计算友好

#### 延迟物化：

物化：将列数据转化为可以被计算或者输出的行数据或者内存数据结果的过程，物化后的数据通常可以用来做数据过滤，聚合计算，Join。

延迟物化：尽可能推迟吴化操作的发生。

- 缓存友好
- CPU/内存带宽友好
- 可以利用到执行计划和算子的优化，例如filter
- 保留直接在压缩列作计算的机会

#### 向量化：

- SIMD
- 数据和树
- 执行模型

**SIMD（single instruction multiple data），**相对于现在多核CPU，其都有能力用一条指令执行多条数据。

SIMD程序使用的指令集有SSE合AVX系列，AVX有AVX-256和AVX-512，SSE提供128-bits的寄存器，AVX-256提供256-bits，AVX-512提供512bits的寄存器。如果这时候CPU也可以并行的计算我们写的代码，那么理论上我们的处理速度就会是之前代码的100倍，SIMD指令就可以完成这样的工作，用SIMD指令完成的代码设计和执行的逻辑就叫做向量化。

数据格式要求：

- 需要处理多个数据，因此数据需要是连续内存
- 需要明确数据类型

执行模型要求：

- 数据要求按批读取
- 函数的调用需要明确数据类型

列存数据库适合设计出这样的执行模型，从而使用向量化技术：

- 按列读取
- 每种列类型定义数据读写逻辑
- 函数按列类型处理

![image-20230216175502973](D:\文档\文本\学习笔记\typora_imag\image-20230216175502973.png)

## 03、Click House存储设计

 表定义和结构

集群架构

引擎架构

### 存储架构

part和partition

- part是物理文件夹的名字
- partition是逻辑结构

part和column

- 每个column都是一个文件
- 所有的column文件都在自己的part文件夹下

column和index

- 一个part有一个主键索引
- 每个column都有列索引

Hash index

1. 将输入的key通过一个hashfunction映射到一组bucket上
2. 每个bucket都包含一个指向一条记录的地址
3. 哈希索引在查找的时候只适用于等值比较

### 索引设计

B- Tree

1. 数据写入是有序的，支持增删查改
2. 每个节点有多个孩子节点
3. 每个节点都按照升序排列key值
4. 每个key有两个指向左右孩子节点的引用
   - 左孩子结点保存的key都小于当前key
   - 右孩子节点保存的key都大于当前key

B+ Tree

1. 所有的数据都存储在叶子节点，非叶子节点只保存key值
2. 叶子节点维护相邻叶子节点的引用
3. 可以通过key值做二分查找，也可以通过叶子节点做顺序访问

- 对于大数据量，B(B+)-Tree深度太高
- 索引数据量太大，多个列如何平衡查询和存储——LSM-Tree
- OLAP场景写入量非常大，如何优化写入

**Log-structured merge-tree(LSM Tree)**是一种为大吞吐写入场景而设计的数据结构

- 着重优化顺序写入
- 主要数据结构
  1. SSTables
  2. Memtable

#### SSTables

1. Key按顺序存储到文件中，称为segment
2. 包含多个segment
3. 每个segment写入磁盘后都是不可更改的，新加的数据只能生成新的segment

#### Memtable

- 在内存中的数据保存在memtable中，大多数实现都是一棵Binary search tree
- 当memtable存储的数据达到一定的阈值的时候，就会按顺序写入到磁盘。

#### 数据查询

- 需要从最新的segment开始遍历每个key
- 也可以每个segment键一个索引

#### Compaction（合并）

- Compaction将多个segment合并成一个segments的过程
- 一般是由一个后台线程完成
- 不同的segments写入新的segment的时候也是需要排序，形成新的segment之后，旧的segment文件就会被删除

### 索引实现

数据按照主键顺序依次做排序

- 优先按照UserID做排序
- 再按照URL排序
- 最后是Event Time

数据被划分为granules

1. granules是最小的数据读取单元
2. 不同的granules可以并行读取

每个granule都对应primary。idx里面的一行

默认每8192行记录主键的一行值，primary.idx需要被全部加载到内存里面，里面保存的每一行数据称为一个index mark，每个列都有这样一个mark文件

- mark文件保存的是每个granules的物理地址
- 每一列都有一个自己的mark文件

mark文件里面的每一行保存两个地址：

1. block_offset：用于定位一个granule的压缩数据再物理文件中的位置，压缩数据会以一个block为单位解压到内存中
2. granule_offset，用于定位一个granule在解压之后的block中的位置

缺陷：数据按照key的顺序做排序，因此只有第一个key的过滤效果好，后面的key过滤效果以来第一个key的基数大小

### 查询优化

secondary index：在URL列上构建二级索引

构建多个主键索引

- 再建一个表，使用需要优化的字段做主键的第一位
- 建一个物化视图
- 使用Projection

使用Projection

projection：类似物化视图，但是不是将数据写入新的表，而是存储在原始表中，以一个列文件的形式存在。

### 数据合并

数据可见性：

1. 数据而合并过程中，未被合并的数据对查询可见
2. 数据合并完成后，新part可见，被合并的part被标记删除

### 数据查询

1. 通过主键找到需要读的mark
2. 切分marks，然后并发的调度reader
3. Reader通过mark block——offset得到需要读的数据文件的偏移量
4. Reader通过mark granule_offset得到解压后数据的偏移量
5. 构建列式filter做数据过滤

## 今日总结

今天从数据库出发，学习了列存储的相关特性，学习了众多优化存储数据，查询数据的技巧。
