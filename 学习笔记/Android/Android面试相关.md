---
title : 'Android面试相关'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-05-04T12:51:46+08:00
description : "- 说下Activity生命周期"
image : img/cat.jpg
draft : false
categories : ["Android"]
tags : ["学习笔记", "Android"]
---
# Android面试相关

## 1.Activity

- 说下Activity生命周期
- Activity A 启动另一个Activity B 会调用哪些方法？如果B是透明主题的又或者是个DialogActivity呢
- 说下onSaveInstanceState()方法的作用 ? 何时会被调用？
- Activity的启动流程
- onSaveInstanceState(),onRestoreInstanceState的调用时机
- activity的启动模式和使用场景
- Activity A跳转Activity B，再按返回键，生命周期执行的顺序
- 横竖屏切换,按home键,按返回键,锁屏与解锁屏幕,跳转透明Activity界面,启动一个 Theme 为 Dialog 的 Activity，弹出Dialog时Activity的生命周期
- onStart 和 onResume、onPause 和 onStop 的区别
- Activity之间传递数据的方式Intent是否有大小限制，如果传递的数据量偏大，有哪些方案
- Activity的onNewIntent()方法什么时候会执行
- 显示启动和隐式启动
- scheme使用场景,协议格式,如何使用
- ANR 的四种场景
- onCreate和onRestoreInstance方法中恢复数据时的区别
- Activity间传递数据的方式
- 跨App启动Activity的方式,注意事项
- Activity任务栈是什么
- 有哪些Activity常用的标记位Flags
- Activity的数据是怎么保存的,进程被Kill后,保存的数据怎么恢复的
## 2.Service

- service 的生命周期，两种启动方式的区别
- Service的两种启动方式？区别在哪
- 如何保证Service不被杀死 ？
- Service与Activity怎么实现通信
- IntentService是什么,IntentService原理，应用场景及其与Service的区别
- Service 的 onStartCommand 方法有几种返回值?各代表什么意思?
- bindService和startService混合使用的生命周期以及怎么关闭
- 用过哪些系统Service ？
- 了解ActivityManagerService吗？发挥什么作用

## 3.BroadcastReceiver

- 广播的分类和使用场景
- 广播的两种注册方式的区别
- 广播发送和接收的原理
- 本地广播和全局广播的区别

## 4.ContentProvider

- 什么是ContentProvider及其使用
- ContentProvider的权限管理
- ContentProvider,ContentResolver,ContentObserver之间的关系
- ContentProvider的实现原理
- ContentProvider的优点
- Uri 是什么

## 5.Handler

- Handler的实现原理
- 子线程中能不能直接new一个Handler,为什么主线程可以主线程的Looper第一次调用loop方法,什么时候,哪个类
- Handler导致的内存泄露原因及其解决方案
- 一个线程可以有几个Handler,几个Looper,几个MessageQueue对象
- Message对象创建的方式有哪些 & 区别？
- Message.obtain()怎么维护消息池的Handler 有哪些发送消息的方法
- Handler的post与sendMessage的区别和应用场景
- handler postDealy后消息队列有什么变化，假设先 postDelay 10s, 再postDelay 1s, 怎么处理这2条消息
- MessageQueue是什么数据结构
- Handler怎么做到的一个线程对应一个Looper，如何保证只有一个MessageQueue ThreadLocal在Handler机制中的作用
- HandlerThread是什么 & 好处 &原理 & 使用场景
- IdleHandler及其使用场景
- 消息屏障,同步屏障机制
- 子线程能不能更新UI
- 为什么Android系统不建议子线程访问UI
- Android中为什么主线程不会因为Looper.loop()里的死循环卡死
- MessageQueue#next 在没有消息的时候会阻塞，如何恢复？
- Handler消息机制中，一个looper是如何区分多个Handler的
- 当Activity有多个Handler的时候，怎么样区分当前消息由哪个Handler处理
- 处理message的时候怎么知道是去哪个callback处理的
- Looper.quit/quitSafely的区别
- 通过Handler如何实现线程的切换
- Handler 如何与 Looper 关联的
- Looper 如何与 Thread 关联的
- Looper.loop()源码
- MessageQueue的enqueueMessage()方法如何进行线程同步的
- MessageQueue的next()方法内部原理
- 子线程中是否可以用MainLooper去创建Handler，Looper和Handler是否一定处于一个线程
- ANR和Handler的联系

## 6.View绘制

