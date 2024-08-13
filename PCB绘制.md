#  PCB绘制

修改中文：右键Preference-》 System-》 General-》 use localized resources

复位按钮： System--》 View-》 Reset

左右镜像x，上下镜像y，不要使用

快捷工具栏

属性中设置大小

**建library**

绘线在顶层和底层都可以

**添加库**：在 components 中右上角三条横线选择file-based libraries preferences -> (intlib)为综合库 （schlib）独立库

**网络标号**：电源标识，网络标识

shift + space可以调整wire的方向和形式

上划线上面加反斜杠\

地标识和电源标识

line不具备电气特性

统一修改： find similar

**统一设计元件标号**，tools -> annotation -> annotate schematics

**制作原件**： Designator -》 p? 格式 在元件中则为管脚号

绘制管脚，十字点朝外

统一修改管脚名称：pins中统一按照编号修改

ctrl+home 跳转到原点

**封装库制作**：

PCB图跳转到原点：edit -> jump -> reference (ctrl + end )

在PCB绘图中测量长度时先设置step: property-》 grid manager -> priority

将PCB版大小设置为物理层大小：选择物理层-》 Design-》 Board shape-> Define from selected objects

**布线布局最重要：**电容不能放一起，要靠近电源

快捷键 加号键 生成过孔

将元件旋转45度: 右键pergerence -> rotation step : 修改45度 空格旋转

**先走靠近器件的线**，**后走电源线和地线** 

自动走线：先设计规则，安全间距，线宽，设置keep-out layer

## 原理图设计

自下而上的设计

先原理图：

网络标号只能在同一个原理图中使用，Port可以在不同原理图中使用

Port：

- 输出，输入，双向口

### **层**

top overlay 顶层实心层，不导电

top layer 顶层

Top solder 不覆盖绿油（白油）的区域

bottom layer 底层 

Mechanical 机械层，不带有电气属性

bottom overlay低层实心层

keep-out layer 禁止布线层,定义布线层的边界

画好禁止布线层之后，design->board shape->define from selected objects

## 自动走线

- 设置安全间距 design -> rules -> 
- 设计线宽（VCC,GND,+5V,-5V）最小位10mil,默认为20mil,最大宽度按为200Mil

自动走线必须要绘制keep-out layer

布局是关键

### 泪滴操作

tools -> teardrops -> add 并选择 或者remove

### 覆铜操作

删除死铜（孤岛）remove dead copper

可以选择带网格的铜

定位孔

## Port & Off Sheet Connector

网络标号的延伸，适合层次化设计

放置多个过孔

覆铜安全间距

过孔直接连接 

机械层挖空操作 tools -> convert -> create board from selected primitives 

到处自定义操作，rules-> 右键 export rules

每个pcb文件的规则相互独立

**Shift选择多个元件**

管脚对应不上：

- 管脚不对
- 没有封装
- 封装不对

Designator要和封装对应

批量放置 过孔 ：tools -> via stiching schielding 

tm**消除报警**

**不覆铜**： place -》 polygon pour cutout 

top solder开窗层

选择过滤功能：快捷工具栏第一个

设置不同的安全间距： clearance设置

设置过孔的覆铜方式

挖槽：在机械层挖槽 tools -> Convert ->create board cutout from selected primitives 选择参数 ： board cutout 

导出规则： 点击design rules -》 report 规则

# DSP设计

## PCB规则总结

Design Rules

- Electrical -> Minimum Clearance **8mil**
- Routing -> 
  - Width 
    - Min : 6mil
    - Pre : 8mil
    - Max : 100mil
  - Routing Via Style -> **Rounting Vias**
  - - Via Diameter	
      - min : 25mil
      - max : 100mil
      - Pre : 25mil
    - Via Hole Size
      - min ： 15mil
      - max ： 50mil
      - pre : 15mil
    - 
- Manufacture
  - **Hole to Hole Clearance 0mil**
  - **Minimum Solder Mask Sliver 0mil**
  - SilkscreenOverComponentPads 0mil
  - SilkToSilkClearance 0mil
  - NetAntennae 1000mil
- Hole Size : **15mil**
  - Diameter : 25mil
- 器件标识尺寸 ： 
  - Height ： 40mil
  - Width : 4mil
  - 

​		

**电源模块**  ：AMS1117-5V 73HD301

**SDRAM** : HY57V641620

AD转换 ： TXB0104QPWRQ1

RESET模块



D:\作业\数字信号处理\课程设计\dsp设计\PCB工程文件





快捷键： 

Ctrl + 左键 查看所有连线

alt + 左键 高亮元件

**只选择某元件后 shift + c取消**
$$
H_d\left(e^{j\omega}\right)=
\begin{cases}\ 1,\ &\ \text{for\ }\ \omega\ >\ \omega_c\ \\ 0, &\ \text{for\ }\ \omega\ \leq\ \omega_c\ \end{cases}
$$
