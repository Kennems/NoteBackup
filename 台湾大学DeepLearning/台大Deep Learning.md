# AI 学习笔记 

## 机器学习简介

Different types of Functions

**Regression** : The function outputs a scalar(标量）.

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

1 **epoch** = see all the batched once   

**update** : update θ for each batch

### Sigmoid -> ReLU (Rectified Linear Unit)

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230505113735.png)

统称为 Activation function

Neural Network

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230505114415.png)

# PyTorch

Python3中机器学习框架

```py
dataset = MyDataset(file)
dataloader = DataLoader(dataset, batch_size = size , shuffle = True)
Training : True
Testing : False
```

```py
from torch.utils.data import Dataset, DateLoader
class MyDataset(Dataset):
	def __init__(self, file): # read data & preprocess
		self.data = ...
	def __getitem__(self,index): #return one sample at a time
		return self.data[index]
	def __len__(self): #return the size of the dataset
		return len(self.data)
```

### Tersors

High-dimensional matrices(arrays)

```py
.shape() # show the dimension
#Directly from data (list or numpy.ndarray)
x = torch.tensor([1, -1], [-1, 1])
x = torch.from_numpy(np.array([[1, -1], [-1, 1]]))
#Tensor of constant zeros & ones 
x = torch.zeros([2, 2])
x = torch.ones([1, 2, 5])
x+y x-y y=x.pow(2) y=x.sum() y=x.mean()
#Transpose : transpose two specified dimensions
x = x.transpose(dim0,dim1) # change the dimension dim0 and dim1
#Squeeze : remove the specified dimension with length 1 
x = x.squeeze(1)
#unsqueeze expand a new dimension
x = x.unsqueeze(1)

```



# PyTorch

```py
dataset = MyDataset(file)
dataloader = Dataloader(dataset, batch_size, shuffle = True)
shuffle : Training -> true
		  Testing -> false
```

### Tersors

- High-dimensional matrices(matrix used in mathematics, arrays)

**dim** in PyTorch == **axis** in NumPy

dimensional

```py
Check with.shape()	
```

#### Creating Tensors

- Directly from data (list or numpy.ndarray)

  ```py
  x = torch.tensor([1, -1], [-1, 1])
  x = torch.from_numpy(np.array([[1, -1], [-1, 1]]))
  ```

- Tensor of constant zeros & ones

```py
x = torch.zeros([2,2])
x = torch.ones([1, 2, 5])
```

- Common Operations

**addition subtraction power summation mean**

- transpose

```py
x.shape
x.transpose(0,1)
```

**Unsqueeze** : expand a new dimension

```py
x = x.unsqueeze(1)
```

**Cat **: conncatenate multiple tensors 合并多个矩阵

```py
torch.cat([x, y, z], dim = 1)
```

**Data Type**: Using different data types for model and data will case errors.

32-bit -torch.float

64-bit -torch.long

**Device**

- Tensors & modules will be computed with CPU by default
- **Use .to()** to move tensors to appropriate devices
  - CPU
    - `x = x.to('cpu')	 - ```py  x = x.to('cuda') `
- GPU
  - check if your computer has NVIDIA GPU
    - `torch.cuda.is_available() - Multiple GPUs  :  specify- ```   'cuda:0', 'cuda:1', 'cuda:2',... `

#### Cradient Calculation

```py
import torch
# 定义一个需要求导的张量 x，并将 requires_grad 参数设置为 True 
x = torch.tensor([[1., 0.], [-1., 1.]], requires_grad=True)
# 计算 x 的平方并对其进行求和，得到张量 z
z = x.pow(2).sum()
# 对张量 z 进行反向传播，自动计算出 x 的梯度
z.backward()
# 输出 x 的梯度
print(x.grad)
```

### torch.nn

#### Network Layers

- Linear Layer (Fully-connected Layer)
  - `nn.linear(in_features, out_features) #### Non-linear Activation Functions```pynn.Sigmoid()nn.ReLU() `

#### Build your own neural network

```py
import torch.nn as nn
class MyModel(nn.Module):
    #initialize your model & define layers
	def __init__(self):
		super(MyModel, self).__init__()
		self.net = nn.Sequential(
			nn.Linear(10, 32),
			nn.Sigmoid(),
			nn.Linear(32,1)
		)
	#compute output of your nn
	def forward(self, x):
		return self.next()
