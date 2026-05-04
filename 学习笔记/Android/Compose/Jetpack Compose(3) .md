---
title : 'Jetpack Compose(3)'
date : 2025-05-27T10:30:13+08:00
lastmod: 2025-05-27T10:20:13+08:00
description : "Android学习笔记" 
categories : ["Android学习笔记"]
tags : ["Android"]
---

# Jetpack Compose(3)

## 状态管理

- 应用内状态指的是可以变化的任何值



## 非结构化状态

当我们添加更多事件和状态时，可能会出现几个问题：

- 测试：由于UI的状态和Views代码交织在一起，因此很难测试此代码。
- 部分状态更新：当屏幕有更多事件时，很容易忘记更新部分状态以响应事件。因此，用户可能会看到不一致或不正确的UI。
- 部分UI更新：每次状态更改后手动更新UI，因此有时很容易忘记这一点。因此用户肯能会在UI中看到随机更新的陈旧数据。
- 代码复杂性：在这种模式下编码时很难提取一些逻辑。结果，代码有变得很难理解的趋势

## 单向数据流

- 为了帮助解决非结构化状态的这些问题，引入了ViewModel和LiveData。
- 将状态从Activity移到了ViewModel，在ViewModel中，状态由LiveData表示。

通过以这种方式构建代码

## 状态提升

Compose中的状态提升是一种将状态移至可组合项的调用方以使用可组合项无状态的模式。

无状态组件更容易测试，往往有更少的错误，并提供更多的重用机会。

![image-20250529120712899](https://raw.githubusercontent.com/Kennems/blog-image/main/image-20250529120712899.png)

## 有状态和无状态

- 使用 remember 存储对象的可组合项会创建内部状态，使该组合项有状态
- 在调用方不需要控制状态，并且不必自行管理状态便可使用状态的情况下，“有状态”会非常有用。但是，具有内部状态的可组合项往往**不易重复使用，也更难测试**。
- 无状态可组合项使指不保持任何状态的可组合项。实现无状态的一种简单方法是使用**状态提升**。

## MutableState

通过以下MutableState三种方式声明一个可组合对象：

```kotlin
val state = remember { mutableStateOf("") }
val value by remember { mutableStateOf("") }
val (value, setValue) = remember { mutableStateOf("") }
```

- 在组合中创建`State<T>`（或其他有状态对象）时，请务必对其执行`remember`操作，否则它会在每次重组时重新初始化。
- `MutableState<T>`类似于`MutableLiveData<T>`，但与Compose运行时集成。由于它时可观察的，它会在更新时通知Compose。

## 软键盘

- keyboardOptions，用于启用显示完成IME操作
- keyboardActions，用于指定响应触发的特定IME操作而触发的操作



## 恢复状态

- 在重新创建 activity 或进程后，可以使用rememberSaveable恢复界面状态。rememberSaveable可以在重组后保持状态。此外，rememberSaveable也可以在重新创建activity和进程后保持状态。



## 存储状态的方式

添加到Bundle的所有数据类型都会自动保存，如何要保存无法添加到Bundle的内容，有以下几种选择：

- Parcelize：最简单的解决方案是向对象添加 @Parcelize注解，对象将变成可打包状态并且可以捆绑
- MapSaver：如果某种原因导致 @Parcelize不合适，可以使用mapServer定义自己的规则，规定如何将对象转换为系统可保存到Bundle的一组值。
- ListSaver：为了避免需要为映射定义键，也可以使用listSaver并将其索引用作键。



## 恢复状态

- 在重新创建`activity`或进程后，可以使用`rememberSaveable`恢复界面状态。`rememberSaveable`可以在重组后保持状态。此外`rememberSaveable`也可以在重新创建`activity`和进程后保持状态。

