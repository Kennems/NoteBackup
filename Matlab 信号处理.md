# Matlab 信号处理

## Matlab熟悉

首页菜单： 

HOME， PLOTS， APPS， EDITOR， PUBLISH， VIEW

### HOME

#### FILE

New Script 新建脚本

New Live Script 新建实时脚本，类似Jupyter, ipynb(Interactive Python Notebook)

New : 

- Script 
- Live Script
- Function
- Live Function
- Class
- System Object
  - Basic
  - Advance
  - Simulink Extension
- Project
  - Blank Project
  - From Folder
  - From Git
  - From SVN
  - From Simulink Template 
- Figure 
- App 
- Stateflow Chart 
- Simulink Model 
- Find Files 
- Compare

#### VARIABLE

- Import Data
- Save Workspace 
- New variable 
- Open Varibale
- Clear Workspace

Code

### PLOTS



### APPS

### EDITOR

#### 

#### NAVIGAT



#### EDIT 

#### BREAKPOINTS

#### RUN 

### PUBLISH 

### VIEW

## 基本处理

### Plot

```matlab
% 在MATLAB中设置时间范围，从0到2*pi，每隔pi/112进行采样
t = 0 : pi / 112: 2 * pi;

% 计算正弦函数y1的值
y1 = sin(t);

% 计算余弦函数y2的值
y2 = cos(t);

% 计算自定义函数y3的值，其中t.^2表示t的平方，.*表示数组元素相乘
y3 = sin(t.^2)-t.*cos(t);

% 绘制三条曲线，红色实线表示y1，蓝色圆点表示y2，黄色三角形表示y3
plot(t, y1, 'r-', t, y2, 'bo', t, y3, 'y^');

% 自动调整坐标轴范围
axis auto;

% 打开网格线
grid on

% 关闭边框
box off
```

