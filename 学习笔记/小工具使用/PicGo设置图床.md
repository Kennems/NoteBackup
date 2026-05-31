---
title : 'PicGo设置图床'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "PicGo 是一款开源的图片上传与管理工具，支持将本地图片快速上传到各种图床（图片托管服务），并自动生成可用的图片链接。它解决了 Markdown 写作中最头疼..."
image : img/cat.jpg
draft : false
categories : ["小工具使用"]
tags : ["学习笔记", "小工具使用"]
---
# PicGo设置图床

## 一、什么是 PicGo

PicGo 是一款开源的图片上传与管理工具，支持将本地图片快速上传到各种图床（图片托管服务），并自动生成可用的图片链接。它解决了 Markdown 写作中最头疼的图片管理问题：不再需要手动上传图片、复制链接、管理本地图片文件。

### 为什么需要图床

- **Markdown 写作**：笔记中的图片需要可访问的 URL，本地图片无法在发布后显示
- **节省空间**：图片托管在云端，本地仓库体积大幅减小
- **多端同步**：无论在哪台设备上写作，图片都能正常加载
- **备份安全**：图片不会随本地文件丢失而丢失

### 核心特性

- 支持拖拽、剪贴板上传、快捷键上传
- 支持批量上传与批量删除
- 支持自定义图片链接格式
- 支持插件扩展（WebUploader、水印插件等）
- 跨平台（Windows、macOS、Linux）

## 二、下载与安装

### macOS

```bash
# 方式一：Homebrew
brew install picgo --cask

# 方式二：从 GitHub Releases 下载
# 访问 https://github.com/Molunerfinn/PicGo/releases
# 下载 PicGo-x.x.x.dmg
```

### Windows

从 GitHub Releases 下载 `.exe` 安装包，或使用 Scoop：

```bash
scoop bucket add extras
scoop install picgo
```

### Linux

```bash
# AppImage 方式
wget https://github.com/Molunerfinn/PicGo/releases/latest/download/PicGo-x.x.x.AppImage
chmod +x PicGo-x.x.x.AppImage
./PicGo-x.x.x.AppImage
```

安装后首次打开，PicGo 会驻留在系统托盘（macOS 菜单栏、Windows 右下角），右键点击图标即可进入主窗口。

## 三、支持的图床服务

PicGo 内置支持多种图床，按类型可分为：

### 免费图床

| 图床 | 特点 | 限制 |
|------|------|------|
| **SM.MS** | 注册即用，无需配置 | 免费版 5GB 容量，单文件 ≤5MB |
| **GitHub** | 利用 GitHub 仓库存储 | 公开仓库无容量限制，需配置 Token |
| **Imgur** | 老牌免费图床 | 国内访问不稳定 |

### 国内云厂商对象存储（OSS）

| 服务 | 优点 | 费用 |
|------|------|------|
| **阿里云 OSS** | 速度快，国内首选 | 按量付费，40GB ≈ 9 元/月 |
| **腾讯云 COS** | 与 GitHub 搭配友好 | 50GB 免费额度 |
| **七牛云 Kodo** | 10GB 永久免费 | 需要备案域名 |
| **又拍云 USS** | 支持 HTTPS | 按量付费 |
| **华为云 OBS** | 企业级稳定性 | 按量付费 |
| **京东云 OSS** | — | 按量付费 |

### 自建/其他

- **MinIO**：自建私有图床
- **S3 协议**：兼容 AWS S3 接口的服务
- **WebUploader**：PicGo 插件，上传到自定义服务器

> **推荐方案**：日常写作用 GitHub 免费图床；生产环境/博客用阿里 OSS 或腾讯 COS。

## 四、配置 GitHub 图床（免费方案）

### 4.1 创建 GitHub 仓库

