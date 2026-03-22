---
title : 'Gradle 使用'
date : 2025-10-25T22:30:13+08:00
lastmod: 2024-10-25T22:20:13+08:00
description : "Android学习笔记" 
image : img/cat.jpg
draft : false    
categories : ["Android学习笔记"]
tags : ["Android"]
# password : leetcode
---

# Gradle 使用

## Sync

### **gradle下载镜像**

使用官网下载非常的慢，建议使用镜像站：

（1）官网地址：https://services.gradle.org/distributions/ 

（2）腾讯镜像 Gradle下载地址：https://mirrors.cloud.tencent.com/gradle/ 

（3）阿里云镜像 Gradle下载地址：https://mirrors.aliyun.com/macports/distfiles/gradle/

### **添加Maven阿里云源**:

```groovy
pluginManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
+       maven { url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        google()
        mavenCentral()
    }
}
```

### 使用代理

使用全局模式 

## 1. Gradle 与 AGP（快速理解）

- **Gradle**：通用构建工具，执行构建脚本、任务（tasks）、依赖解析、生命周期。Gradle 有不同版本（参考官方 Releases）。([Gradle](https://gradle.org/releases/?utm_source=chatgpt.com))
- **Android Gradle Plugin (AGP)**：Google 为 Android 提供的 Gradle 插件，封装了 Android-specific 的构建逻辑（如 `com.android.application`、`com.android.library`）。AGP 与 Android Studio、Gradle 版本存在兼容矩阵，常需要同时升级。查看 AGP 的发布说明以掌握特性/兼容性。([Android Developers](https://developer.android.com/build/releases/gradle-plugin?utm_source=chatgpt.com))

------

## 2. 项目内重要文件与含义（根目录）

- `gradle/wrapper/gradle-wrapper.properties`：声明用哪个 Gradle 发行版（Wrapper 会自动下载）。推荐使用 Wrapper 而不是系统级 `gradle`。示例：

```properties
distributionUrl=https\://services.gradle.org/distributions/gradle-8.6-all.zip
```

（修改后运行 `./gradlew wrapper` 可更新）｡ ([docs.gradle.org](https://docs.gradle.org/current/userguide/gradle_wrapper.html?utm_source=chatgpt.com))

- `gradle.properties`：Gradle 层级配置（如 `org.gradle.jvmargs=-Xmx4g`、`kotlin.code.style=official`、`android.useAndroidX=true` 等）。
- `local.properties`：本地 SDK 路径等私有设置（不要提交到 VCS）：例如 `sdk.dir=/path/to/Android/Sdk`。
- `settings.gradle` / `settings.gradle.kts`：声明多 module 配置（`include(":app", ":lib")`）与构建特性（例如 `dependencyResolutionManagement`）。([Android Developers](https://developer.android.com/build?utm_source=chatgpt.com))
- `build.gradle` / `build.gradle.kts`（根与 module 级）：根 build 定义共享版本/插件，module build 定义插件（`com.android.application`）和依赖。

------

## 3. Gradle Wrapper（强烈推荐）与基础命令

**为什么用 Wrapper？** 一致的 Gradle 版本、CI 无需先装 Gradle、跨机器复现性。Wrapper 包含 `gradlew`（Linux/macOS）和 `gradlew.bat`（Windows）。运行方式（示例）：

```bash
# macOS / Linux
./gradlew tasks

# Windows (PowerShell 或 cmd)
gradlew.bat tasks
```

Wrapper 的行为、创建与工作方式请参看官方文档。([docs.gradle.org](https://docs.gradle.org/current/userguide/gradle_wrapper.html?utm_source=chatgpt.com))

------

## 4. 常用命令（清晰、完整）

在项目根目录运行（`./gradlew` 或 `gradlew.bat`）：

构建 / 打包

```bash
# 构建 debug APK（不会安装）
./gradlew :app:assembleDebug

# 构建 release APK（未签名或使用签名配置）
./gradlew :app:assembleRelease

# 构建 Android App Bundle（AAB）
./gradlew :app:bundleRelease
```

安装到设备 / 模拟器

```bash
# 构建并安装 debug 到所有连接设备（仅 debugVariant）
./gradlew :app:installDebug
```

通用任务

```bash
# 执行单个 module 的所有 build lifecycle（包括 test）
./gradlew :app:build

# 清理
./gradlew clean
```

调试/信息

```bash
# 列出所有任务
./gradlew tasks --all

# 列出依赖树
./gradlew :app:dependencies

# 刷新依赖缓存（强制重新下载）
./gradlew build --refresh-dependencies

# 打开 stacktrace 或 更详细日志
./gradlew build --stacktrace
./gradlew build --info
./gradlew build --debug
```

常见并发/缓存参数（CLI）

```bash
./gradlew build --parallel --configure-on-demand
```

备注：`assemble` 与 `build` 的区别：`assemble` 只生成 APK/AAB，`build` 通常还会运行检查、测试等任务（视插件配置）。可参考社区讨论。([Stack Overflow](https://stackoverflow.com/questions/44185165/what-are-the-differences-between-gradle-assemble-and-gradle-build-tasks?utm_source=chatgpt.com))

------

## 5. 常见配置示例（实战片段）

### 根 build.gradle（Groovy）

```groovy
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:8.10.0" // AGP 版本
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.9.0"
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}
```

### 根 build.gradle.kts（Kotlin DSL）

```kotlin
plugins {
    id("com.android.application") version "8.10.0" apply false
    kotlin("jvm") version "1.9.0" apply false
}

pluginManagement {
    repositories {
        gradlePluginPortal()
        google()
        mavenCentral()
    }
}

dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}
```

（Kotlin DSL 的迁移与差异见官方迁移指南）。([Android Developers](https://developer.android.com/build/migrate-to-kotlin-dsl?utm_source=chatgpt.com))

### module-level（app）示例（Groovy）

```groovy
plugins {
    id 'com.android.application'
    id 'kotlin-android'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.app"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        debug {
            applicationIdSuffix ".debug"
            debuggable true
        }
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
    }

    // productFlavors 示例
    flavorDimensions "tier"
    productFlavors {
        free { dimension "tier" }
        paid { dimension "tier" }
    }
}

dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib:1.9.0"
    implementation 'androidx.core:core-ktx:1.11.0'
}
```

### 签名配置（示例片段，建议放在 `gradle.properties` 或外部安全存储）

```groovy
signingConfigs {
    release {
        storeFile file(System.getenv("KEYSTORE_PATH"))
        storePassword System.getenv("KEYSTORE_PASS")
        keyAlias System.getenv("KEY_ALIAS")
        keyPassword System.getenv("KEY_PASS")
    }
}
```

### R8 / ProGuard

- `minifyEnabled true` + 指定 `proguard-rules.pro`。AGP 使用 R8（modern replacement）进行压缩/混淆/优化。确保为第三方库添加必要的 keep 规则。

------

## 6. 构建性能优化 & 缓存技巧

- 使用 **Gradle Daemon**（通常默认开启）与合适的 `org.gradle.jvmargs`（例如 `-Xmx3g`）。
- 开启并行构建 `org.gradle.parallel=true`（多 module 项目有明显收益）。
- 使用 **配置按需**（`--configure-on-demand` 或 `org.gradle.configureondemand=true`）。
- 启用 **Build Cache**（本地或远程）来复用任务输出。
- 使用 `kotlin.incremental=true`、`kapt.use.worker.api=true` 等 Kotlin-specific 优化。
- 把不常变化的依赖固定版本，避免频繁解析。
- 避免在 `build.gradle` 中执行重量级同步/网络操作（会影响配置阶段）。

------

## 7. 在 CI 中的典型脚本（示例：Linux runner）

```bash
# 安装 JDK（根据项目需要），设置 ANDROID_HOME / ANDROID_SDK_ROOT
export ANDROID_SDK_ROOT=/opt/android-sdk
export PATH=$PATH:$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/platform-tools

# 使用 gradle wrapper（无需全局 gradle）
./gradlew :app:assembleRelease --no-daemon --stacktrace
```

建议缓存 `~/.gradle` 与 `~/.m2`（若使用 Maven），并固定 Wrapper 与 AGP 版本以避免“突然失败”。

------

## 8. 升级 AGP 与 Gradle：步骤与注意事项

1. **查看兼容矩阵**：AGP 与 Gradle、Android Studio 三者有兼容要求（升级前查官方表）。([Android Developers](https://developer.android.com/build/releases/gradle-plugin?utm_source=chatgpt.com))
2. **升级顺序建议**：先升级 Gradle Wrapper（`gradle-wrapper.properties`），再升级 AGP（根 `build.gradle` 的 classpath）或 plugins DSL 的版本。
3. **本地测试**：在分支上做变更并在 CI 执行完整构建与测试。
4. **解决废弃 API**：新 AGP 版本会移除或标记废弃配置，按编译器/IDE 提示修复。
5. **查阅迁移指南**：官方会提供迁移文档，尤其是大版本（如 AGP 7.x → 8.x）。([Android Developers](https://developer.android.com/build/releases/past-releases/agp-8-10-0-release-notes?utm_source=chatgpt.com))

示例：修改 `gradle/wrapper/gradle-wrapper.properties`：

```properties
distributionUrl=https\://services.gradle.org/distributions/gradle-8.9-all.zip
```

然后在根目录运行：

```bash
./gradlew --version
./gradlew clean build --refresh-dependencies
```

------

## 9. 常见问题与排查命令

- **任务不存在（Task with path 'assembleRelease' not found）**：确认 module 名称与任务是否存在（`:app:assembleRelease`），或 module 是否应用了 `com.android.application`/`com.android.library` 插件。([Gradle Forums](https://discuss.gradle.org/t/dependson-assemblerelease-fails-task-with-path-assemblerelease-not-found-in-project/30688?utm_source=chatgpt.com))
- **依赖解析失败**：使用 `./gradlew :app:dependencies` 检查冲突，`--refresh-dependencies` 强制刷新。
- **构建时间长**：启用 `--info` 或 `--profile` 生成构建性能分析报告（`./gradlew build --profile`），分析瓶颈。
- **签名/密钥泄露风险**：不要把 keystore 或密码提交到 VCS，使用 CI secrets 或 `gradle.properties` 在 CI 中注入。
- **堆内存/OOM**：增大 `org.gradle.jvmargs` 或降低并行数。

常用排错命令（可直接执行）

```bash
./gradlew :app:dependencies
./gradlew :app:tasks --all
./gradlew assembleDebug --stacktrace --info
./gradlew build --scan   # 若启用 build scan
```

------

## 10. 最佳实践清单（快速掌握）

- 一律使用 **Gradle Wrapper**（`./gradlew`）。([docs.gradle.org](https://docs.gradle.org/current/userguide/gradle_wrapper.html?utm_source=chatgpt.com))
- 固定 AGP 与 Gradle 版本并在升级前查兼容性表。([Android Developers](https://developer.android.com/build/releases/gradle-plugin?utm_source=chatgpt.com))
- 把敏感信息（keystore 密码）通过 CI Secrets 注入，不提交到仓库。
- 优先把公共配置放到根 `build.gradle`（version 管理、repositories）。
- 使用本地/远程 **Build Cache** 与并行构建来提升速度。
- 在 CI 中缓存 `~/.gradle`、使用 Wrapper，并在 PR/分支上运行完整构建与测试。
- 当项目复杂度上升，考虑迁移到 **Kotlin DSL**（但需评估构建性能差异与团队熟悉度）。([Android Developers](https://developer.android.com/build/migrate-to-kotlin-dsl?utm_source=chatgpt.com))

------

### 附：常用命令速查表（复制即可运行）

```bash
# 列出任务
./gradlew tasks --all

# 构建 debug APK
./gradlew :app:assembleDebug

# 构建 release AAB
./gradlew :app:bundleRelease

# 安装 debug 到设备
./gradlew :app:installDebug

# 清理
./gradlew clean

# 列依赖树
./gradlew :app:dependencies

# 刷新依赖并构建（用于杜绝缓存相关问题）
./gradlew build --refresh-dependencies --stacktrace --info

# 并行与配置按需（提高速度）
./gradlew build --parallel --configure-on-demand
```
