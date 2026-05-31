---
title : 'MATLAB 学习笔记'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "MATLAB（Matrix Laboratory）是由 MathWorks 公司开发的数值计算和可视化环境。其核心思想是将数据表示为矩阵，并围绕矩阵运算构建高效..."
image : img/cat.jpg
draft : false
categories : ["Matlab"]
tags : ["学习笔记", "Matlab"]
---
# MATLAB 学习笔记

MATLAB（Matrix Laboratory）是由 MathWorks 公司开发的数值计算和可视化环境。其核心思想是将数据表示为矩阵，并围绕矩阵运算构建高效的编程语言和交互工具。

## MATLAB 概述

- **名称来源**：MATrix + LABoratory，矩阵实验室
- **主要用途**：数值计算、算法开发、数据可视化、仿真建模、信号/图像处理、控制系统设计
- **IDE 组成**：
  - 命令窗口（Command Window）—— 交互式执行命令
  - 编辑器（Editor）—— 编写和调试 .m 文件
  - 工作区（Workspace）—— 查看当前变量
  - 当前文件夹（Current Folder）—— 管理文件路径
  - 路径管理（Path）—— addpath、pathtool 管理搜索路径
- **文件类型**：
  - `.m` — 脚本或函数文件
  - `.mat` — 二进制数据文件（save/load）
  - `.fig` — 图形文件
  - `.mlx` — 实时脚本（Live Script）
  - `.p` — 预解析的受保护代码文件

## 变量与数据类型

MATLAB 中所有变量都是数组，标量视为 1×1 矩阵。

### 数值矩阵

```matlab
% 创建矩阵
A = [1 2 3; 4 5 6; 7 8 9];     % 3×3 矩阵
B = zeros(3, 4);                 % 全零矩阵
C = ones(2, 5);                  % 全一矩阵
D = eye(4);                      % 单位矩阵
E = rand(3);                     % 均匀分布随机数 (0~1)
F = randn(4, 2);                 % 标准正态分布随机数
G = linspace(0, 1, 100);         % 0~1 等间距 100 个点
H = 0:0.1:10;                    % 冒号表达式，步长 0.1
```

### 索引与切片

```matlab
A(2, 3)          % 第 2 行第 3 列
A(:, 2)          % 所有行第 2 列 → 列向量
A(1, :)          % 第 1 行所有列 → 行向量
A(2:4, [1 3])    % 第 2~4 行，第 1、3 列
A(end, :)        % 最后一行
A(:)             % 展平为列向量
```

### 元胞数组（Cell Array）

```matlab
C = cell(3, 1);
C{1} = 'hello';
C{2} = [1 2 3; 4 5 6];
C{3} = struct('name', 'test', 'val', 42);
% 访问：C{1} 取值，C(1) 取子元胞
```

### 结构体（Struct）

```matlab
s.name = 'Alice';
s.age = 25;
s.scores = [90 85 92];

% 结构体数组
s(2).name = 'Bob';
s(2).age = 23;
```

### 表（Table）

```matlab
T = table([1; 2; 3], {'A'; 'B'; 'C'}, [10; 20; 30], ...
          'VariableNames', {'ID', 'Name', 'Value'});
% 访问
T.Name           % 取列
T(1, :)          % 取行
T{2, 'Value'}    % 取值
```

### 字符串与字符数组

```matlab
str1 = 'hello';                 % 字符数组（1×5 char）
str2 = "world";                 % 字符串标量（string 类型）
str3 = ["a", "b"; "c", "d"];   % 字符串矩阵
% 拼接
full = strcat(str1, ' ', str2); % 字符数组拼接
full2 = str1 + " " + str2;      % 字符串拼接 (R2016b+)
```

## 脚本编程

### 控制流

```matlab
%% if-elseif-else
if x > 0
    disp('positive');
elseif x == 0
    disp('zero');
else
    disp('negative');
end

%% for 循环
for i = 1:10
    fprintf('i = %d\n', i);
end

for val = [1 5 9 13]  % 遍历任意向量
    disp(val);
end

%% while 循环
n = 1;
while n <= 10
    n = n + 1;
end

%% switch-case
switch method
    case 'linear'
        disp('linear method');
    case {'quadratic', 'cubic'}
        disp('higher order');
    otherwise
        disp('default');
end
```

