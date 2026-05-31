---
title : '导致报错原因：'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-05-04T13:01:31+08:00
description : "app.figure.WindowStyle = 'modal';"
image : img/cat.jpg
draft : false
categories : ["Matlab"]
tags : ["学习笔记", "Matlab"]
---
# 导致报错原因：

```matlab
                 app.figure.WindowStyle = 'modal';
```

代码结构：

组件属性： properties

四个方法：

​	图像施加效果更新变换图像

​	句柄控件控制的回调函数

​	UI界面初始化

​	UI界面生成与删除

- [x] 无法计算此图像时显示无此变换
- [ ] 各种变换参数设置
  - [x] 小波变换
  - [ ] DCT
  - [ ] 傅里叶

```matlab
                % 更新 handles 结构
                guidata(hObject, handles);
                % 显示等待条
                h = waitbar(0, '更新图像中 等待...');
                steps = 200;
                for step = 1:steps
                    waitbar(step / steps)
                end
                close(h)
                % 更新函数
                update(app,handles);
```

```
   % 将handles.img导出到Workspace中，变量名为exportedImg
    assignin('base', 'exportedImg', handles.img);
```

二值图片反色

```
	handles.img = imcomplement(handles.img);
```

```matlab
classdef Image_Processing_GUI < matlab.apps.AppBase
	% 与应用程序组件对应的属性
    properties (Access = public)
     %根据对图片施加的效果更新变换图像
    methods (Access = private)
     % 句柄控件控制的回调函数
    methods (Access = private)
     % UI界面布局初始化
    methods (Access = private)
    % UI界面生成与删除
    methods (Access = public)
end
```