```

#### Loss Functions

- **Mean squared Error** (for regression tasks)

```
criterion = nn.MSELoss()
```

- Cross Entropy (for classification tasks) 交叉熵

```
criterion = nn.CrossEntropyLoss()
```

- `loss = criterion(model_output, expected_value) ### torch.optim- Stochastic Gradient Descent (SGD)  - ```py    torch.optim.SGD(model.parameters(), lr, momentum = 0) `
- For every batch of data
  - Call optimizer.zero_grad() to **reset** gradients of model parameters.
  - Call loss.backward() to **backpropagate** gradients of prediction loss
  - Call optimizer.step() to **adjust** model parameters

### Neural Network Training Setup

```py
dataset = MyDataSet(file)
tr_set = DataLoader(dataset, 16, shuffle = True)
model = MyModel().to(device)
criterion = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), 0.1)
```

#### Training Loop

```py
for epoch in range(n_epochs):  # Iterate over n_epochs
    model.train()  # Set the model to training mode
    for x, y in tr_set:  # Iterate over the training set
        optimizer.zero_grad()  # Clear the gradients
        x, y = x.to(device), y.to(device)  # Move data to the device (e.g., GPU)
        pred = model(x)  # Forward pass, compute predictions
        loss = criterion(pred, y)  # Compute the loss
        loss.backward()  # Backward pass, compute gradients
        optimizer.step()  # Update the model's parameters using the gradients
```

#### Validation Loop

```py
model.eval()  # Set the model to evaluation mode
total_loss = 0

for x, y in dv_set:  # Iterate over the validation set
    x, y = x.to(device), y.to(device)  # Move data to the device
    with torch.no_grad():  # Disable gradient computation
        pred = model(x)  # Forward pass, compute predictions
        loss = criterion(pred, y)  # Compute the loss
    total_loss += loss.cpu().item() * len(x)  # Accumulate the loss
avg_loss = total_loss / len(dv_set)  # Calculate the average loss per sample
```

#### Testing Loop

```py
model.eval()  # Set the model to evaluation mode
preds = []

for x in tt_set:  # Iterate over the test set
    x = x.to(device)  # Move data to the device
    with torch.no_grad():  # Disable gradient computation
        pred = model(x)  # Forward pass, compute predictions
        preds.append(pred.cpu())  # Append the predictions to the list
```

### Data, demo1

#### Load data :

use pandas to load a csv file

```py
train_data = pd.read_cav('./name.csv').drop(columns=['date']).values
x_train, y_train = train_data[:,:-1], train_data[:,:-1]
```

#### Dataset

**init** : Read data and preproces

**getitem** : Return one sample at a time, In this case, one sample includes a 117 dimensional feature and a label

**len** : Return the size of the dataset. In this case, it is 2699

```py
class COVID19Dataset(Dataset):
    '''
    x: np.ndarray  特征矩阵.
    y: np.ndarray  目标标签, 如果为None,则是预测的数据集
    '''
    def __init__(self, x, y=None):
        if y is None:
            self.y = y
        else:
            self.y = torch.FloatTensor(y)
        self.x = torch.FloatTensor(x)

    def __getitem__(self, idx):
        if self.y is None:
            return self.x[idx]
        return self.x[idx], self.y[idx]

    def __len__(self):
        return len(self.x)
```

#### Dataloader

```py
train_loader = DataLoader(train_dataset, batch_size = 32, shuffle = True, pin_memory = True)
```

#### Model

```py
class My_Model(nn.Module):
    def __init__(self, input_dim):
        super(My_Model, self).__init__()
        # TODO: 修改模型结构, 注意矩阵的维度（dimensions） 
        self.layers = nn.Sequential(
            nn.Linear(input_dim, 16),
            nn.ReLU(),
            nn.Linear(16, 8),
            nn.ReLU(),
            nn.Linear(8, 1)
        )

    def forward(self, x):
        x = self.layers(x)
        x = x.squeeze(1) # (B, 1) -> (B)
        return x
```

#### Criterion

```py
criterion = torch.nn.MSELoss(reduction = 'mean')
```

#### Optimizer

