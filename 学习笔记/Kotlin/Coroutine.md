# Coroutine

Kotlin 协程是一种轻量级的并发框架，可以在不阻塞线程的情况下执行异步代码。

## 协程 vs 线程

|      | 线程       | 协程         |
| ---- | ---------- | ------------ |
| 调度 | OS 内核    | 用户态调度   |
| 开销 | ~1MB 栈    | ~几KB        |
| 切换 | 系统调用   | 挂起函数调用 |
| 数量 | 受限       | 可创建数千个 |

## 基础用法

### 启动协程

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // launch: 不关心返回值
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello,")
}
```

### async / await

```kotlin
fun main() = runBlocking {
    val deferred = async {
        delay(1000L)
        "result"
    }
    println(deferred.await())  // 等待结果
}
```

### 挂起函数

```kotlin
suspend fun fetchData(): String {
    delay(1000L)  // 非阻塞延迟
    return "data"
}
```

## 协程上下文和作用域

```kotlin
// 指定调度器
launch(Dispatchers.IO) { /* IO操作 */ }
launch(Dispatchers.Default) { /* CPU密集 */ }
launch(Dispatchers.Main) { /* UI线程 */ }

// 自定义作用域
coroutineScope {
    launch { delay(500); println("Task1") }
    launch { delay(1000); println("Task2") }
}
println("Done")  // 等待所有子协程完成后才执行
```

## 线程操作

### 切换线程

```kotlin
launch(Dispatchers.Main) {
    val result = withContext(Dispatchers.IO) {
        // 在 IO 线程执行
        doNetworkRequest()
    }
    // 回到 Main 线程
    updateUI(result)
}
```

### 等待多个协程

```kotlin
coroutineScope {
    val job1 = launch { /* ... */ }
    val job2 = async { /* ... */ }
    job1.join()   // 等待 job1 完成
    job2.await()  // 等待 job2 完成并获取结果
}
```

### 互斥锁

```kotlin
val mutex = Mutex()
var counter = 0

suspend fun increment() {
    mutex.withLock {
        counter++
    }
}
```

## Structured Concurrency

协程的结构化并发确保：
- 父协程会等待所有子协程完成
- 子协程取消时父协程不会受到影响
- 异常会正确传播

```kotlin
runBlocking {
    launch {
        launch { /* 子协程1 */ }
        launch { /* 子协程2 */ }
    }
    // runBlocking 会等待所有 launch 完成
}
```