### 断点调试

- 在编辑器行号旁单击设断点（红色圆点）
- `dbstop if error` — 出错时自动中断
- `dbcont` — 继续执行
- `dbstep` — 单步执行
- `dbquit` — 退出调试模式
- `keyboard` — 在代码中插入交互断点

## 函数编程

### 函数定义

```matlab
function [out1, out2] = myFunction(in1, in2, in3)
% 函数说明（help 命令显示的内容）
    out1 = in1 + in2;
    out2 = in1 * in2;
end
```

- 函数名必须与文件名一致（例如 `myFunction.m`）
- 一个 .m 文件中只能有一个主函数，可以有多个子函数（local function）
- 函数语句不宜过长，尽量分解功能，每个函数只做一件事

### 匿名函数（Function Handle）

```matlab
f = @(x) x.^2 + 2*x + 1;
f(3)  % → 16

g = @(a, b) a * sin(b);
g(2, pi/4)

% 传递函数句柄
integral(@(x) exp(-x.^2), 0, Inf)
```

### 变长输入输出

```matlab
function varargout = varExample(varargin)
    n = nargin;       % 实际输入参数个数
    nout = nargout;   % 实际输出参数个数
    for i = 1:nargin
        fprintf('Input %d: ', i);
        disp(varargin{i});
    end
end
```

### 作用域与持久变量

```matlab
function counter()
    persistent cnt    % 声明持久变量（多次调用间保持值）
    if isempty(cnt)
        cnt = 0;
    end
    cnt = cnt + 1;
    fprintf('Called %d times\n', cnt);
end
```

- 函数内部变量默认局部，对外不可见
- `global` 关键字声明全局变量（不推荐，易造成耦合）

## 面向对象编程

### 类定义

```matlab
classdef MyClass < handle   % handle 类（传引用）vs value 类（传值）
    properties
        Name
        Value
    end

    properties (Access = private)
        InternalData
    end

    properties (Dependent)
        ComputedProp
    end

    methods
        function obj = MyClass(name, val)  % 构造函数
            obj.Name = name;
            obj.Value = val;
        end

        function result = compute(obj, x)
            result = obj.Value * x;
        end

        function set.Name(obj, name)       % set 方法
            if ischar(name) || isstring(name)
                obj.Name = name;
            else
                error('Name must be a string');
            end
        end

        function val = get.ComputedProp(obj)  % get 方法
            val = obj.Value * 2;
        end
    end

    methods (Static)
        function d = defaultObj()
            d = MyClass('default', 0);
        end
    end
end
```

### 继承

```matlab
classdef ChildClass < MyClass & SomeInterface
    properties
        ExtraProp
    end

    methods
        function obj = ChildClass(name, val, extra)
            obj@MyClass(name, val);  % 调用父类构造函数
            obj.ExtraProp = extra;
        end

        function result = compute(obj, x)  % 重写父类方法
            result = obj.Value * x + obj.ExtraProp;
        end
    end
end
```

### 枚举类

```matlab
classdef ColorEnum
    enumeration
        Red, Green, Blue
    end
end
% 使用：c = ColorEnum.Red
```

### 值类与句柄类对比

| 特性 | value 类（默认） | handle 类 |
|------|-----------------|-----------|
| 赋值行为 | 复制副本 | 共享引用 |
| 修改行为 | 不影响原对象 | 影响原对象 |
| 适用场景 | 纯数据容器 | 图形对象、需要观察者模式 |

## 绘图

### 二维绘图

```matlab
x = linspace(0, 2*pi, 100);
y1 = sin(x);
y2 = cos(x);

figure;                          % 创建新图形窗口
plot(x, y1, 'r-', 'LineWidth', 2); hold on;
plot(x, y2, 'b--', 'LineWidth', 1.5);
plot(x, y1 + y2, 'g:', 'LineWidth', 1);

xlabel('x');
ylabel('y');
title('Sine and Cosine');
legend('sin(x)', 'cos(x)', 'sin+cos');
grid on;
axis tight;
```