- View绘制流程
- MeasureSpec是什么
- 子View创建MeasureSpec创建规则是什么
- 自定义Viewwrap_content不起作用的原因
- 在Activity中获取某个View的宽高有几种方法
- 为什么onCreate获取不到View的宽高
- View#post与Handler#post的区别
- Android绘制和屏幕刷新机制原理
- Choreography原理
- 什么是双缓冲
- 为什么使用SurfaceView
- 什么是SurfaceView
- View和SurfaceView的区别
- SurfaceView为什么可以直接子线程绘制
- SurfaceView、TextureView、SurfaceTexture、GLSurfaceView
- getWidth()方法和getMeasureWidth()区别
- invalidate() 和 postInvalidate() 的区别
- Requestlayout，onlayout，onDraw，DrawChild区别与联系
- LinearLayout、FrameLayout 和 RelativeLayout 哪个效率高
- LinearLayout的绘制流程
- 自定义 View 的流程和注意事项
- 自定义View如何考虑机型适配
- 自定义控件优化方案
- invalidate怎么局部刷新
- View加载流程（setContentView）

## 7.View事件分发

- View事件分发机制
- view的onTouchEvent，OnClickListerner和OnTouchListener的onTouch方法 三者优先级
- onTouch 和onTouchEvent 的区别
- ACTION_CANCEL什么时候触发
- 事件是先到DecorView还是先到Window
- 点击事件被拦截，但是想传到下面的View，如何操作
- 如何解决View的事件冲突
- 在 ViewGroup 中的 onTouchEvent 中消费 ACTION_DOWN 事件，ACTION_UP事件是怎么传递
- Activity ViewGroup和View都不消费ACTION_DOWN,那么ACTION_UP事件是怎么传递的
- 同时对父 View 和子 View 设置点击方法，优先响应哪个
- requestDisallowInterceptTouchEvent的调用时机

## 8.RecyclerView

- RecyclerView的多级缓存机制,每一级缓存具体作用是什么,分别在什么场景下会用到哪些缓存
- RecyclerView的滑动回收复用机制
- RecyclerView的刷新回收复用机制
- RecyclerView 为什么要预布局
- ListView 与 RecyclerView区别
- RecyclerView性能优化
  9.Viewpager&Fragment
- Fragment的生命周期 & 结合Activity的生命周期
- Activity和Fragment的通信方式， Fragment之间如何进行通信
- getFragmentManager、getSupportFragmentManager 、getChildFragmentManager之间的区别？
- 为什么使用Fragment.setArguments(Bundle)传递参数
- FragmentPagerAdapter与FragmentStatePagerAdapter的区别与使用场景
- FragmentPageAdapter和FragmentStatePageAdapter区别及使用场景
- Fragment懒加载
- ViewPager2与ViewPager区别
- Fragment嵌套问题

## 10.WebView

- 如何提高WebView加载速度
- WebView与 js的交互
- WebView的漏洞
- JsBridge原理

## 11.动画

- 动画的类型
- 补间动画和属性动画的区别
- ObjectAnimator，ValueAnimator及其区别
- TimeInterpolator插值器，自定义插值器
- TypeEvaluator估值器

## 12.Bitmap

- Bitmap 内存占用的计算
- getByteCount() & getAllocationByteCount()的区别
- Bitmap的压缩方式
- LruCache & DiskLruCache原理
- 如何设计一个图片加载库
- 有一张非常大的图片,如何去加载这张大图片
- 如果把drawable-xxhdpi下的图片移动到drawable-xhdpi下，图片内存是如何变的。
- 如果在hdpi、xxhdpi下放置了图片，加载的优先级。如果是400_800，1080_1920，加载的优先级。

## 13.mvc&mvp&mvvm

- MVC及其优缺点
- MVP及其优缺点
- MVVM及其优缺点
- MVP如何管理Presenter的生命周期，何时取消网络请求

## 14.Binder

- Android中进程和线程的关系,区别
- 为何需要进行IPC,多进程通信可能会出现什么问题
- Android中IPC方式有几种、各种方式优缺点
- 为何新增Binder来作为主要的IPC方式
- 什么是Binder
- Binder的原理
- Binder Driver 如何在内核空间中做到一次拷贝的？
- 使用Binder进行数据传输的具体过程
- Binder框架中ServiceManager的作用
- 什么是AIDL
- AIDL使用的步骤
- AIDL支持哪些数据类型
- AIDL的关键类，方法和工作流程
- 如何优化多模块都使用AIDL的情况
- 使用 Binder 传输数据的最大限制是多少，被占满后会导致什么问题
- Binder 驱动加载过程中有哪些重要的步骤
- 系统服务与bindService启动的服务的区别
- Activity的bindService流程
- 不通过AIDL，手动编码来实现Binder的通信

## 15.内存泄漏&内存溢出

