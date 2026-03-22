## Dagger & Hilt

https://developer.android.com/training/dependency-injection/hilt-android#generated-components

## IOC(Inversion of Control)

借助于“第三方”实现具有依赖关系的对象之间的解耦

## 依赖注入(Dependency Injection)

获得依赖对象的过程被反转

构造方法注入

Setter注入

###  IOC框架

- 基于反射实现注入：Spring IOC（动态的进行依赖关系建立）
- 静态方式，程序在编译时就已经提供好了建立依赖关系的类 （Dagger2）

## Dagger作用域限定

- 使用作用域注解，可以将某个对象的生命周期限定为其组件的生命周期。这样也意味着在作用域范围内，使用到的是同一个实例
- `@Singleton`表示一个单例对象，生命周期和程序运行的生命周期保持一致







### 概念

- Hilt 是基于 Dagger 的 Android 专用 DI 框架，用来简化在 Android 应用中使用 Dagger 时所需写的大量模板代码。 ([dagger.dev](https://dagger.dev/hilt/?utm_source=chatgpt.com))
- Hilt 提供一个标准化的注入方式，为 Android 各种组件（如 Application、Activity、Fragment、ViewModel）自动生成组件／容器，从而管理其生命周期，并减少手动 wiring 的工作量。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-android?utm_source=chatgpt.com))

### 为什么要用 Hilt？

- 它减少了你手动创建组件、模块、注入器等的样板代码（boilerplate）– 你不用自己写 Dagger 的大量配置。 ([blog.shipbook.io](https://blog.shipbook.io/hilt?utm_source=chatgpt.com))
- 它在编译时就能进行依赖关系验证，减少运行时错误（比如缺少绑定或循环依赖） 。 ([blog.shipbook.io](https://blog.shipbook.io/hilt?utm_source=chatgpt.com))
- 它与 Android 的 Jetpack 组件（比如 ViewModel、Navigation、Compose、WorkManager）整合得很好。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-jetpack?utm_source=chatgpt.com))

### 核心注解 & 概念

下面列几个你在实践中会频繁看到的注解／概念：

- `@HiltAndroidApp`：用于你的 `Application` 类，标记整个应用开始由 Hilt 管理。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-android?utm_source=chatgpt.com))
- `@AndroidEntryPoint`：用于 Activity、Fragment、View、Service 等 Android 框架类，标记它们可以接收依赖注入。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-android?utm_source=chatgpt.com))
- `@Inject`：用于构造函数注入／字段注入，告诉 Hilt “我需要这个依赖”。 ([Medium](https://tomas-repcik.medium.com/dependency-injection-with-hilt-in-android-development-e23fc636d65c?utm_source=chatgpt.com))
- `@Module` + `@InstallIn(...)`：用于定义 Hilt 如何提供某些类型的依赖（尤其是接口、第三方库类、或者你不能直接构造的类） 。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-android?utm_source=chatgpt.com))
- 作用域（Scope）：如 `@Singleton`、`@ActivityScoped`、`@ViewModelScoped` 等，控制依赖实例的生命周期。 ([Medium](https://medium.com/@anandgaur2207/dagger-hilt-in-android-ffcc08a343f3?utm_source=chatgpt.com))

### 适用场景 &限制

- 适合大中型 Android 应用，用来管理复杂的依赖关系、提高模块化、可测试性。
- 如果你的项目很简单、依赖少，也有轻量方案（如 Koin）可考虑。
- 需要注意：在 “纯 Android 库模块” 中使用 Hilt 有一些限制（比如应用模块需要拥有 `@HiltAndroidApp` 的 Application） 。 ([Stack Overflow](https://stackoverflow.com/questions/63246625/hilt-using-in-android-library?utm_source=chatgpt.com))

## Hilt Demo

### 步骤 0：创建项目

1. 在 Android Studio 新建一个 “Empty Activity” 项目，选择 Kotlin。
2. 确保项目的 `compileSdkVersion`、`targetSdkVersion` 都是较新的版本（例如 33 或 34），并启用 Java 8 兼容。

### 步骤 1：在 Gradle 中添加 Hilt 配置

在项目根级 `build.gradle`（如果是 Kotlin DSL 则 `build.gradle.kts`）中加入插件声明：

```kotlin
plugins {
//    https://mvnrepository.com/artifact/com.google.dagger/hilt-android
    id("com.google.dagger.hilt.android") version "2.57.2" apply false
//    https://github.com/google/ksp/releases
    id("com.google.devtools.ksp") version "2.2.20-2.0.4" apply false
}
```

在 app 模块 `build.gradle` 中：

```kotlin
plugins {
    id("com.google.devtools.ksp")
    id("com.google.dagger.hilt.android")
}

dependencies {
    implementation("com.google.dagger:hilt-android:2.57.2")
    ksp("com.google.dagger:hilt-android-compiler:2.57.2")
}
```

### 步骤 2：创建 Application 类并标注

```kotlin
@HiltAndroidApp
class MyApplication: Application() {

}
```

这样 Hilt 会生成基础组件并与 Application 生命周期关联。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-android?utm_source=chatgpt.com))

并在 `AndroidManifest.xml` 中将 application name 指向这个类：

```xml
<application
    android:name=".MyApplication"
    ...
>
```

### 步骤 3：创建一个 “服务” 类（依赖项）

假设你有一个简单的接口 `GreetingService`，实现为 `GreetingServiceImpl`。

```kotlin
interface GreetingService {
    fun greet(name: String): String
}

class GreetingServiceImpl @Inject constructor() : GreetingService {
    override fun greet(name: String): String {
        return "Hello, $name!"
    }
}
```

这里使用了构造函数注入 (`@Inject constructor()`) 来告诉 Hilt 如何构造 `GreetingServiceImpl`。

### 步骤 4：如果需要，为接口提供 Module

因为 `GreetingService` 是接口，Hilt 无法直接构造，所以我们需要一个 Module 提供绑定。

```kotlin
@Module
@InstallIn(SingletonComponent::class)
abstract class ServiceModule {
    @Binds
    @Singleton
    abstract fun bindsGreetingService(
        impl: GreetingServiceImpl
    ): GreetingService
}
```

- `@Module` 标识这是一个 Hilt 模块。
- `@InstallIn(SingletonComponent::class)` 指明这个绑定存在于应用级（单例）组件中。
- `@Binds` 用来绑定接口与实现。
- `@Singleton` 用来表示单例作用域。

### 步骤 5：在 ViewModel 中注入依赖

```kotlin
@HiltViewModel
class MyViewModel @Inject constructor(
    private val greetingService: GreetingService
) : ViewModel() {
    fun getGreeting(name: String): String {
        return greetingService.greet(name)
    }
}
```

然后在 `MainActivity` 中：

```kotlin
private val viewModel: MyViewModel by viewModels()
// 在 onCreate 中
val message = viewModel.getGreeting("Kennem")
```

### 步骤 6：在 Activity/Fragment 中注入依赖

假设在 `MainActivity.kt` 中：

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val viewModel : MyViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        binding.tv.text = viewModel.getGreeting("kennem")
    }
}
```

- `@AndroidEntryPoint` 标注 Activity 使 Hilt 能将其视为注入目标。 ([Android Developers](https://developer.android.com/training/dependency-injection/hilt-android?utm_source=chatgpt.com))
- 使用 `@Inject` 字段注入服务。
- 注意：字段不能是 private。