### 子图

```matlab
figure;
subplot(2, 2, 1); plot(x, sin(x)); title('sin');
subplot(2, 2, 2); plot(x, cos(x)); title('cos');
subplot(2, 2, 3); plot(x, sin(x).^2); title('sin^2');
subplot(2, 2, 4); plot(x, cos(x).^2); title('cos^2');
```

### 三维绘图

```matlab
[X, Y] = meshgrid(-3:0.1:3);
Z = X .* exp(-X.^2 - Y.^2);

figure;

subplot(2, 2, 1);
surf(X, Y, Z); title('surf'); shading interp; colorbar;

subplot(2, 2, 2);
mesh(X, Y, Z); title('mesh');

subplot(2, 2, 3);
contour(X, Y, Z, 20); title('contour');

subplot(2, 2, 4);
plot3(X(:), Y(:), Z(:), '.'); title('point cloud');
```

### 图形定制

```matlab
% 颜色：r/g/b/c/m/y/k/w
% 线型：- / -- / : / -.
% 标记：o / + / * / . / x / s / d / ^ / v

plot(x, y, 'rs--', 'MarkerSize', 8, 'MarkerFaceColor', 'r');
set(gca, 'FontSize', 12, 'XScale', 'log');  % 修改坐标轴属性
xlim([0, 10]); ylim([-1.5, 1.5]);           % 限定范围
text(pi, 0, '\leftarrow \pi', 'FontSize', 14); % 添加标注
```

### 导出图形

```matlab
saveas(gcf, 'myplot.png');
print('-dpng', '-r300', 'myplot_highres.png');  % 高分辨率
exportgraphics(gcf, 'myplot.pdf', 'ContentType', 'vector');  % 矢量格式
```

## 常用数学运算

### 线性代数

```matlab
A = [1 2; 3 4]; B = [5 6; 7 8];

A * B          % 矩阵乘法
A .* B         % 逐元素乘法
A'             % 转置（共轭转置）
A.'            % 非共轭转置
inv(A)         % 逆矩阵
det(A)         % 行列式
rank(A)        % 秩
eig(A)         % 特征值 [V, D] = eig(A)
svd(A)         % SVD 分解 [U, S, V] = svd(A)
A \ b          % 解线性方程组 Ax = b（推荐，数值稳定）
A / b          % xA = b
poly(A)        % 特征多项式系数
norm(A)        % 范数
cond(A)        % 条件数
```

### 多项式

```matlab
p = [1, -5, 6];            % 多项式 x^2 - 5x + 6
roots(p)                   % 求根 → 3, 2
polyval(p, [0 1 2 3])      % 计算多项式值
conv(p1, p2)               % 多项式乘法（卷积）
deconv(p1, p2)             % 多项式除法
polyfit(x, y, n)           % n 次多项式拟合
```

### 微积分

```matlab
syms x;
f = x^3 + 2*x^2 + sin(x);

diff(f)                    % 导数 → 3*x^2 + 4*x + cos(x)
diff(f, 2)                 % 二阶导
int(f)                     % 不定积分
int(f, 0, pi)              % 定积分

% 数值积分
integral(@(x) exp(-x.^2), 0, Inf);
integral2(@(x, y) x.*y, 0, 1, 0, 1);     % 二重积分
integral3(@(x, y, z) x+y+z, 0, 1, 0, 1, 0, 1); % 三重积分

% 微分方程
syms y(t);
eqn = diff(y, t) == -2*y;
cond = y(0) == 1;
sol = dsolve(eqn, cond);  % 解析解

% 数值解 ODE
f = @(t, y) -2*y;
[t, y] = ode45(f, [0 5], 1);
plot(t, y);
```

### 统计与数据

