---
title : 'Jetpack Compose(4)'
date : 2025-06-04T10:30:13+08:00
lastmod: 2024-06-04T10:20:13+08:00
description : "Android学习笔记" 
categories : ["Android学习笔记"]
tags : ["Android"]
---

# Jetpack Compose(4)

## CompositionLocal

- 通常情况下，在`Compose`中，数据以参数形式向下流经整个页面树传递给每个可组合函数。但是对于广泛使用的常用数据（如颜色和类型样式），这样会很麻烦
- 为了支持无需将颜色作为显示参数依赖项传递给大多数可组合项，`Compose`提供了`CompositionLocal`，可以创建以树为作用域的具名对象，可以用作让数据流经页面树的一种隐式方式

## 显示传参和隐式传参

```kotlin
// 隐式传参
class ImplicitTest {
    var color: String = "red"

    private fun Layout(){
        println("layout")
        Text(color)
        provider("blue"){
            Grid(color)
        }
        Grid(color)
        Grid(color)
        Text(color)
    }

    private fun Grid(color: String){
        println("other components in grid")
        Text(color)
    }

    private fun Text(color: String) {
        println("Text")
        println(color)
    }

    private fun provider(value: String, content: () -> Unit){
        println("provider")
        val tmpColor = color

        color = value
        content()

        color = tmpColor
    }

    @Test
    fun test_implicit(){
        Layout()
    }
}
```



```kotlin
// 显式传参
class ExplicitTest {
    @Test
    fun addition_isCorrect() {
        assertEquals(4, 2 + 2)
    }

    private fun Layout() {
        println("layout")
        val color: String = "red"
        Text(color)
        Grid(color)
        Grid(color)
        Text(color)
    }

    private fun Grid(color: String) {
        println("other components in grid")
        Text(color)
    }

    private fun Text(color: String) {
        println("Text")
        println(color)
    }

    @Test
    fun test_explicit() {
        Layout()
    }
}
```

- 如果需要为`CompositionLocal`提供新值，使用`CompositionLocalProvider`及其`provides infix`函数
- `CompositionLocal`的`current`值对应于该组合部分中的某个祖先提供的最接近的值。

## 自定义CompositionalLocal

<img src="https://raw.githubusercontent.com/Kennems/blog-image/main/image-20250605164014744.png" alt="image-20250605164014744" style="zoom:50%;" />

```kotlin
data class Elevations(val card: Dp = 0.dp)

val LocalElevations = compositionLocalOf { Elevations() }

object CardElevation {
    val high: Elevations
        get() = Elevations(card = 10.dp)
    val low: Elevations
        get() = Elevations(card = 0.05.dp)
}

@Composable
fun MyCard(
    elevation: Dp = LocalElevations.current.card,
    backgroundColor: Color,
    content: @Composable ColumnScope.() -> Unit
) {
    Card(
        elevation = CardDefaults.cardElevation(
            defaultElevation = elevation
        ),
        modifier = Modifier.size(200.dp),
        colors = CardDefaults.cardColors(
            containerColor = backgroundColor
        ),
        content = content
    )
}
```



## compositionLocalOf

- 如果更改提供的值，会使读取其current值的组件发生重组

## staticCompositionLocalOf

- 与`compositionLocalOf`不同，更改值会导致提供`CompositionLocal`的整个 `content lambda` 被重组，而不仅仅是在组合中读取 `current` 值的组件
- 如果为`CompositionLocal`提供的值发生更改的可能性微乎其微或永远不会更改，使用`staticCompositionLocalOf`可提高性能

