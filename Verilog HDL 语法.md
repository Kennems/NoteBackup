# Verilog HDL 语法

## 点灯程序

```verilog
module led(
    output wire     led0        ,
    output wire     led1        ,
    output wire     led2        ,
    output wire     led3        

);

assign led0 = 0;
assign led1 = 0;
assign led2 = 0;
assign led3 = 0;

endmodule
```

**module**

Verilog语言中的基本描述单位是模块

模块描述某个设计的**功能或结构**，以及它与其他**外部模块进行通信的端口**。

 

## 仿真程序

在`initial`赋值的信号，必须定义成`reg`类型

```
module test(
    // 输入只有wire型
    input            port_a             ,
    input            port_b             ,
    input            port_c             ,
    // 输出既有wire型也有reg型
    output    wire    port_d            ,
    output    wire    port_e            ,
    output    wire    port_f            ,
    output    wire    port_g            ,
    output    wire    port_h
);

assign port_a =         port_a & port_b;
assign port_e =         port_a | port_c;
assign port_f =         ~port_a;
assign port_g =         port_b ^ port_c;
assign port_h =         port_a ^~ port_c;

endmodule 
```





```verilog
begin 
    // begin end 之间的代码是顺序执行的
end
```

## Verilog数字表示格式

```verilog
无符号数：<位宽>' <进制><数字>
有符号数：<位宽>' <sb><数字> //有符号数是按照补码表示的，即第一位是符号位
```

## 逻辑值

```
1：
0：
x:不确定
z:gao'z
```