```matlab
data = randn(1000, 1);

mean(data)     % 均值
median(data)   % 中位数
std(data)      % 标准差
var(data)      % 方差
min(data)      % 最小值
max(data)      % 最大值
range(data)    % 极差
corrcoef(X)    % 相关系数矩阵
cov(X)         % 协方差矩阵

histogram(data, 30);          % 直方图
boxplot(data);                % 箱线图
scatter(x, y);                % 散点图
```

### 信号处理基础

```matlab
fs = 1000;                    % 采样率
t = (0:999) / fs;
x = sin(2*pi*50*t) + 0.5*sin(2*pi*120*t);

% FFT
X = fft(x);
f = (0:length(X)-1) * fs / length(X);
plot(f, abs(X));
xlabel('Frequency (Hz)');

% 滤波
[b, a] = butter(4, 0.2, 'low');    % 低通巴特沃斯滤波器
y = filter(b, a, x);

% 卷积
conv(x, h, 'same');

% 自相关
autocorr(x, 20);
```

## 文件读写

```matlab
% CSV/Excel
data = readmatrix('data.csv');
T = readtable('data.xlsx');
writematrix(data, 'output.csv');

% .mat 文件
save('workspace.mat', 'var1', 'var2');
load('workspace.mat');

% 文本文件
fid = fopen('output.txt', 'w');
fprintf(fid, 'x = %.4f, y = %.4f\n', x, y);
fclose(fid);

% 图像
img = imread('photo.jpg');
imshow(img);
imwrite(img, 'output.png');
```

## Simulink 与仿真

- **Simulink**：基于模型的设计与仿真环境
- 模块化建模：连续/离散系统、控制逻辑、状态机（Stateflow）
- 参数设置：求解器选择（ode45/ode23/离散）、仿真时间、步长
- 代码生成：Embedded Coder 生成 C/C++ 代码用于嵌入式部署
- 数据管理：Data Import/Export（From/To Workspace、From/To File）
- 定点化：Fixed-Point Designer 实现浮点到定点转换
- 自定义库：slblocks.m 注册自定义模块库

## 常用工具箱（Toolboxes）

| 工具箱 | 用途 |
|--------|------|
| Signal Processing Toolbox | 信号滤波、频谱分析、窗函数、滤波器设计 |
| Image Processing Toolbox | 图像读取/处理、形态学、分割、特征提取 |
| Control System Toolbox | LTI 模型、根轨迹、Bode/Nyquist 图、PID 整定 |
| Optimization Toolbox | 线性/非线性规划、最小二乘、fmincon |
| Statistics and ML Toolbox | 分类、回归、聚类、PCA、假设检验 |
| Deep Learning Toolbox | 神经网络搭建、训练、GPU 加速、ONNX 导入 |
| Symbolic Math Toolbox | 符号计算、微积分、方程求解 |
| Parallel Computing Toolbox | parfor、GPU 加速、分布式计算 |
| Computer Vision Toolbox | 视觉标定、目标检测、特征匹配、视频处理 |
| Database Toolbox | 数据库连接与 SQL 查询 |
| Financial Toolbox | 金融建模、风险分析、衍生品定价 |

## 编程规范与最佳实践

- **命名**：变量小写 + 下划线，函数首字母小写，类首字母大写
- **注释**：使用 `%%` 分段（Section），`%` 行注释，函数前写 help 文档
- **性能**：
  - 预分配矩阵（`zeros`/`NaN`），避免循环中动态扩展
  - 向量化运算优先于 for 循环
  - 使用 `tic`/`toc` 或 `timeit` 计时优化
- **调试**：善用断点、`dbstop if error`、`warning`、`assert`
- **路径管理**：`addpath(genpath('subfolder'))` 添加子目录
- **版本控制**：`.m` 文件可纳入 Git 管理，与常规代码一致

## 常用快捷键

| 快捷键 | 功能 |
|--------|------|
| F5 | 运行 |
| F9 | 运行选中代码 |
| F12 | 切换断点 |
| Ctrl+I | 自动缩进 |
| Ctrl+R | 注释 |
| Ctrl+T | 取消注释 |
| Ctrl+D | 打开变量/函数定义 |
| Tab | 自动补全 |