```py
optimizer = torch.optim.SGD(model.parameters(), lr = 1e-5, momentum = 0.9)
```

#### Training Loop

### Documentation and Common Errors

read pytorch tutorial

### Colab（highly recommended）



# Officially begin

Deep = Many hidden layers

### Neurall Network

Find a function in function set.

### Goodness of function

Pick the best function

### Backpropagation - Backward Pass(反向传播)

反向的neural network

# Regression

- Stock Market Forecast
- Self-driving Car
- Recommendation

## Step 1 : Model

A set of function

## Step 2 : Goodness of Function

$$
\hat{y}^1代表x^1对应的确切值
$$

Loss function L：
$$
L(f)=L(w,b) ~ Estimated ~ y ~ based~~oninput~~function
$$

$$
L(w,b)=\sum_{n=1}^{10}(\hat{y}^n-(b+w\cdot x_{cp}^n))^2
$$

## Step 3 ：Best Function

In linear regression, the loss function L is convex.

Overfitting

## Regularization

$$
L(w,b)=\sum_{n=1}^{10}(\hat{y}^n-(b+w\cdot x_{cp}^n))^2+\lambda\cdot \sum(w_i)^2
$$

不需要考虑bias，调整平滑程度，smooth

- Gradient descent
- Overfitting and Regularization

# Classification

independently and identically distributed(i.i.d)
$$
L(h^{train},D_{all})-L(h^{all}, d_{all}) \leq \delta\\
we\ need \ \forall h \in \H, |L(h,D_{train}) -L(h,D_{all}) | \leq \delta/2\\
L(h^{train},D_{all})\leq L(h^{train},D_{all}) + \delta/2
$$
重温数码宝贝：

**模型出现bad的概率：**
$$
P(D_{train}\ is\ bad)\leq |H| \cdot 2exp(-2N\epsilon^2 ) \\
N \ge \frac{log(2|H|/\delta)}{2\epsilon^2}
$$
**Tradeoff of Model Complexity** 

  ![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230707153005.png)

## Training data for Classification 

pair

## Ideal Alternatives

#### Function(Model):

$$
f(x)\\
x -> g(x)>0~Output=class1\\
else\ Output=class2
$$

#### lossfunction:

The number of times of get incotrrect results on training data.
$$
L(f) = \sum_{n}\delta(f(x^n)\neq\hat{y}^n)
$$

#### Find the best function;

- Example : Perceptron, SVM

### Prior

$$
P(C_1|x)=\frac{P(x|C_1)P(C_1)}{P(x|C_1)P(C_1)+P(x|C_2)P(C_2)}
$$

### Gaussian 

Maximum Likelihood 

2D array or 3D array mean the array with 2 or 3 axes respectively, but the n-dimensional vector mean the vector of length n.

Learn something that can really differ you from others.

# Logistics Regression

## Function Set


$$
f_{w,b}=\sigma(\sum_{i}w_ix_i)+b
$$
**Output** : Between 0 and 1 
$$
f_{w,b}(x)=P_{w,b}(C_1|x)
$$

$$
w^*,b^*=arg\ \underset{w,b}{max}L(w,b)\\
等同于 w^*,b^* = arg\ \underset{w,b}{min}-lnL(w,b)
$$

**Cross Entropy**:
$$
Distribution \ p:
p(x=1)=\hat{y}^n\\
p(x=0)=1-\hat{y}^n\\
Distribution \ q:
q(x=1)=f(x^n)\\
q(x=0)=1-f(x^n)\\
H(p,q)=-\sum_xp(x)ln(q(x))
$$

## Loss Function

$$
L(f)=\sum_nC(f(x^n),\hat{y}^n)\\
C(f(x^n),\hat{y}^n)=-[\hat{y}^nlnf(x^n)+(1-\hat{y}^n)ln(1-f(x^n))]
$$

## Update

logistic regression 和 linear regression 形式完全相同
$$
w_i\gets w_i-\eta \sum_{n}-(\hat{y}^n-f_{w,b}(x^n))x_i^n
$$

## Discriminative (logistic) & Generative (Gaussian描述)

Generative做了某些假设。

- Benefit of generative model
  - With the assumption of probability distribution, less training data is needed
  - With the assumption of probability distribution, more robust to the noise
  - **Priors** and **class-dependent** probabilities can be estimated from different sources.

