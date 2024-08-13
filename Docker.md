# Docker

快速构建、运行、管理应用的工具

Docker打印所有容器（包括关闭的）

```dockerfile
docker ps -a
```

正在运行的

```
docker ps
```

删除容器

```
docker rm <container_id>
```

停止容器

```
docker stop <container_id>
```

```
docker run -d \
--name mysql \
-p 3306:3306 \ 
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123 \
mysql
```

## Docker基础知识

![image-20240723192837909](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240723192837909.png)





## Docker 数据卷

作用：宿主机目录和docker容器内目录做一个关联



## MySql容器的数据挂载

```
docker run -d \
--name mysql \
-p 3307:3306 \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123 \
-v /root/mysql/data:/var/lib/mysql \
-v /root/mysql/init:/docker-entrypoint-initdb.d \
-v /root/mysql/conf:/etc/mysql/conf.d \
mysql
```

## 自定义镜像

### DokerFile

DockerFile就是一个文本文件， 其中包括一个个的指令（Instruction），用指令来说明要执行什么操作来构建镜像。将来Docker可以根据DockerFile帮我们构建镜像。

![image-20240724145130585](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240724145130585.png)

## 网络

加入自定义网络的容器才可以通过容器互相访问， Docker的网络操作命令如下：

![image-20240724223132701](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20240724223132701.png)



## 部署流程

1. 部署Java应用

2. 部署nginx

   - ```
     docker run -d \
         --name nginx \
         -p 18080:18080 \
         -p 18081:18081 \
         -v /root/nginx/html:/usr/share/nginx/html \
         -v /root/nginx/nginx.conf:/etc/nginx/nginx.conf \
         --network kennem \
         nginx
     ```





## DockerCompose

![image-20240726213413347](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240726213413347.png)

`DockerCompose`通过一个单独的`docker-compose.yml`模板文件（YAML格式）来定义一组相关联的应用容器， 帮助我们实现多个相互关联的Docker容器的快速部署。

```yaml
version: "3.8"

services:
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "./mysql/conf:/etc/mysql/conf.d"
      - "./mysql/data:/var/lib/mysql"
      - "./mysql/init:/docker-entrypoint-initdb.d"
    networks:
      - hm-net
  hmall:
    build: 
      context: .
      dockerfile: Dockerfile
    container_name: hmall
    ports:
      - "8080:8080"
    networks:
      - hm-net
    depends_on:
      - mysql
  nginx:
    image: nginx
    container_name: nginx
    ports:
      - "18080:18080"
      - "18081:18081"
    volumes:
      - "./nginx/nginx.conf:/etc/nginx/nginx.conf"
      - "./nginx/html:/usr/share/nginx/html"
    depends_on:
      - hmall
    networks:
      - hm-net
networks:
  hm-net:
    name: hmall
```