- 什么是OOM & 什么是内存泄漏以及原因
- Thread是如何造成内存泄露的，如何解决？
- Handler导致的内存泄露的原因以及如何解决
- 如何加载Bitmap防止内存溢出
- MVP中如何处理Presenter层以防止内存泄漏的

## 16.性能优化

- 内存优化
- 启动优化
- 布局加载和绘制优化
- 卡顿优化
- 网络优化
  17.Window&WindowManager
- 什么是Window
- 什么是WindowManager
- 什么是ViewRootImpl
- 什么是DecorView
- Activity，View，Window三者之间的关系
- DecorView什么时候被WindowManager添加到Window中

## 18.WMS

- 什么是WMS
- WMS是如何管理Window的
- IWindowSession是什么，WindowSession的创建过程是怎样的
- WindowToken是什么
- WindowState是什么
- Android窗口大概分为几种？分组原理是什么
- Dialog的Context只能是Activity的Context，不能是Application的Context
- App应用程序如何与SurfaceFlinger通信的
- View 的绘制是如何把数据传递给 SurfaceFlinger 的
- 共享内存的具体实现是什么
- relayout是如何向SurfaceFlinger申请Surface
- 什么是Surface

## 19.AMS

- ActivityManagerService是什么？什么时候初始化的？有什么作用？
- ActivityThread是什么?ApplicationThread是什么?他们的区别
- Instrumentation是什么？和ActivityThread是什么关系？
- ActivityManagerService和zygote进程通信是如何实现的
- ActivityRecord、TaskRecord、ActivityStack，ActivityStackSupervisor，ProcessRecord
- ActivityManager、ActivityManagerService、ActivityManagerNative、ActivityManagerProxy的关系
- 手写实现简化版AMS

## 20.系统启动

- android系统启动流程
- SystemServer，ServiceManager，SystemServiceManager的关系
- 孵化应用进程这种事为什么不交给SystemServer来做，而专门设计一个Zygote
- Zygote的IPC通信机制为什么使用socket而不采用binder

## 21.App启动&打包&安装

- 应用启动流程
- apk组成和Android的打包流程
- Android的签名机制，签名如何实现的,v2相比于v1签名机制的改变
- APK的安装流程

## 22.序列化

- 什么是序列化
- 为什么需要使用序列化和反序列化
- 序列化的有哪些好处
- Serializable 和 Parcelable 的区别
- 什么是serialVersionUID
- 为什么还要显示指定serialVersionUID的值?

## 23.模块化&组件化

- 什么是模块化
- 什么是组件化
- 组件化优点和方案
- 组件独立调试
- 组件间通信
- Application动态加载
- ARouter原理

## 24.热修复&插件化

- 插件化的定义
- 插件化的优势
- 插件化框架对比
- 插件化流程
- 插件化类加载原理
- 插件化资源加载原理
- 插件化Activity加载原理
- 热修复和插件化区别
- 热修复原理

## 25.AOP

- AOP是什么
- AOP的优点
- AOP的实现方式,APT,AspectJ,ASM,epic,hook

## 26.Jetpack

- Navigation
- DataBinding
- Viewmodel
- livedata
- liferecycle

## 27.开源框架

- Okhttp源码流程,线程池
- Okhttp拦截器,addInterceptor 和 addNetworkdInterceptor区别
- Okhttp责任链模式
- Okhttp缓存怎么处理
- Okhttp连接池和socket复用
- Glide怎么绑定生命周期
- Glide缓存机制,内存缓存，磁盘缓存
- Glide与Picasso的区别
- LruCache原理
- Retrofit源码流程,动态代理
- LeakCanary弱引用,源码流程
- Eventbus
- Rxjava

# 2023最新Android中高级面试题汇总+解析

## Java面试相关

## 1.HashMap

- HashMap原理
- HashMap 有用过吗？您能给我说说他的主要用途吗？
- 您能说说 HashMap 常用操作的底层实现原理吗？如存储 put(K key, V value)，查找 get(Object key)，删除 remove(Object key)，修改 replace(K key, V value)等操作
- hash 冲突（或者叫 hash 碰撞）是什么？为什么会出现这种现象，如何解 决 hash 冲突？
- HashMap 的容量为什么一定要是 2 的 n 次方？
- 您能说说 HashMap 和 HashTable 的区别吗？
- HashMap中put()如何实现的
- HashMap中get()如何实现的
- 为什么HashMap线程不安全
- HashMap1.7和1.8有哪些区别
- 解决hash冲突的时候，为什么用红黑树
- 红黑树的效率高，为什么一开始不用红黑树存储
- 不用红黑树，用二叉查找树可以不
- 为什么阀值是8才转为红黑树
- 为什么退化为链表的阈值是6
- hash冲突有哪些解决办法
- HashMap在什么条件下扩容
- HashMap中hash函数怎么实现的，还有哪些hash函数的实现方式
- 为什么不直接将hashcode作为哈希值去做取模,而是要先高16位异或低16位
- 为什么扩容是2的次幂
- 链表的查找的时间复杂度是多少
- 红黑树

