# IDEA操作技巧

新建project，之后删除原来的module， 之后导入新module

## 所有的Java版本配置：

### 1、Maven中

```xml
    <properties>
        <java.version>17</java.version>
    </properties>
```

![image-20240611190103645](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190103645.png)

### 2、Java Compiler

![image-20240611190213915](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190213915.png)

### 3、Project Structure

![image-20240611190310341](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190310341.png)

![image-20240611190344691](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190344691.png)

![image-20240611190403531](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190403531.png)

![image-20240611190431662](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190431662.png)

### 4、Maven配置

![image-20240611190532848](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190532848.png)

![image-20240611190556660](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240611190556660.png)



## 导入模块后没有文件

### 1、选择项目结构

![image-20240613114009635](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240613114009635.png)

选择**源文件** Sources 

![image-20240613114128934](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240613114128934.png)





package打包时出现包不存在的错误时：

1、先更换maven版本，在

![image-20240613155129299](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240613155129299.png)

2、配置： https://blog.csdn.net/wngpenghao/article/details/128579459