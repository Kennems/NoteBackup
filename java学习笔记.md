# java学习笔记

## 一、基础知识

```java
  注释的作用
  解释说明程序，提高程序的阅读性
  帮助我们调试程序
  Java语言最基本的单位的类，所以我们首先要定义一个类
  Java程序要想能够独立运行，必须要有主方法
  如果想要Java程序有输出，必须要有输出语句
  定义类的格式：
       class classname{
               //the body of class
       }
  主方法的格式
       public static void main(String[] args)
               //the body of the main
       }
  输出语句的格式：
       System.out.println("")
  关键字： 被Java程序赋予特定含义的单词
  关键字的特点： 关键字都是小写的
  关键字注意事项：
       a:goto和const作为保留字，目前并不适用
       b:类似Editplus这样的高级记事本，针对关键字又特殊标记
  标识符：就是给类，接口，方法，变量等其名字是使用的字符序列（字符串）
  组成规则：
   英文字母大小写
   数字
   _和$
  注意事项：
   不能以数字开头
   不能是Java中的关键字
   区分大小写
   Student和student是两个名字
  常见的命名规则：见名知意
    包 其实就是文件夹
    全部小写
    单级： com
    多级； cn.itcast
   类或接口
       一个单词： 首字母大写
        Student，Person，Teacher
    方法或者变量
    	一个单词：全部小写
    		name,age,show()
    	多个单词：从第二个单词开始，每个单词首字母大写
    		myName,showAllStudentNames()
    常量
    	一个单词：全部大写
    		AGE
    	多个单词：每个单词都大写，用_连接
    		STUDENT_MAX_AGE
class NameDemo{
	public static void main(String[] args){
		System.out.println("Hello World!");
    }
}
```

### 1.1变量

变量：在执行的过程中其值不可以发生改变

- ​	举例：π

分类：

- ​	A：字面值常量：1，2，3
- ​	B：自定义常量

字面值常量的分类：

- 字符串常量 用“”括起来的内容

- 整数常量，所有整数数据

- 小数数据，所有的带小数的数据

- 字符常量 用单引号括起来的内容

- 布尔常量 只有两个之 true和false 

- 空常量 null

- ```java
  public class l1_2 {
      public static void main(String[] args){
          System.out.println("HelloWorld!");
          System.out.println(100);
          System.out.println(13.388);
          System.out.println('A');
          System.out.println('1');
          System.out.println(true);
          System.out.println(false);
      }
  }
  ```

### 1.2进制

- 二进制：由0，1组成，以0b开头

- 八进制：由0，1，.... 7组成，以0开头

- 十进制：由0，1，2，3，4，...  9 组成，整数默认为十进制

- 十六进制： 由0，1，...  9 a,b,c,d,e,f(大小写均可)组陈，以0x开头

- 

- ```java
  public class l1_3 {
      public static void main(String[] args){
          System.out.println(0b100);//4
          System.out.println(0100);//64
          System.out.println(100);//100
          System.out.println(0x100);//256
      }
  }
  ```

### 1.3变量

### 1.4数据类型

```java
数据类型 
	基本类型： 四类八种
	引用类型： 类，接口，数组。。。
	基本类型： 
		整数：
			byte  1字节
			short 2byte
			int   4byte 
			long  8byte 
		浮点类：
			float  4byte
			double 8byte 
		字符类：
			char   2byte 
		布尔：
			boolean 1byte
		java中采用的是Unicode编码，二Unicode编码中每个字符是两个字节，所以Javaz中的字符可以存储一个汉字
		整数默认是int类型，浮点数默认是double类型，long 类型的变量要加了或者L，float类型的变量，要加f或者F
		同一对{}里，不能有同名的变量
		public class l1_6 {
    public static void main(String[] args){
        byte b=1;
        System.out.println(1);
        System.out.println(b);
        short s=100;
        System.out.println(s);
        int i=100000;
        System.out.println(i);
//        int j=2147483648;
//        System.out.println(j);
        long l=2147483647l;
        System.out.println(l);
        float f=12.34f;
        System.out.println(f);
        double d=23.56;
        System.out.println(d);
        char ch='a';
        System.out.println(ch);
        boolean flag=true;
        System.out.println(flag);
        
    }
}
```

### 1.5数组

```java
import java.util.Scanner;
public class l1_7 {
    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入三个数：");
        int[] arr;
        int i=0;
        arr=new int[3];
        System.out.println(arr);

        while(true){
            arr[i]=sc.nextInt();
            i++;
            if(i>=arr.length){
                break;
            }
        }
//        for(int i=1;i<arr.length;i++){
//            arr[i]=sc.nextInt();
//        }
        for(int j=0;j<arr.length;j++){
            System.out.println("arr["+j+"]="+arr[j]);
        }
        int temp=0;
        for(int j=1;j<arr.length;j++){
            temp=arr[j-1] > arr[j] ? arr[j-1] : arr[j];
        }
        System.out.println("arr数组中最大的值为： "+temp);
    }
}
import com.sun.media.jfxmediaimpl.HostUtils;
import java.util.Scanner;
public class l1_4 {
    public static void main(String[] args){
        int arr[]= new int[]{1, 2, 3, 4, 5, 6, 7, 8};
        String strArry[]= new String[]{"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
        Scanner sc = new Scanner(System.in);
        System.out.println("please input the number ");
        int index = sc.nextInt();
        index-=1;
        System.out.println("the week you looking for it the " + strArry[index]);
    }
    //遍历
    public static void printArray(int[] arr){
        System.out.println("[");
        for(int i=0;i<arr.length;i++){
            if(i<=arr.length-1){
                System.out.println(arr[i]+", ");
            }
            System.out.println(arr[i]);
        }
        System.out.println("]");
    }
    //the max and the min
    public static int getMax(int[] arr){
        int max=arr[0];
        for(int i=0;i<arr.length;i++){
            if(arr[i]>max){
                max=arr[i];
            }
        }
        return max;
    }
    //reverse
    public static void reverse(int[] arr){
        for(int start=0,end=arr.length;start<=end;start++,end--){
            int temp=arr[start];
            arr[start]=arr[end];
            arr[end]=temp;
        }
    }
    //the first index the element appear
    public static int indexOfArray(int[] arr,int key){
        int index=-1;
        for(int i=1;i<arr.length;i++){
            if(key==arr[i]){
                index=i;
                break;
            }
        }
        return index;
    }
}
```