## Multi-class Classification

**SoftMax**
$$
Softmax(z_i)=\frac{e^{z_i}}{\sum_{c=1}^{C} e^{z_c}}\\
1 > y_i' > 0\\
\sum_iy_i'=1
$$

## Limitation of Logistic Regression

只能画一条直线

- Feature Transformation
  - Cascading logistic regression models 

## Optimization Issue 

层数较多表现的反而没有层数较少的好

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230705203112.png)

## Over fitting

- 增加训练资料
- **Data augmentation** 

- constrained model
  - Less parameters, sharing parameters
  - Less features
  - Early stopping

CNN->比较没有弹性的model

分Training Set

- N-fold Cross Validation 

# Optimization Fail

H : Hessian 

**Tayler Series Approximation** 
$$
L(\theta) \approx L(\theta^\prime)+\frac{1}{2}(\theta-\theta^\prime)^TH(\theta-\theta^\prime)
$$

- **H** is **positive definte** = All eigen values are positive -> **local minima**
- **H** is **negative definte** = All eigen values are negative -> **local **
- Some eigen values are positive , and some are negative -> **Saddle point**

在高维下**local minima**可能会变成**saddle poing**

# Tips for training : Batch and Momentum

## Batch

1 epoch = see all the batches once -> **shuffle** after each epoch  

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230705230316.png)

## Momentum

Movement not just based on gradient, but previous movement.

## Different parameters needs different learning rate 

$$
\theta_i^{t+1} \gets \theta_i^t-\frac{\eta}{\sigma_i^t}g_i^t\\
\sigma_i^t=\sqrt{\frac{1}{t+1}\sum_{i=0}^t(g_i^t)^2}
$$

Adagred

## RMSProp

$$
\theta_i^{t+1} \gets \theta_i^t-\frac{\eta}{\sigma_i^t}g_i^t\\
\sigma_i^t = \sqrt{\alpha(\sigma_i^{t-1})^2+(1-\alpha)(g_i^t)^2}
$$

Adam : RMSProp + Momentum

## Learning Rate Sceduling 

$$
\theta_i^{t+1} \gets \theta_i^t-\frac{\eta^t}{\sigma_i^t}g_i^t\\
$$

## Warm Up

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230706094859.png)

# New Optimization





# Convolutional network (CNN)

Network的架构调整

1、All the images to be classified have the same size.

 Receptive filed

## Simplification 1 - Typical Setting 

all channels : 会看所有的channels 

kernel size : 长和宽 （e.g., 3*3)

Stride : 移动的步长,希望有高度的重叠

padding : 补值，补充超出范围的值

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230707112303.png)

Each receptive field has a set of neurons.

Each receptive field has the neurons with the same set of parameters, which called filter.

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230707112417.png)

##  Pooling 

## Flatten

  

# Spatial Transformer（STN）

处理旋转和放大图形的CNN分类

## interpolation 插值法

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230707163244.png)

# Self-attention

## Sequence Labeling 

consider the context -> 参数很大并且容易Over fitting

Self-attention会持有整个sequence的信息

input : vector 

output : vector
$$
q_i=W^qa^i\\
k_i=W^ka^i\\
v^i=W^va^i
$$
![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20230707171500.png)

## Multi-head Self-attention

其中
$$
q_i, k_i, v_i均可以有多个
$$

## Self-attention for Speech

Truncated(**截短的**) Self-attention

Self-attention is the complex version of CNN

CNN is simplified self-attention

- Recurrent Neural Network(RNN)

RNN所做的事情都可以用Self-attention来替代

Self-attention更有效率

RNN



# Transformer

## Sequence to sequence (Seq2seq)

Encoder -> Decoder 

## Encoder

input some vectors and output some vectors 

## Decoder

### Autoregressive

### Non-autoregressive Decoder

同时输出BEGIN并且同时输出结果和END

Advantage ： parallel, controllable output length 

NAT is usually worse than AT

### Encoder-Decoder

Cross Attention

 Teacher Forcing : using the ground truth as input 

Copy Mechanism 

- Pointer Network
- Copying Mechanism 

## Guided Attention 

## Beam Search 

Scheduled Sampling 
