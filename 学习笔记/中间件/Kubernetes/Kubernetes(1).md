---
title : 'Kubernetes(1)'
date : 2024-11-30T22:30:13+08:00
lastmod: 2024-11-30T22:20:13+08:00
description : "Kubernetes(1)"  
categories : ["Kubernetes"]
tags : ["Kubernetes"]
---

# Kubernetes(1)

## 1、课程背景和目标

### 背景

- 从IT基础设施主计划向容器化转换
- 从人工式运维工作模式向自动化运维模式转换
- 从自动化运维体系向全体系智能化运维模式转换

## 容器

- 主机
- 虚拟机
- 容器

为了降低虚拟机造成的物理主机资源浪费，提高物理主机的资源利用率，并能够提供像虚拟机一样良好的应用程序隔离环境，人们把这种轻量级的虚拟机，称为“容器”。 

![image-20241130202536541](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241130202536541.png)

## 容器管理工具

容器管理工具类似于虚拟机管理工具，主要用于容器的创建、启动、关闭、删除等。

容器管理工具有：

1. docker公司的docker
2. 阿里的Pouch
3. LXC，LXD，RKT等

## 容器编排部署工具

容器管理工具可以完成容器的基础管理，但是容器的应用并不是只能进行简单应用部署的，可以使用容器完成企业中更加复杂的应用部署，当需要对多应用的系统进行部署时，就需要**更加复杂的工具来完成对容器进行应用的编排**，这就是我们所说的容器编排部署工具。

### docker三剑客

- `docker machine`
- `docker compose`
- `docker swarm`

### mesos + marathon

### Kubernetes

## K8s功能

- 自动装箱
  - 基于容器对应用运行环境的资源配置要求自动部署应用容器
- 自动修复（自愈能力）
  - 当容器失败时，会对容器进行重启
  - 当所部署的Node节点有问题时，会对容器进行重新部署和重新调度
  - 当容器未通过监控检查时，会关闭此容器
  - 直到容器正常运行时，才会对外提供服务
- 水平扩展
  - 通过简单的命令、用户UI界面或基于CPU等资源使用情况，对应用容器进行规模扩大或规模剪裁
- 服务发现
  - 用户不需要使用额外的服务发现机制，就能够基于Kubernetes自身能力实现服务发现和负载均衡
- 滚动更新
  - 可以根据应用的变化，对应用容器进行的应用，进行一次性或批量式更新
- 版本回退
  - 可以根据应用部署情况，对应用容器进行的应用，进行历史版本即时回退
- 密钥和配置管理
  - 在不需要重新构建镜像的情况下，可以部署和更新密钥和应用配置，类似热部署
- 存储编排
  - 自动实现存储系统挂载和应用，特别对有状态应用实现数据持久化非常重要
  - 存储系统可以来自于本地目录、网络存储（NFS，Cluster，Ceph，Cinder等），公共云存储服务等。

## Node和Pod支持

- Node节点数量支持
  - 早期版本管理 100 台
  - 现版本可以管理 2000 台
- pod管理支持
  - 早期版本管理 1000 个
  - 现版本管理 150000 个

## 应用部署架构分类

- 无中心节点架构
  - ClusterFS 
- 有中心节点架构
  - HDFS
  - K8S

K8s集群架构节点角色功能

- Master Node
  - K8s集群控制节点，对集群进行调度管理，接受集群外用户到集群操作请求
  - Master Node由API Server，Scheduler，Cluster State Store（ETCD数据库）和Controller Manager Server所组成；
- Worker Node
  - 集群工作节点，运行用户业务应用容器；
  - Worker Node包括kubelet、kube proxy和Container Runtime；

## K8s集群部署工具

### 1、二进制源码包部署

### 2、使用kubeadm部署

- 安装软件kubelet kubeadm kubectl 
- 初始化集群
- 添加node到集群中
- 证书自动生成
- 集群管理系统是以容器方式存在，容器运行在master
- 容器镜像是谷歌提供
  - 阿里云下载容器镜像，需要重新打标记
  - 谷歌下载

## 部署

用Vmware搭建三台主机：

![image-20241130235507961](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241130235507961.png)

![image-20241130235539532](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241130235539532.png)



### 同步时间

```shell
ntpdate time1.aliyun.com
```

编写cron表达式每个一小时同步时间：

```shell
crontab -e
```

```cron
0 */1 * * * ntpdate time1.aliyun.com
```



### 永久关闭Swap分区

```shell
vi /etc/fstab
```

注释Swap分区：

![image-20241201150907777](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241201150907777.png)

### 添加网桥过滤

```shell
vi /etc/sysctl.d/k8s.conf
```

![image-20241201151527210](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241201151527210.png)

```conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
vm.swappiness = 0
```

加载br_netfilter模块

```shell
modprobe br_netfilter
```

查看是否加载

```shell
lsmod | grep br_net
```

加载网桥过滤配置文件

```shell
sysctl -p /etc/sysctl.d/k8s.conf
```

### 开启ipvs

```shell
yum -y install ipset ipvsadm
```

添加需要加载的模块

```shell
cat > /etc/sysconfig/modules/ipvs.modules <<EOF
#!/bin/bash
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack_ipv4
EOF
```

授权、运行、检查是否加载

```shell
chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules && lsmod | grep -e ip_vs -e nf_conntrack_ipv4
```

### 在manager节点及worker节点安装指定版本的docker-ce

yum源获取

```shell
wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
```

![image-20241201153450241](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241201153450241.png)

对版本进行排序

```shell
yum list docker-ce.x86_64 --showduplicates | sort -r
```

安装指定版本的docker-ce

```shell
yum -y install --setopt=obsoletes=0 docker-ce-18.06.3.ce-3.el7
```



在 `/etc/docker/daemon.json` 添加如下内容：

```shell
cat /etc/docker/daemon.json
```

```
"exec-opts":["native.cgroupdriver=systemd"]
```

之后重启

```shell
[root@master1 ~]# systemctl restart docker
[root@master1 ~]# systemctl status docker
```

![image-20241201173841792](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20241201173841792.png)

## 集群软件安装及配置

```shell
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
```

### 安装软件包

```shell
yum -y install kubeadm kubelet kubectl
```

### 软件配置

主要配置kubelet，如果不配置可能会导致k8s集群无法启动

### k8s集群容器镜像准备

编写shell脚本：

```shell
kubeadm config images list >> image.list
```

```shell
#!/bin/bash
image_list='registry.k8s.io/kube-apiserver:v1.28.15
registry.k8s.io/kube-controller-manager:v1.28.15
registry.k8s.io/kube-scheduler:v1.28.15
registry.k8s.io/kube-proxy:v1.28.15
registry.k8s.io/pause:3.9
registry.k8s.io/etcd:3.5.9-0
registry.k8s.io/coredns/coredns:v1.10.1'


for image in ${image_list}
do
        docker pull $image
done
```

运行脚本：

```shell
sh image.list
```

### k8s集群初始化

```shell
kubeadm init --kubernetes-version=v1.17.2 --pod-network-cidr=172.16.0.0/16 --apiserver-advertise-address=192.168.216.100
```

