---
title : 'WSL 配置 CUDA'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "在 WSL (Windows Subsystem for Linux) 中配置 CUDA 环境，可以在 Windows 上直接运行 GPU 加速的深度学习、科学..."
image : img/cat.jpg
draft : false
categories : ["小工具使用"]
tags : ["学习笔记", "小工具使用"]
---
# WSL 配置 CUDA

在 WSL (Windows Subsystem for Linux) 中配置 CUDA 环境，可以在 Windows 上直接运行 GPU 加速的深度学习、科学计算等 Linux 应用，无需双系统或虚拟机。

## 目录

- [一、前提条件](#一前提条件)
- [二、安装 WSL2 与 Ubuntu](#二安装-wsl2-与-ubuntu)
- [三、安装 NVIDIA GPU 驱动](#三安装-nvidia-gpu-驱动)
- [四、安装 CUDA Toolkit](#四安装-cuda-toolkit)
- [五、环境变量配置](#五环境变量配置)
- [六、验证安装](#六验证安装)
- [七、安装 cuDNN](#七安装-cudnn)
- [八、常见问题与排查](#八常见问题与排查)
- [九、性能注意事项](#九性能注意事项)
- [十、参考链接](#十参考链接)

---

## 一、前提条件

### 1.1 硬件要求

- 一块支持 CUDA 的 NVIDIA GPU（查看支持列表：[CUDA GPUs](https://developer.nvidia.com/cuda-gpus)）
- 推荐显存 >= 4GB（深度学习训练建议 >= 8GB）

### 1.2 Windows 版本要求

- Windows 10 版本 21H2 或更高（内部版本 19044+）
- **或** Windows 11 版本 21H2 或更高
- 必须启用 WSL2 功能

### 1.3 软件要求

- Windows 系统已更新至最新
- BIOS/UEFI 中已启用虚拟化（VT-x / AMD-V）
- 拥有管理员权限

---

## 二、安装 WSL2 与 Ubuntu

### 2.1 启用 WSL 功能

以管理员身份打开 **PowerShell** 或 **命令提示符**，执行：

```powershell
# 启用 WSL 功能
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 启用虚拟机平台
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

重启计算机。

### 2.2 设置 WSL2 为默认版本

重启后，打开 PowerShell：

```powershell
# 设置 WSL 默认版本为 WSL2
wsl --set-default-version 2
```

### 2.3 安装 Ubuntu 发行版

```powershell
# 查看可用的 Linux 发行版
wsl --list --online

# 安装 Ubuntu 22.04 LTS（推荐）
wsl --install -d Ubuntu-22.04
```

安装完成后，启动 Ubuntu 并设置用户名和密码。

### 2.4 验证 WSL 版本

```bash
# 在 Windows PowerShell 中查看 WSL 状态
wsl --status

# 查看已安装的发行版及其版本
wsl --list --verbose
```

确保 Ubuntu 的 VERSION 列为 **2**。

### 2.5 更新 Ubuntu 软件包

进入 WSL Ubuntu 终端：

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 三、安装 NVIDIA GPU 驱动

### 3.1 在 Windows 端安装驱动

**WSL 本身不需要在 Linux 内安装 GPU 驱动**，GPU 驱动直接安装在 Windows 上即可。WSL 会自动使用 Windows 的 NVIDIA 驱动。

1. 前往 [NVIDIA 驱动下载](https://www.nvidia.com/Download/index.aspx)
2. 选择对应的 GPU 型号和 Windows 操作系统
3. 下载并安装 **Game Ready 驱动** 或 **Studio 驱动**

要求驱动版本 >= 525.60（CUDA 12.x 支持需要较新驱动）。

### 3.2 验证驱动在 WSL 中可用

在 WSL 终端中执行：

```bash
nvidia-smi
```

正常输出应显示 GPU 信息、驱动版本和 CUDA 版本。如果提示 `command not found`，说明驱动未安装或 WSL 集成未生效。

---

## 四、安装 CUDA Toolkit

### 4.1 选择 CUDA 版本

查看 `nvidia-smi` 输出的 "CUDA Version" 字段，该值表示当前驱动支持的最高 CUDA 版本。安装的 CUDA Toolkit 不能高于此版本。

推荐的 CUDA 版本对应关系：

| CUDA Toolkit | 最低驱动版本 | 适用场景       |
|--------------|-------------|----------------|
| CUDA 12.x    | >= 525.60   | 最新深度学习框架 |
| CUDA 11.8    | >= 450.80   | 兼容性较好      |
| CUDA 11.3    | >= 450.80   | 旧项目兼容      |

### 4.2 使用 Network Installer 安装（推荐）

以下以 **CUDA 12.4** 为例，在 WSL Ubuntu 中执行：

```bash
# 下载 CUDA 12.4 pin 文件
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin

# 移动到 sources.list.d
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600

# 下载并添加 CUDA 仓库的 deb 包
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda-repo-wsl-ubuntu-12-4-local_12.4.0-1_amd64.deb

# 安装仓库配置
sudo dpkg -i cuda-repo-wsl-ubuntu-12-4-local_12.4.0-1_amd64.deb

# 添加 CUDA GPG key
sudo cp /var/cuda-repo-wsl-ubuntu-12-4-local/cuda-*-keyring.gpg /usr/share/keyrings/

# 更新包索引
sudo apt-get update

# 安装 CUDA Toolkit
sudo apt-get -y install cuda-toolkit-12-4
```

### 4.3 使用 Runfile 安装（替代方案）

如果网络环境较好，也可以直接下载 Runfile 安装：

```bash
# 下载 CUDA 12.4 Runfile
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run

# 运行安装程序
sudo sh cuda_12.4.0_550.54.14_linux.run
```

安装时注意：
- 不要安装驱动（WSL 使用 Windows 驱动）
- 选择安装 CUDA Toolkit 即可
- 默认安装到 `/usr/local/cuda-12.4`

### 4.4 安装最新 CUDA（在线方式）

也可以使用 NVIDIA 官方提供的 `cuda-keyring` 包来保持最新：

```bash
# 添加 CUDA 仓库
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update

# 安装最新版 CUDA Toolkit
sudo apt-get -y install cuda-toolkit
```

---

## 五、环境变量配置

### 5.1 配置 PATH 和 LD_LIBRARY_PATH

编辑 `~/.bashrc`（或 `~/.zshrc`）：

```bash
# 打开配置文件
nano ~/.bashrc
```

在文件末尾添加：

```bash
# CUDA 环境变量配置
export CUDA_HOME=/usr/local/cuda-12.4
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```

如果希望使用符号链接版本（自动指向最新安装的 CUDA 版本）：

```bash
export CUDA_HOME=/usr/local/cuda
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```

### 5.2 使配置生效

```bash
source ~/.bashrc
```

### 5.3 验证环境变量

```bash
echo $CUDA_HOME
which nvcc
nvcc --version
```

---

## 六、验证安装

### 6.1 检查 nvidia-smi

```bash
nvidia-smi
```

输出示例：

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.14    Driver Version: 551.86       CUDA Version: 12.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce RTX 4090  | Off | 00000000:01:00.0  Off |                  OFF |
| 30%   40C    P0    80W / 450W |   1234MiB / 24564MiB |      0%      Default |
|                               |                      |                  N/A |
+-----------------------------+----------------------+----------------------+
```

### 6.2 检查 nvcc

```bash
nvcc --version
```

输出示例：

```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2024 NVIDIA Corporation
Built on Tue_Feb_27_16:19:30_PST_2024
Cuda compilation tools, release 12.4, V12.4.99
Build cuda_12.4.r12.4/compiler.33567101_0
```

### 6.3 编译并运行 CUDA 示例

```bash
# 创建一个简单的 CUDA 程序
cat > ~/test_cuda.cu << 'EOF'
#include <stdio.h>

__global__ void hello_cuda() {
    printf("Hello from GPU block %d, thread %d!\n", blockIdx.x, threadIdx.x);
}

int main() {
    hello_cuda<<<2, 4>>>();
    cudaDeviceSynchronize();
    printf("CUDA is working on WSL!\n");
    return 0;
}
EOF

# 编译并运行
nvcc -o ~/test_cuda ~/test_cuda.cu
~/test_cuda
```

### 6.4 Python CUDA 验证

如果安装了 Python，可以使用 PyTorch 或 TensorFlow 验证：

```bash
# 方式一：使用 PyTorch
python3 -c "import torch; print('CUDA available:', torch.cuda.is_available()); print('Device count:', torch.cuda.device_count()); print('Device name:', torch.cuda.get_device_name(0))"

# 方式二：使用 TensorFlow
python3 -c "import tensorflow as tf; print('GPU devices:', tf.config.list_physical_devices('GPU'))"
```

---

## 七、安装 cuDNN

### 7.1 下载 cuDNN

cuDNN（CUDA Deep Neural Network library）是 NVIDIA 针对深度神经网络的加速库。

**方式一：通过 Ubuntu 包安装**

```bash
# 下载并安装 cuDNN deb 包（需从 NVIDIA 官网下载）
# 访问 https://developer.nvidia.com/cudnn 注册并下载

# 以 cuDNN 9.0 为例
wget https://developer.download.nvidia.com/compute/cudnn/9.0.0/local_installers/cudnn-local-repo-ubuntu2204-9.0.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.0.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-9.0.0/cudnn-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cudnn-cuda-12
```

**方式二：手动解压安装**

```bash
# 下载 cuDNN tar 包（需从 NVIDIA Developer 网站下载，需要注册账号）
# 下载后解压
tar -xzvf cudnn-linux-x86_64-9.0.0.312_cuda12-archive.tar.xz

# 复制到 CUDA 目录
sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include/
sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64/

# 设置权限
sudo chmod a+r /usr/local/cuda/include/cudnn*.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

### 7.2 验证 cuDNN 安装

```bash
# 查看 cuDNN 版本
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2

# 输出示例：
# #define CUDNN_MAJOR 9
# #define CUDNN_MINOR 0
# #define CUDNN_PATCHLEVEL 0
```

---

## 八、常见问题与排查

### 8.1 nvidia-smi: command not found

**原因**：WSL 无法访问 NVIDIA 驱动。

**解决**：
1. 确认 Windows 端已安装 NVIDIA 驱动
2. 确认驱动版本 >= 525.60
3. 重启 WSL：在 PowerShell 中执行 `wsl --shutdown`，然后重新启动 WSL
4. 确认 Windows 更新到最新版本

### 8.2 CUDA version mismatch

**现象**：`nvidia-smi` 显示的 CUDA 版本与 `nvcc --version` 显示的版本不一致。

**原因**：这是正常现象。`nvidia-smi` 显示的是驱动支持的最高 CUDA 版本，`nvcc` 显示的是实际安装的 Toolkit 版本。

**解决**：确保安装的 Toolkit 版本 <= 驱动支持的 CUDA 版本。

### 8.3 WSL 无法检测到 GPU

**解决步骤**：

```powershell
# 在 PowerShell 中检查
nvidia-smi

# 重启 WSL
wsl --shutdown

# 重新启动 WSL
wsl

# 在 WSL 中检查
nvidia-smi
```

如果仍然无效，检查 Windows 设备管理器中是否禁用了 GPU，或 GPU 是否被其他程序占用。

### 8.4 /usr/local/cuda 符号链接未创建

如果手动安装了多个版本的 CUDA，需要手动创建或更新符号链接：

```bash
# 创建符号链接指向最新版本
sudo ln -sf /usr/local/cuda-12.4 /usr/local/cuda
```

### 8.5 编译时找不到 cuda_runtime.h

**原因**：CUDA include 路径未正确配置。

**解决**：
```bash
# 确认文件存在
ls /usr/local/cuda/include/cuda_runtime.h

# 如果不在标准路径，尝试重新安装 CUDA Toolkit
# 或手动设置 CFLAGS
export CFLAGS="-I/usr/local/cuda/include"
```

### 8.6 PyTorch/TensorFlow 无法检测到 GPU

**原因**：
1. PyTorch/TensorFlow 版本与 CUDA 版本不匹配
2. CUDA 环境变量未设置

**解决**：
```bash
# 确认环境变量已设置
echo $LD_LIBRARY_PATH | grep cuda

# 重新安装匹配版本的框架
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124

# 或对于 TensorFlow
pip3 install tensorflow[and-cuda]
```

### 8.7 CUDA Out of Memory

**原因**：Windows 和 WSL 共享 GPU 显存，Windows 图形界面也会占用显存。

**解决**：
1. 关闭不必要的 Windows 图形程序
2. 减小 batch size
3. 使用 `torch.cuda.empty_cache()` 释放缓存
4. 监控显存使用：`watch -n 1 nvidia-smi`

### 8.8 Permission denied / 权限问题

```bash
# 确保当前用户在 video 组中
sudo usermod -aG video $USER

# 重新登录后生效
# 或临时测试
newgrp video
```

---

## 九、性能注意事项

### 9.1 WSL2 与原生 Linux 的性能对比

WSL2 使用真正的 Linux 内核，GPU 直通（GPU Paravirtualization）性能损耗：

| 场景               | 性能损耗                        |
|-------------------|--------------------------------|
| 深度学习训练        | < 5%（通常 1-3%）               |
| 数据拷贝（小文件）  | 较慢（跨文件系统边界）           |
| 数据拷贝（大文件）  | 接近原生                        |
| PCIe 通信          | 无额外损耗                      |

总体而言，**GPU 计算性能几乎无损**，适合深度学习训练和推理。

### 9.2 文件系统性能优化

WSL2 的文件系统性能是最大瓶颈之一。建议：

**最佳实践**：

```bash
# 将项目文件放在 WSL 内部文件系统中
# Linux 内部路径（推荐，性能好）：
/home/username/projects/

# Windows 路径（性能差，避免使用）：
/mnt/c/Users/username/projects/
```

将数据集和代码放在 WSL 的 ext4 文件系统中，而不是 Windows 的 NTFS 分区上。跨文件系统操作（`/mnt/c/`）会引入显著性能开销。

### 9.3 内存管理

WSL2 默认使用动态内存分配，最大可用内存为宿主机内存的 50% 或 8GB（取较大值）。

可以通过 `.wslconfig` 文件自定义资源限制：

在 Windows 用户目录（`C:\Users\<用户名>\`）下创建 `.wslconfig` 文件：

```ini
[wsl2]
memory=32GB        # 限制 WSL2 最大内存
processors=8       # 限制 CPU 核心数
swap=8GB           # 设置交换空间大小
localhostForwarding=true
```

修改后重启 WSL：

```powershell
wsl --shutdown
wsl
```

### 9.4 多 GPU 与 CUDA 版本管理

如果需要多版本 CUDA 共存，推荐使用 **conda** 环境隔离：

```bash
# 安装 Miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh

# 创建不同 CUDA 版本的环境
conda create -n cuda11 python=3.10
conda activate cuda11
conda install cudatoolkit=11.8

conda create -n cuda12 python=3.10
conda activate cuda12
conda install cudatoolkit=12.4
```

### 9.5 WSL 自动启动服务

如果需要在 WSL 启动时自动加载 CUDA 环境或启动 Jupyter：

编辑 `~/.bashrc` 添加：

```bash
# 自动启动 CUDA 环境
export CUDA_HOME=/usr/local/cuda
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH

# 如需要自动启动 Jupyter Lab（后台运行）
if [ -z "$(pgrep -f jupyter-lab)" ]; then
    nohup jupyter-lab --no-browser --port=8888 > /dev/null 2>&1 &
fi
```

### 9.6 WSL 网络配置

WSL2 使用 NAT 网络模式，如果需要从局域网访问：

```powershell
# PowerShell 端口转发（以 Jupyter 8888 端口为例）
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8888 connectaddress=127.0.0.1 connectport=8888

# 查看端口转发规则
netsh interface portproxy show all

# 删除端口转发
netsh interface portproxy delete v4tov4 listenport=8888
```

同时需要在 Windows 防火墙中允许对应端口。

---

## 十、参考链接

- [NVIDIA CUDA 下载（WSL-Ubuntu）](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local)
- [NVIDIA WSL 文档](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)
- [Microsoft WSL 安装文档](https://learn.microsoft.com/zh-cn/windows/wsl/install)
- [CUDA GPUs 支持列表](https://developer.nvidia.com/cuda-gpus)
- [NVIDIA Developer cuDNN 下载](https://developer.nvidia.com/cudnn)
- [WSL 配置指南 `.wslconfig`](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)
- [PyTorch 安装（选择对应 CUDA 版本）](https://pytorch.org/get-started/locally/)

---

> **提示**：配置完成后，建议使用 `nvidia-smi` 和 `nvcc --version` 双重验证。如果遇到问题，优先检查 NVIDIA 驱动版本和 Windows 版本是否满足最低要求。深度学习训练建议始终将数据存放在 WSL 内部文件系统中以获得最佳性能。
