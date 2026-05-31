---
title : 'AI 学习笔记'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-05-04T12:22:45+08:00
description : "Different types of Functions"
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# AI 学习笔记 

## 机器学习简介

Different types of Functions

**Regression** : The function outputs a scalar（标量）.

- predict the PM2.5

**Classification** ： Given options (classes), the function outputs the correct one.

- Spam filtering

**Structured Learning** ： create something with structure(image, document)

Example : YouTube Channel  

#### 1.Function with Unknown Parameters.

$$
y=b+wx_1
$$

#### 2.Define Loss from Training Data

- Loss is a function of parameters 

$$
L(b,w)
$$

- Loss : how good a set of values is.
- L is mean absolute error (MAE)


$$
e=\left | y-\hat{y}  \right |
$$

- L is mean square error (MSE)

$$
e=(y-\hat{y})^2
$$

$$
L=\frac{1}{N} \sum_{n}^{}e_n
$$

#### 3.Optimization

$$
w^*,b^*=arg\,\min_{w,b} \,L
$$

**Gradient Descent** 

- (Randomly) Pick an initial value ：

$$
w^0
$$

- Compute :

$$
\frac {\partial L} {\partial w} |_{w=w_0}
$$

**Negative** : Increase **w**

**Positive** : Decrease **w**
$$
\eta\frac {\partial L} {\partial w} |_{w=w_0}
$$
**η**：learning rate (hyperparameters)

- Update w iteratively
  - Local minima
  - global minima 

类似一个参数，推广到多个参数。

### Linear Models

Linear models have severe limitation. Model Bias.

We need a more flexible model!

**curve** = **constant** + sum of a set of **Hard Sigmoid Function**
$$
y=c\frac {1} {1+exp(-(b+wx_1))} \\
=csigmoid(b+wx_1)
$$

$$
y=b+\sum_{i}sigmoid(b_i+w_ix_i)
$$

$$
y=b+\sum_{i}sigmoid(b_i+\sum_{j}w_{ij}x_j)
$$

**线性代数**角度：
$$
r=b+Wx
$$

$$
a=\sigma(r)
$$

$$
y=b+c^Ta
$$

Loss

- Loss is a function of parameters L(θ)
- Loss means how good a set of values is.

### Optimization of New Model 

$$
\theta= 
\begin{bmatrix}  
  \theta_1 \\
  \theta_2 \\
  \theta_3 \\
  \dots
\end{bmatrix}
$$

$$
\theta=arg \min_\theta L
$$

-  (Randomly) Pick initial values θ^0

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230505112625.png)

1 **epoch** = see all the batches once   

**update** : update θ for each batch

### Sigmoid -> ReLU (Rectified Linear Unit)

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230505113735.png)

统称为 Activation function

Neural Network

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230505114415.png)

## PyTorch

PyTorch 是 Meta（原 Facebook）开源的深度学习框架，具有动态计算图、易用性强、调试方便等特点。

### Tensor（张量）

Tensor 是 PyTorch 中的基本数据结构，类似于 NumPy 的 ndarray，但支持 GPU 加速。

```python
import torch

# 创建 tensor
x = torch.tensor([[1, 2], [3, 4]])
y = torch.zeros(3, 4)          # 全零张量
z = torch.ones(2, 3)           # 全一张量
a = torch.randn(3, 3)          # 标准正态分布随机张量
b = torch.arange(0, 10)        # 类似 range 的张量

# tensor 运算
c = x + y
d = torch.matmul(x, y)         # 矩阵乘法
e = x.mean()                   # 均值
f = x.sum()                    # 求和

# GPU 加速
if torch.cuda.is_available():
    x = x.cuda()
    y = y.cuda()
    print(x + y)
```

### Autograd（自动求导）

PyTorch 的自动求导机制是训练神经网络的核心。

```python
x = torch.tensor(2.0, requires_grad=True)
y = x ** 2 + 3 * x + 1
y.backward()
print(x.grad)  # 输出 dy/dx = 2x + 3 = 7
```

### 构建神经网络

通过继承 `nn.Module` 来定义模型：

```python
import torch.nn as nn
import torch.nn.functional as F

class SimpleNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(784, 256)
        self.fc2 = nn.Linear(256, 128)
        self.fc3 = nn.Linear(128, 10)
        self.dropout = nn.Dropout(0.2)

    def forward(self, x):
        x = x.view(x.size(0), -1)
        x = F.relu(self.fc1(x))
        x = self.dropout(x)
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
```

### 常用网络层

| 层类型 | 说明 |
|--------|------|
| `nn.Linear(in, out)` | 全连接层 |
| `nn.Conv2d(C_in, C_out, K)` | 二维卷积层 |
| `nn.MaxPool2d(kernel_size)` | 最大池化层 |
| `nn.BatchNorm1d/2d` | 批归一化 |
| `nn.LSTM(input_size, hidden)` | LSTM 层 |
| `nn.Embedding(vocab_size, dim)` | 词嵌入层 |

### 损失函数与优化器

```python
criterion = nn.CrossEntropyLoss()        # 交叉熵损失

optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
```

### 训练循环模板

```python
def train(model, dataloader, epochs=10):
    model.train()
    for epoch in range(epochs):
        total_loss = 0
        for X, y in dataloader:
            optimizer.zero_grad()
            output = model(X)
            loss = criterion(output, y)
            loss.backward()
            optimizer.step()
            total_loss += loss.item()
        print(f'Epoch {epoch+1}, Loss: {total_loss/len(dataloader):.4f}')

def evaluate(model, dataloader):
    model.eval()
    correct = 0
    total = 0
    with torch.no_grad():
        for X, y in dataloader:
            output = model(X)
            pred = output.argmax(dim=1)
            correct += (pred == y).sum().item()
            total += y.size(0)
    print(f'Accuracy: {100 * correct / total:.2f}%')
```

### 数据集与 DataLoader

```python
from torch.utils.data import Dataset, DataLoader

class CustomDataset(Dataset):
    def __init__(self, data, labels):
        self.data = data
        self.labels = labels

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        return self.data[idx], self.labels[idx]

dataset = CustomDataset(data, labels)
dataloader = DataLoader(dataset, batch_size=32, shuffle=True)
```

### 模型保存与加载

```python
torch.save(model.state_dict(), 'model.pth')
model.load_state_dict(torch.load('model.pth'))
```