### 1.6数据类型转换

```java
import com.sun.media.jfxmediaimpl.HostUtils;
import com.sun.xml.internal.ws.api.model.wsdl.WSDLOutput;

public class l1_8 {
    /*
        运算符
        参与运算的数据类型，要求类型一致
        boolean 不能转换为其他数据类型
        隐式转换：
            A: byte,short,char-->int-->long-->float-->double
            B: byte,short,char相互之间不转换，他们参与运算首先要转换为int
     */
    public static void main(String[] args) {
        int a=01;
        int b=20;
        System.out.println(a+b);
        int c=a+b;
        System.out.println(c);
        System.out.println("-------");

        //define the variable
        byte by=2;
        int i=4;
        System.out.println(by+i);
        int j=by+i;
        System.out.println(j);
        //强制转换
        //目标类型 变量名 = （目标类型） （被转换的数据）
        /*
        + 做加法操作
            A:针对数据操作
            B:针对字符做+
             ASCII
            C: 针对字符串做+
                在字符串的操作中，叫字符串连接符
         */
        System.out.println('a');
        System.out.println('a'+1);
        System.out.println('a'+'b');
        System.out.println("hello"+'a'+1);
        System.out.println("5+5="+5+5);

        /*
        byte
        变量相加： 首先提示类型，再做操作
        常量相加： 先操作，然后判断结果是否在左边的范围，如果在不报错，不在则报错
         */
        byte b1=3,b2=4,byt;
        //byt=b1+b2;报错
        byt=3+4;
        byte t=(byte) 130;
        System.out.println(t);
        
    }
}

```

### 1.7运算符

```java
public class l1_9 {
    public static void main(String[] args) {
        /*
        运算： 对常量和变量进行操作的过程称为运算
        运算符： 对常量和常量进行操作的符号称为运算符
        表达式： 有运算符吧常量和变量连接起来的式子
            a+b
        常见的运算符：
            算术运算符
            赋值运算符
            比较运算符
            逻辑运算符
            位运算符
            三目运算符
         */
        //算术
        System.out.println(10+20);
        System.out.println(10/20);
        System.out.println(10/1.0/20);
        System.out.println("1"+"2");
        //++ -- 同c++
        //赋值运算符
        //  =
        //  复合 += -= *= /= %=
        short s=1;
        //s=s+1; //有问题，可能精度损失
        s+=1;//等价于 ： s=(s的数据类型）（s+1）
        //关系运算符 == ！= >= <= && 和 & || 和 | 两个有短路效果 一个全部执行
        //逻辑运算符
        // & | ！ ^ 与 或 非 异或
        // >>右补零 << 根据最高位确定补1还是补0 >>>右移左边补零
        //位 ^ 运算符号： 针对一个数据异或两次，其值不变
        int a=1,b=2;
        a=a^b;
        b=a^b;
        a=a^b;
        System.out.println("a="+a+" b="+b);
        //三元运算符 （关系表达式） ？ 表达式1 ： 表达式2 ; 真执行1 假执行2
        
    }
}

```

### 1.8键盘录入数据

```java
import java.util.Scanner;
public class l1_10 {
    public static void main(String[] args) {
        /*
        A: 导包
            import java.util.Scanner;
        B: 创建对象
            Scanner sc = new Scanner (System.in);
        C: 获取数据
            int i=sc.nextInt();
         */
        Scanner sc = new Scanner(System.in);
        int a=sc.nextInt();
        System.out.println(a);

    }
}
```

### 1.9顺序结构 选择结构 循环

```java
/*
	for(初始化语句；判断条件语句；控制条件语句){
		初始化->判断->执行循环->执行控制->判断（->执行循环）
	}
	while{
	
	}
	do{
	
	}while()
	break;
	continue;
```

### 1.10 方法

```java
public class l1_11 {
    public static void main(String[] args) {
        /*
        方法 完成特定共功能的代码块
        格式
            修饰符 返回值类型 方法名（参数类型 参数名1，参数类型 参数名2）{
                方法体；
                return 返回值
            }

         */
        System.out.println(sum(1,2));
        /*
        方法重载：
            同一个类中允许存在多个同名方法，只要参数个数或者参数类型不同
            和返回值类型无关
         */
    }
    public static int sum(int a,int b){
        int c=a+b;
        return c;
    }
}

```

### 1.11数组

