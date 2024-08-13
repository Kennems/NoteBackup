## 滑动DFT

在一些场合需要用到实时频谱分析，但是我们一般所使用的FFT算法，需要每N个采样点才能计算一次，因此它不是对每一个采样值计算瞬时频谱，存在**很大的延时而且FFT算法计算量较大**。要想得到**实时频谱**，则每输入一个数据就要计算一次FFT，但是只是更改了一个数据呀！因此需要寻找一种更快的方法，这个方法能够利用前面计算的FFT结果，**滑动FFT（SDFT）**就能解决上述问题。
        SDFT可以由之前计算的FFT结果推导出新增数据后的FFT结果，SDFT只需N次复数乘法和N次复数加法就可以得到新采样值的瞬时频谱值。SDFT原理如下公式所示，具体推导过程请看文中给的参考文章。

       从公式可以看出计算新采样点n的频谱，只需要用前一个点的频谱值加上新采样值x(n)减去之前的采样值x(n-N)再乘以一个系数。

**matlab**代码：

```matlab
%测试SDFT功能，下面每个节功能不一样，使用根据需要进行相应注释
clear;
clc;
close all hidden;

nn=256;
sf1=256;
t=0:1/sf1:255/sf1;

%%
%节1开始：此节用于验证SDFT是否正确
xx=cos(100*pi*t)+sin(60*pi*t); %获取数据
y=fft(xx, nn);                 %计算FFT
plot(abs(y));                  %绘图
%节结束
%%
%节2开始：SDFT实现,生成动画
%空矩阵的作用是作为SDFT的初值

x1=zeros(1,nn); %建立空矩阵，
y1=zeros(1,nn); %建立空矩阵
k=0:1:nn-1;     %用于计算u矩阵
u=exp(1i*2*pi*k/nn);

for t1=0:1/sf1:255/sf1
    a=cos(100*pi*t1)+sin(60*pi*t1);     %新数据输入
    x1(length(x1)+1)=a;                 %开辟一个空间将新数据加入队列尾
    b=x1(1);                            %从队列头取出一个数据
    x1(1)=[];                           %删除存放上面取出的数据空间
    for i1=1:nn                         %计算SDFT，循环次数等于N=256
       y1(i1)= u(i1)*(y1(i1)-b+x1(nn)); %计算k=0,1,2...N-1的频域值（详细请看文中公式）
    end
   plot(k, abs(y1));                    %绘图（用于录制动画）
   F=getframe;                          %保存帧（用于录制动画）
end
movie(F,1)                              %播放上面录制的动画
% plot(k, abs(y1));                     %如果不录制动画则注释掉上面相关语句再将此语句去除注释直接查看最终波形
%节结束
%%
%节3开始：SDFT实现，生成gif图
%空矩阵的作用是作为SDFT的初值

x1=zeros(1,nn); %建立空矩阵，
y1=zeros(1,nn); %建立空矩阵
k=0:1:nn-1;     %用于计算u矩阵
u=exp(1i*2*pi*k/nn);
pic_num=1;
for t1=0:1/sf1:255/sf1
    a=cos(100*pi*t1)+sin(60*pi*t1);     %新数据输入
    x1(length(x1)+1)=a;                 %开辟一个空间将新数据加入队列尾
    b=x1(1);                            %从队列头取出一个数据
    x1(1)=[];                           %删除存放上面取出的数据空间
    for i1=1:nn                         %计算SDFT，循环次数等于N=256
       y1(i1)= u(i1)*(y1(i1)-b+x1(nn)); %计算k=0,1,2...N-1的频域值（详细请看文中公式）
    end
    plot(k, abs(y1));                   %绘图（用于录制动画）
    

    drawnow;                            %生成gif代码，输出在此m文件目录下面
    F=getframe(gcf);
    I=frame2im(F);
    [I,map]=rgb2ind(I,256);
    if pic_num == 1
        imwrite(I,map,'test.gif','gif','Loopcount',inf,'DelayTime',0.2);
    else
        imwrite(I,map,'test.gif','gif','WriteMode','append','DelayTime',0.2);
    end
    pic_num = pic_num + 1;

end
%节结束
```

