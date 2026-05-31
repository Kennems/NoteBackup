---
title : 'Linux深度学习笔记'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-05-05T17:05:36+08:00
description : "lspci | grep -i nvidia"
image : img/cat.jpg
draft : false
categories : ["计算机基础"]
tags : ["学习笔记", "计算机基础", "Linux"]
---
# Linux深度学习笔记

## GPU 环境配置

### 安装 NVIDIA 驱动

```bash
# 检查显卡型号
lspci | grep -i nvidia

# Ubuntu 安装驱动
sudo apt update
sudo apt install nvidia-driver-550
sudo reboot
```

### 安装 CUDA

```bash
# 下载 CUDA Toolkit
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
sudo sh cuda_12.4.0_550.54.14_linux.run
```

### 安装 cuDNN

从 [NVIDIA Developer](https://developer.nvidia.com/cudnn) 下载对应版本的 cuDNN 库文件并复制到 CUDA 目录。

### 验证安装

```bash
nvidia-smi                  # 查看 GPU 状态
nvcc --version              # 查看 CUDA 版本
python3 -c "import torch; print(torch.cuda.is_available())"
```

## 深度学习框架

### PyTorch

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

### TensorFlow

```bash
pip install tensorflow[and-cuda]
```

## Docker GPU 支持

```bash
# 安装 nvidia-container-toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
    sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt update && sudo apt install -y nvidia-container-toolkit
sudo systemctl restart docker

# 运行 GPU 容器
docker run --gpus all nvidia/cuda:12.4.0-base nvidia-smi
```

## 常用命令

```bash
# 监控 GPU
watch -n 1 nvidia-smi
nvidia-smi -l 1           # 每秒刷新

# 查看 GPU 进程
fuser -v /dev/nvidia*

# 限制 GPU 显存（TensorFlow）
export TF_FORCE_GPU_ALLOW_GROWTH=true

# 指定 GPU
export CUDA_VISIBLE_DEVICES=0,1
```
