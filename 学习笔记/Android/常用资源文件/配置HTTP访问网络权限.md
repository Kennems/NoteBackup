# 配置 HTTP 访问网络权限

## Android 网络权限配置

### 1. 声明网络权限 (AndroidManifest.xml)

```xml
<!-- 必须声明的网络权限 -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

### 2. 允许 HTTP 明文流量 (Android 9+)

Android 9 (API 28) 起默认禁止 HTTP 明文流量，需要额外配置：

**方案 A：全局允许 (不推荐)**

```xml
<application
    android:usesCleartextTraffic="true"
    ...>
</application>
```

**方案 B：网络安全配置 (推荐)**

1. 创建 `res/xml/network_security_config.xml`：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <!-- 允许指定域名的 HTTP 访问 -->
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">192.168.1.100</domain>
        <domain includeSubdomains="true">10.0.2.2</domain>
    </domain-config>
</network-security-config>
```

2. 在 AndroidManifest.xml 中引用：

```xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    ...>
</application>
```

### 3. 常用场景

| 场景 | 地址 | 说明 |
|------|------|------|
| Android 模拟器访问宿主机 | `10.0.2.2` | 模拟器专属地址 |
| 局域网真机调试 | `192.168.x.x` | 手机和服务器同一局域网 |
| Genymotion 模拟器 | `10.0.3.2` | Genymotion 专属地址 |

### 4. Retrofit / OkHttp 示例

```kotlin
// OkHttp 客户端配置
val client = OkHttpClient.Builder()
    .connectTimeout(30, TimeUnit.SECONDS)
    .readTimeout(30, TimeUnit.SECONDS)
    .build()

// Retrofit 实例
val retrofit = Retrofit.Builder()
    .baseUrl("http://10.0.2.2:8080/")
    .client(client)
    .addConverterFactory(GsonConverterFactory.create())
    .build()
```