## 2.ArrayList

- ArrayList定义
- ArrayList 的构造器
- add 方法源码分析
- get 方法源码分析
- set 方法源码分析
- ArrayList和LinkedList的区别，以及应用场景

## 3.LinkedList

- LinkedList 定义
- LinkedList 支持的操作
- Node 类
- addFirst 源码分析
- getFirst 方法源码分析
- removeFirst 方法源码分析
- add(int index, E e)方法源码分析
## 4.HashSet 源码

- 属性
- 构造方法
- 添加元素
- 删除元素
- 查询元素
- 遍历元素
- 全部
## 5.内存模型

- 内存模型产生背景
- 物理机的并发问题
- Java 内存模型的组成分析
- Java 内存间的交互操作
- Java 内存模型运行规则
## 6.垃圾回收算法（JVM）


- JVM的内存模型,每个里面都保存的什么
- 类加载机制的几个阶段加载、验证、准备、解析、初始化、使用、卸载
- 对象实例化时的顺序
- 类加载器,双亲委派及其优势
- 垃圾回收机制
- 谈谈对 JVM 的理解?
- JVM 内存区域，开线程影响哪块区域内存？
- 对 Dalvik、ART 虚拟机有什么了解？对比

## 7.多线程

- 谈一谈java线程模型
- Java中创建线程的方式,Callable,Runnable,Future,FutureTask
- 线程的几种状态
- 谈谈线程死锁，如何有效的避免线程死锁？
- 如何实现多线程中的同步
- synchronized和Lock的使用、区别,原理；
- volatile，synchronized和volatile的区别？为何不用volatile替代synchronized？
- 锁的分类，锁的几种状态，CAS原理
- 为什么会有线程安全？如何保证线程安全
- sleep()与wait()区别,run和start的区别,notify和notifyall区别,锁池,等待池
- Java多线程通信
- 为什么Java用线程池
- Java中的线程池参数,共有几种
- 说下 Java 中的线程创建方式，线程池的工作原理

## 8.注解

- 注解的分类和底层实现原理
- 自定义注解

## 9.反射

- 什么是反射
- 反射机制的相关类
- 反射中如何获取Class类的实例
- 如何获取一个类的属性对象 & 构造器对象 & 方法对象
- Class.getField和Class.getDeclaredField的区别，getDeclaredMethod和getMethod的区别
- 反射机制的优缺点

## 10.泛型

- 泛型概念的提出（为什么需要泛型）？
- 什么是泛型？
- 自定义泛型接口、泛型类和泛型方法
- 类型通配符

## 11.设计模式

- 你所知道的设计模式有哪些
- 单例设计模式
- 工厂设计模式
- 建造者模式（Builder）
- 适配器设计模式
- 装饰模式（Decorator）
- 策略模式（strategy）
- 观察者模式（Observer）

好的，下面是用 **Java** 实现的设计模式满分答案，包括每个设计模式的定义、使用场景以及代码示例：

------

### 1. 你所知道的设计模式有哪些

设计模式通常分为三大类：

- **创建型模式**：
  - **单例模式（Singleton）**
  - **工厂模式（Factory）**
  - **抽象工厂模式（Abstract Factory）**
  - **建造者模式（Builder）**
  - **原型模式（Prototype）**
- **结构型模式**：
  - **适配器模式（Adapter）**
  - **装饰器模式（Decorator）**
  - **外观模式（Facade）**
  - **桥接模式（Bridge）**
  - **组合模式（Composite）**
  - **享元模式（Flyweight）**
  - **代理模式（Proxy）**
- **行为型模式**：
  - **策略模式（Strategy）**
  - **观察者模式（Observer）**
  - **命令模式（Command）**
  - **责任链模式（Chain of Responsibility）**
  - **状态模式（State）**
  - **模板方法模式（Template Method）**
  - **访问者模式（Visitor）**
  - **中介者模式（Mediator）**
  - **备忘录模式（Memento）**

------

### 2. 单例设计模式（Singleton）

- **定义**：单例模式确保一个类只有一个实例，并提供一个全局访问点来获取该实例。
- **使用场景**：当需要确保类有且只有一个实例时，例如数据库连接、线程池等。
- **实现方式**：可以通过**饿汉式**和**懒汉式**来实现。