1. 登录 [GitHub](https://github.com)
2. 点击右上角 `+` → `New repository`
3. 填写仓库名（如 `blog-images`）
4. 选择 **Public**（私有仓库的图片无法被外部访问）
5. 勾选 `Initialize this repository with a README`
6. 点击 `Create repository`

### 4.2 生成 Personal Access Token

1. 点击 GitHub 右上角头像 → `Settings`
2. 左侧导航栏 → `Developer settings`
3. 点击 `Personal access tokens` → `Tokens (classic)`
4. 点击 `Generate new token` → `Generate new token (classic)`
5. 填写 Note（如 `PicGo`）
6. 过期时间选择 `No expiration`（或按需选择）
7. 勾选 `repo` 权限（全部选中）
8. 点击页面底部 `Generate token`
9. **立即复制保存生成的 Token**（关闭页面后无法再次查看）

### 4.3 配置 PicGo

打开 PicGo → **图床设置** → **GitHub 图床**：

| 配置项 | 说明 | 示例值 |
|--------|------|--------|
| 设定仓库名 | 用户名/仓库名 | `yourname/blog-images` |
| 设定分支名 | 上传到哪个分支 | `main` |
| 设定 Token | 上一步生成的内容 | `ghp_xxxxxxxxxxxx` |
| 指定存储路径 | 仓库内的目录 | `images/` |
| 设定自定义域名 | CDN 加速（可选） | `https://cdn.jsdelivr.net/gh/yourname/blog-images` |

> **关于自定义域名**：使用 jsDelivr CDN 可以加速国内访问，格式为：
> `https://cdn.jsdelivr.net/gh/用户名/仓库名`
> 例如：`https://cdn.jsdelivr.net/gh/ken/note-images`

配置完成后点击 **设为默认图床**，然后到上传区拖一张图片测试。

### 4.4 使用 jsDelivr CDN 加速

GitHub 原始域名 `raw.githubusercontent.com` 在国内访问较慢。推荐使用 jsDelivr：

```text
原始链接：
https://raw.githubusercontent.com/ken/blog-images/main/images/photo.png

CDN 加速后：
https://cdn.jsdelivr.net/gh/ken/blog-images/images/photo.png
```

在 PicGo 的 GitHub 配置中填写：
- **自定义域名**：`https://cdn.jsdelivr.net/gh/ken/blog-images`

上传后 PicGo 会自动生成带 CDN 加速的链接。

> ⚠️ jsDelivr 在国内部分地区偶有波动，如果稳定性要求高，建议走阿里云 OSS。

## 五、配置阿里云 OSS 图床

### 5.1 开通 OSS 服务

1. 登录[阿里云](https://www.aliyun.com)
2. 搜索"对象存储 OSS" → 开通服务
3. 进入控制台 → 创建 **Bucket**
   - Bucket 名称：全局唯一，如 `my-blog-images`
   - 地域：选择离你最近的区域
   - 读写权限：**公共读**
4. 创建完成后进入 Bucket 概览页

### 5.2 获取 AccessKey

1. 鼠标悬停右上角头像 → `AccessKey 管理`
2. 创建或使用已有的 AccessKey
3. 记录 **AccessKey ID** 和 **AccessKey Secret**

### 5.3 配置 PicGo

打开 PicGo → **图床设置** → **阿里云 OSS**：

| 配置项 | 说明 | 示例值 |
|--------|------|--------|
| 设定 KeyId | AccessKey ID | `LTAI5txxxxx` |
| 设定 KeySecret | AccessKey Secret | — |
| 设定 Bucket | Bucket 名称 | `my-blog-images` |
| 设定存储区域 | Bucket 所在区域 | `oss-cn-hangzhou` |
| 指定存储路径 | 上传目录前缀 | `images/` |
| 设定自定义域名 | 可选，方便迁移 | `https://my-blog-images.oss-cn-hangzhou.aliyuncs.com` |

**存储区域填写示例：**

| 地域 | Endpoint 后缀 |
|------|---------------|
| 华东 1（杭州） | `oss-cn-hangzhou` |
| 华东 2（上海） | `oss-cn-shanghai` |
| 华北 2（北京） | `oss-cn-beijing` |
| 华南 1（深圳） | `oss-cn-shenzhen` |

## 六、配置腾讯云 COS 图床

### 6.1 开通 COS 服务

1. 登录[腾讯云](https://cloud.tencent.com)
2. 搜索"对象存储 COS" → 开通服务
3. 进入控制台 → 创建 **存储桶**
   - 名称：如 `blog-images-125xxxxxxx`
   - 所属地域：就近选择
   - 访问权限：**公有读私有写**
4. 记录存储桶名称和所属地域

### 6.2 获取密钥

1. 进入 [API 密钥管理](https://console.cloud.tencent.com/cam/capi)
2. 创建或使用已有密钥
3. 记录 **SecretId** 和 **SecretKey**

### 6.3 配置 PicGo

打开 PicGo → **图床设置** → **腾讯云 COS**：

| 配置项 | 说明 | 示例值 |
|--------|------|--------|
| 设定 SecretId | SecretId | `AKIDxxxxxxxx` |
| 设定 SecretKey | SecretKey | — |
| 设定 Bucket | 存储桶名称 | `blog-images-125xxxxxxx` |
| 设定存储区域 | 所属地域 | `ap-guangzhou` |
| 指定存储路径 | 目录前缀 | `images/` |
| 设定自定义域名 | 可选 | `https://blog-images-125xxxxxxx.cos.ap-guangzhou.myqcloud.com` |

**存储区域填写格式：**

| 地域 | 区域简称 |
|------|----------|
| 广州 | `ap-guangzhou` |
| 上海 | `ap-shanghai` |
| 北京 | `ap-beijing` |
| 成都 | `ap-chengdu` |

## 七、PicGo 进阶设置

### 7.1 上传配置

在 PicGo 设置页面，推荐以下配置：

- **时间戳重命名**：开启，避免同名文件覆盖（如 `photo_20250524123045.png`）
- **上传后自动复制 URL**：开启
- **默认链接格式**：Markdown（`![](url)`）
- **上传前重命名**：按需开启
- **开启提示音**：上传成功时有声音反馈
- **日志记录**：开启，方便排查问题

### 7.2 快捷键

| 操作系统 | 快捷键 | 功能 |
|----------|--------|------|
| macOS | `Command+Shift+P` | 从剪贴板上传 |
| macOS | `Command+Shift+E` | 打开主窗口 |
| Windows | `Ctrl+Shift+P` | 从剪贴板上传 |
| Windows | `Ctrl+Shift+E` | 打开主窗口 |

### 7.3 推荐插件

通过 PicGo 插件市场可以安装：

| 插件名 | 功能 |
|--------|------|
| `picgo-plugin-watermark` | 上传时自动添加水印 |
| `picgo-plugin-compress` | 上传前自动压缩图片 |
| `picgo-plugin-squoosh` | 使用 Squoosh 压缩图片 |
| `picgo-plugin-pic-migr` | 迁移文章中的图片到图床 |
| `picgo-plugin-web-uploader` | 上传到自定义 Web 接口 |
| `picgo-plugin-github-plus` | GitHub 图床增强版 |

安装方式：PicGo → 插件设置 → 搜索插件名 → 安装。

## 八、与 Typora 集成

Typora 原生支持 PicGo，配置非常简单。

### 8.1 配置 Typora

1. 打开 Typora → `偏好设置`（macOS: `Cmd+,` / Windows: `Ctrl+,`）
2. 左侧选择 **图像**
3. **插入图片时** → 选择 **上传图片**
4. **图片上传设置** → 选择 **PicGo.app**（或 **Custom Command**）

### 8.2 验证配置

1. 点击 **验证图片上传选项**
2. 如果显示"验证成功"，说明配置完成
3. 也可以拖一张图片到 Typora 测试，图片会自动上传并显示

### 8.3 上传模式说明

Typora 提供三种上传策略：

- **插入图片时上传图片**：每次插入都触发上传（推荐）
- **上传图片优先**：优先使用上传后的链接
- **使用本地路径图片上传**：手动选择本地图片上传

建议选择第一种，配合 PicGo 的剪贴板上传，可以做到完全无感。

### 8.4 批量上传本地图片

如果你已经写好的笔记中包含本地图片：

1. Typora → 偏好设置 → 图像
2. 点击 **上传所有本地图片**（或 `格式` → `图像` → `上传所有本地图片`）
3. Typora 会自动将文中所有本地图片上传到图床并替换链接

> ⚠️ 批量上传前建议先备份，防止网络问题导致部分图片上传失败。

## 九、与 Obsidian 集成

Obsidian 本身没有内置 PicGo 支持，需要借助插件或外部工具。

### 9.1 方案一：Auto Upload Image 插件（推荐）

1. 打开 Obsidian → 设置 → 第三方插件 → 社区插件市场
2. 搜索并安装 **Auto Upload Image**（或 **Image Auto Upload Plugin**）
3. 启用该插件
4. 插件设置中切换到 **PicGo** 模式
5. 确保 PicGo 正在运行（后台运行即可）

**使用方式**：
- 拖拽图片到 Obsidian 笔记
- 或者粘贴剪贴板中的截图
- 插件会自动上传到 PicGo 配置的图床，并替换为 Markdown 链接

### 9.2 方案二：Obsidian Image Uploader 插件

1. 安装 **Obsidian Image Uploader** 插件
2. 配置 PicGo 路径和端口
3. 支持右键菜单上传和批量上传

### 9.3 方案三：自定义上传命令

也可以在 Obsidian 中通过自定义命令调用 PicGo：

```bash
# macOS 使用 open 命令
open -a PicGo
```

配合 Obsidian 的 **Advanced URI** 插件可以实现更精细的自动化。

### 9.4 与 iPic 配合（macOS）

如果你也在用 iPic，Obsidian 可以配置为自动调用 iPic 上传：

1. Obsidian → 设置 → 文件与链接
2. 开启 **自动将内部链接转换为 Markdown 链接**
3. 使用 iPic 的剪贴板上传功能

## 十、上传方式详解

PicGo 支持多种上传方式，适应不同场景：

### 拖拽上传

直接将图片文件或文件夹拖入 PicGo 上传区域。

### 剪贴板上传

- 使用截图工具截取图片后，PicGo 会自动感知剪贴板中的图片
- 快捷键 `Command+Shift+P`（macOS）/ `Ctrl+Shift+P`（Windows）
- 适合配合截图工作流：截图 → 快捷键 → 自动上传 → 链接已复制到剪贴板

### 批量上传

支持一次选择多张图片批量上传，PicGo 会分别上传并返回所有链接。

### 右键上传（Windows）

在文件资源管理器中选中图片 → 右键 → `通过 PicGo 上传`。

## 十一、常见问题与解决

### 11.1 GitHub 上传失败

**现象**：上传后提示失败，或图片无法显示。

**排查步骤**：

1. 检查 Token 权限：确保 Token 勾选了 `repo` 权限
2. 检查仓库可见性：**Public** 仓库才能被外部访问
3. 检查分支名：`main` 还是 `master`（GitHub 默认分支已改为 `main`）
4. 检查网络连接：能否访问 `github.com` 和 `api.github.com`
5. 检查仓库路径：用户名/仓库名 是否正确（不含 `https://`）

**常见错误**：

```text
Error: HttpError: Not Found
→ 仓库名或 Token 填写错误

Error: Request failed with status code 403
→ Token 过期或权限不足

Error: Request failed with status code 401
→ Token 无效，重新生成
```

### 11.2 阿里云 OSS 上传失败

**现象**：上传失败或 403 Forbidden。

**排查步骤**：

1. Bucket 权限设置为 **公共读**
2. AccessKey 是否有 OSS 权限（进入 RAM 用户管理检查策略）
3. 存储区域名称是否正确（如 `oss-cn-hangzhou`）
4. Bucket 名称是否正确（区分大小写）
5. 是否开启了 Bucket 的版本控制（不影响上传，但可能导致意外问题）

**防盗链设置**：如果设置了防盗链，需要在 OSS 控制台 → 防盗链中添加允许访问的域名（如 `*.typora.io`、`*你的博客域名*`）。

### 11.3 图片上传成功但无法显示

**原因分析**：

1. **CDN 缓存**：刚上传的图片需要等待 CDN 缓存刷新（jsDelivr 可通过清空 URL 强制刷新）
2. **防盗链**：OSS/COS 开启了 Referer 防盗链
3. **CORS 配置**：跨域问题，需要配置 Bucket 的 CORS 规则
4. **HTTPS 混合内容**：你的站点是 HTTPS，但图片链接是 HTTP

**jsDelivr 缓存清理**：

```text
// 使用 jsDelivr 的缓存清理 API
https://purge.jsdelivr.net/gh/用户名/仓库名/图片路径
```

### 11.4 图片命名冲突

当不同目录下有同名文件时，上传可能会互相覆盖。

**解决方案**：

- 开启 **时间戳重命名**（推荐）：`photo_20250101120000.png`
- 使用 UUID 重命名插件
- 在存储路径中按日期组织：`images/2025/01/photo.png`

### 11.5 Typora + PicGo 连接失败

**现象**：Typora 验证图片上传失败，提示"Failed to upload image"。

**解决步骤**：

1. 确保 PicGo 正在运行（检查系统托盘/菜单栏）
2. Typora 中检查 PicGo 路径是否正确
   - macOS：`/Applications/PicGo.app`
   - Windows：`C:\Users\用户名\AppData\Local\Programs\picgo\PicGo.exe`
3. 检查 PicGo 的 **监听端口** 设置（默认 36677）
4. 在 Typora 中切换到 **Custom Command** 模式，尝试手动测试

**自定义命令方案**（适合 PicGo 起不来时）：

```bash
# macOS 使用 CLI 版本（需先安装 picgo CLI）
picgo upload

# Windows PowerShell
& "C:\Users\用户名\AppData\Local\Programs\picgo\PicGo.exe" upload
```

### 11.6 PicGo 无法开机自启

macOS 添加开机启动：

```bash
# 编写 plist 文件实现
cp /Applications/PicGo.app/Contents/Info.plist ~/Library/LaunchAgents/picgo.plist
launchctl load ~/Library/LaunchAgents/picgo.plist
```

或在系统偏好设置 → 用户与群组 → 登录项中直接添加 PicGo。

Windows 可以在 PicGo 设置中直接开启"开机自启"。

### 11.7 图片链接格式选择

PicGo 支持多种 URL 格式：

| 格式 | 样例 | 适用场景 |
|------|------|----------|
| Markdown | `![](url)` | Markdown 编辑器（Typora、Obsidian） |
| HTML | `<img src="url">` | 网页、富文本编辑器 |
| URL | `url` | 需要原始链接时 |
| Base64 | `data:image/...` | 不适合图床场景 |

## 十二、进阶技巧

### 12.1 多图床同时使用

PicGo 默认只能使用一个图床作为默认图床。如果需要同时使用多个：

1. 安装 `picgo-plugin-super-prefix` 插件
2. 配置不同的上传前缀对应不同图床
3. 或使用不同配置文件，通过命令行切换

### 12.2 使用 CLI 模式

PicGo 也提供了命令行版本，适合脚本集成：

```bash
# 安装 picgo CLI
npm install picgo -g

# 上传单张图片
picgo upload /path/to/image.png

# 上传剪贴板中的图片
picgo upload

# 指定配置文件
picgo -c /path/to/config.json upload image.png
```

### 12.3 批量迁移文章图片

如果你的旧文章中已经使用了本地图片，可以使用以下工具迁移：

1. **picgo-plugin-pic-migr**：PicGo 的图片迁移插件
2. 配合正则表达式批量替换文章中的图片地址
3. 或使用 Typora 的"上传所有本地图片"功能

### 12.4 图片压缩后再上传

为了节省空间和加快加载速度，建议上传前压缩：

- 使用 PicGo 的 `picgo-plugin-compress` 或 `picgo-plugin-squoosh` 插件
- 压缩级别建议 80%-85%，肉眼几乎看不出差异
- PNG 转 JPEG 可以大幅减小体积

### 12.5 自定义水印

使用 `picgo-plugin-watermark` 插件：

1. 安装插件
2. 配置水印文字或图片
3. 设置位置（右下角、左上角等）
4. 设置透明度（建议 30%-50%）

## 十三、总结

PicGo 是 Markdown 写作中图片管理的核心工具，配合 Typora 或 Obsidian 可以做到"写笔记时完全感受不到图片的存在"。

**推荐方案总结**：

| 使用场景 | 推荐图床 | 编辑器 |
|----------|----------|--------|
| 免费/个人博客 | GitHub + jsDelivr | Typora |
| 国内博客/公司 | 阿里云 OSS | Typora |
| 知识库/笔记 | 腾讯 COS | Obsidian |
| 多设备同步 | GitHub | Obsidian |

**配置文件备份**：PicGo 的配置文件位于：

- macOS：`~/Library/Application Support/picgo/data.json`
- Windows：`C:\Users\用户名\AppData\Roaming\picgo\data.json`
- Linux：`~/.config/picgo/data.json`

更换电脑时可以直接复制该文件恢复配置。

> **温馨提示**：图床的 Token 和 Secret 是敏感信息，不要提交到 GitHub 仓库。建议将 Token 添加到 `.env` 文件或密码管理器中妥善保管。