```java
// 饿汉式
public class Singleton {
    private static final Singleton instance = new Singleton();
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        return instance;
    }
}
// 懒汉式（线程安全）
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

------

### 3. 工厂设计模式（Factory）

- **定义**：工厂模式提供一个接口用于创建对象，但不暴露创建的具体实现。
- **使用场景**：当创建对象的过程较为复杂，或者需要创建多个类型的对象时，工厂模式可以提供解耦。

```java
// Animal接口
public interface Animal {
    void speak();
}

// Dog类
public class Dog implements Animal {
    @Override
    public void speak() {
        System.out.println("Woof!");
    }
}

// Cat类
public class Cat implements Animal {
    @Override
    public void speak() {
        System.out.println("Meow!");
    }
}

// AnimalFactory类
public class AnimalFactory {
    public Animal createAnimal(String animalType) {
        if ("dog".equalsIgnoreCase(animalType)) {
            return new Dog();
        } else if ("cat".equalsIgnoreCase(animalType)) {
            return new Cat();
        }
        return null;
    }
}
```

------

### 4. 建造者模式（Builder）

- **定义**：建造者模式通过将一个复杂对象的构建过程分解为多个简单的步骤，使得客户端可以按步骤生成不同表示的对象。
- **使用场景**：当对象的构建过程非常复杂，或者需要灵活控制构建的步骤时，建造者模式提供了很好的解决方案。

```java
// Product类
public class Product {
    private String part1;
    private String part2;

    public void setPart1(String part1) {
        this.part1 = part1;
    }

    public void setPart2(String part2) {
        this.part2 = part2;
    }

    @Override
    public String toString() {
        return "Product [part1=" + part1 + ", part2=" + part2 + "]";
    }
}

// Builder接口
public interface Builder {
    void buildPart1();
    void buildPart2();
    Product getResult();
}

// ConcreteBuilder类
public class ConcreteBuilder implements Builder {
    private Product product = new Product();

    @Override
    public void buildPart1() {
        product.setPart1("Part 1");
    }

    @Override
    public void buildPart2() {
        product.setPart2("Part 2");
    }

    @Override
    public Product getResult() {
        return product;
    }
}

// Director类
public class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }

    public Product construct() {
        builder.buildPart1();
        builder.buildPart2();
        return builder.getResult();
    }
}
```

------

### 5. 适配器设计模式（Adapter）

- **定义**：适配器模式通过创建一个适配器类，将不兼容的接口转换为客户端所期望的接口。
- **使用场景**：常见于需要与第三方库或老旧系统进行集成时。

```java
// Target接口
public interface Target {
    void request();
}

// Adaptee类
public class Adaptee {
    public void specificRequest() {
        System.out.println("Specific request");
    }
}

// Adapter类
public class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest();
    }
}
```

------

### 6. 装饰模式（Decorator）

- **定义**：装饰模式允许动态地为一个对象添加新的功能，而无需改变其结构。
- **使用场景**：当需要给某个对象增加额外功能时，使用装饰模式可以不修改原有代码。

```java
// Coffee接口
public interface Coffee {
    int cost();
}

// SimpleCoffee类
public class SimpleCoffee implements Coffee {
    @Override
    public int cost() {
        return 5;
    }
}

// MilkDecorator类
public class MilkDecorator implements Coffee {
    private Coffee coffee;

    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public int cost() {
        return coffee.cost() + 2;
    }
}
```

------

### 7. 策略模式（Strategy）

- **定义**：策略模式定义了一系列算法，并将每一个算法封装起来，使它们可以互换使用。
- **使用场景**：例如，排序算法的选择、支付方式的切换等。

```java
// Strategy接口
public interface Strategy {
    String execute();
}

// ConcreteStrategyA类
public class ConcreteStrategyA implements Strategy {
    @Override
    public String execute() {
        return "Strategy A";
    }
}

// Context类
public class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public String executeStrategy() {
        return strategy.execute();
    }
}
```

------

### 8. 观察者模式（Observer）

- **定义**：观察者模式定义了一种一对多的依赖关系，当一个对象状态发生变化时，所有依赖它的对象都会收到通知。
- **使用场景**：如事件驱动编程、UI组件与数据绑定等。

```java
// Observer接口
public interface Observer {
    void update(String state);
}

// Subject类
public class Subject {
    private List<Observer> observers = new ArrayList<>();

    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    public void notifyObservers(String state) {
        for (Observer observer : observers) {
            observer.update(state);
        }
    }
}

// ConcreteObserver类
public class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    @Override
    public void update(String state) {
        System.out.println(name + " received state update: " + state);
    }
}
```

