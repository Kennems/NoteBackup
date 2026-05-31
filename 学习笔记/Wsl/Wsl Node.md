---
title : 'Wsl Node'
date : 2025-10-23T10:00:01+08:00
lastmod: 2025-11-09T10:00:01+08:00
description : "Wsl初始化" 
image : img/cat.jpg
draft : false    
categories : ["Linux"]
tags : ["Wsl"]
---

# WSL Node.js 开发环境搭建指南

## 一、准备工作

### 1.1 确认 WSL 版本

```bash
wsl --status
wsl -l -v
```

确保 WSL 版本为 2。如果是 WSL 1，请升级：

```bash
wsl --set-version <distro> 2
```

### 1.2 更新包管理器

```bash
sudo apt update
sudo apt upgrade -y
```

### 1.3 安装基础依赖

```bash
sudo apt install -y build-essential curl git libssl-dev
```

- `build-essential`：C/C++ 编译工具链，很多 npm 包需要编译原生模块
- `curl`：用于下载 NVM 安装脚本
- `git`：版本控制
- `libssl-dev`：OpenSSL 开发库

---

## 二、安装 NVM（Node Version Manager）

### 2.1 安装 NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

或者使用 wget：

```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

### 2.2 配置环境变量

安装脚本会自动添加以下内容到 `~/.bashrc`、`~/.zshrc` 或 `~/.profile`：

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

手动生效配置：

```bash
source ~/.bashrc
```

### 2.3 验证 NVM 安装

```bash
nvm --version
```

输出类似 `0.40.1` 即表示安装成功。

### 2.4 NVM 常用命令速查

| 命令 | 说明 |
|------|------|
| `nvm ls-remote` | 列出所有可安装的 Node.js 版本 |
| `nvm ls` | 列出本地已安装的 Node.js 版本 |
| `nvm install <version>` | 安装指定版本 |
| `nvm use <version>` | 切换使用指定版本 |
| `nvm alias default <version>` | 设置默认版本 |
| `nvm current` | 显示当前使用的版本 |
| `nvm uninstall <version>` | 卸载指定版本 |
| `nvm which <version>` | 显示指定版本的路径 |

---

## 三、安装 Node.js

### 3.1 安装 LTS 版本

推荐安装最新的 LTS 版本：

```bash
nvm install --lts
```

### 3.2 安装特定版本

```bash
nvm install 18.20.4   # Node.js 18 LTS
nvm install 20.17.0   # Node.js 20 LTS
nvm install 22.11.0   # Node.js 22 LTS（最新）
```

### 3.3 切换默认版本

```bash
nvm alias default 20.17.0
```

### 3.4 验证安装

```bash
node --version
npm --version
```

---

## 四、配置 npm

### 4.1 配置 npm 镜像源

国内环境建议使用淘宝镜像加速：

```bash
npm config set registry https://registry.npmmirror.com
```

查看当前配置：

```bash
npm config get registry
```

### 4.2 配置 npm 全局安装路径

避免全局包需要 sudo 权限：

```bash
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
```

在 `~/.bashrc` 中添加：

```bash
export PATH=~/.npm-global/bin:$PATH
```

### 4.3 配置缓存目录

```bash
npm config set cache ~/.npm-cache
```

### 4.4 保存精确版本号

```bash
npm config set save-exact true
```

---

## 五、安装包管理工具

### 5.1 Yarn

**安装 Yarn Classic：**

```bash
npm install -g yarn
yarn --version
```

**配置镜像源：**

```bash
yarn config set registry https://registry.npmmirror.com
```

**安装 Yarn Berry（Yarn v2/v3）：**

```bash
yarn set version berry
```

### 5.2 pnpm

```bash
npm install -g pnpm
pnpm --version
```

**配置镜像源：**

```bash
pnpm config set registry https://registry.npmmirror.com
```

**pnpm 优势：**

- 高效的磁盘空间利用（硬链接 + 内容寻址存储）
- 严格的依赖隔离，避免幽灵依赖
- 原生支持 monorepo

### 5.3 三者对比

| 特性 | npm | yarn | pnpm |
|------|-----|------|------|
| 安装速度 | 较慢 | 中等 | 最快 |
| 磁盘占用 | 高 | 中 | 低（硬链接） |
| 幽灵依赖 | 有 | 有 | 无 |
| Monorepo 支持 | workspaces | workspaces | 原生支持 |
| 锁定文件 | package-lock.json | yarn.lock | pnpm-lock.yaml |

---

## 六、配置 Shell 环境

### 6.1 安装 Oh My Zsh（可选）

```bash
sudo apt install -y zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 6.2 安装常用全局工具

```bash
# 开发服务器
npm install -g nodemon  # 自动重启
npm install -g ts-node  # TypeScript 直接运行
npm install -g typescript  # TypeScript 编译器

# 项目管理
npm install -g rimraf  # 跨平台 rm -rf
npm install -g cross-env  # 跨平台环境变量
npm install -g npm-check-updates  # 依赖更新检查

# 开发工具
npm install -g http-server  # 简易 HTTP 服务器
npm install -g serve  # 静态文件服务
npm install -g pm2  # 进程管理
npm install -g eslint  # 代码检查
npm install -g prettier  # 代码格式化
```

---

## 七、VS Code Remote WSL 配置

### 7.1 安装 Remote - WSL 扩展

在 VS Code 中搜索并安装 `ms-vscode-remote.remote-wsl` 扩展。

### 7.2 连接到 WSL

```bash
code .
```

或者在 VS Code 中：左下角绿色按钮 → "Open Folder in WSL"。

### 7.3 设置 VS Code 设置

在 WSL 环境中，VS Code 会使用独立的设置文件。建议配置：

```json
{
  "terminal.integrated.defaultProfile.linux": "zsh",
  "terminal.integrated.fontFamily": "'Cascadia Code', 'Fira Code', 'JetBrains Mono'",
  "editor.fontSize": 14,
  "editor.formatOnSave": true,
  "files.eol": "\n",
  "files.autoSave": "onFocusChange",
  "npm.enableScriptExplorer": true,
  "typescript.updateImportsOnFileMove.enabled": "always",
  "javascript.updateImportsOnFileMove.enabled": "always"
}
```

### 7.4 WSL 中推荐的 VS Code 扩展

```bash
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension bradlc.vscode-tailwindcss
code --install-extension ms-vscode.vscode-typescript-next
code --install-extension github.copilot
code --install-extension eamodio.gitlens
code --install-extension formulahendry.auto-rename-tag
code --install-extension christian-kohler.path-intellisense
code --install-extension christian-kohler.npm-intellisense
code --install-extension streetsidesoftware.code-spell-checker
```

---

## 八、文件系统性能优化

### 8.1 WSL 文件系统架构

WSL 中有两种文件系统：

- **WSL 原生文件系统**（`/home/username/`）：ext4 格式，性能优秀
- **Windows 文件系统**（`/mnt/c/`）：通过 9P 协议挂载，性能较差

### 8.2 性能差异对比

| 操作 | `/home/` (ext4) | `/mnt/c/` (drvfs) |
|------|-----------------|-------------------|
| `npm install` | 2-5 秒 | 20-60 秒 |
| `git clone` | 1-3 秒 | 10-30 秒 |
| 文件读写 | 原生速度 | 慢 3-5 倍 |
| `chmod`/`chown` | 支持 | 不支持（需额外配置） |

### 8.3 最佳实践

**项目存放位置：**

```
✅ 推荐：/home/username/projects/my-app
❌ 避免：/mnt/c/Users/username/projects/my-app
```

**如果需要访问 Windows 文件：**

```bash
# 创建软链接到 WSL 目录
ln -s /mnt/c/Users/username/projects ~/win-projects
```

**跨文件系统操作优化：**

在 Windows 侧的 `.wslconfig` 文件中配置（`C:\Users\<username>\.wslconfig`）：

```ini
[wsl2]
memory=8GB
processors=4
localhostForwarding=true
```

### 8.4 使用 Windows 上的 Node.js 与 WSL 交互

如果需要在 Windows 端运行 Node.js，建议同时在 Windows 和 WSL 中各安装一份工具链，避免跨文件系统访问。

---

## 九、数据库安装与配置

### 9.1 MySQL

```bash
sudo apt install -y mysql-server
sudo service mysql start
sudo mysql_secure_installation
```

**配置远程访问（开发环境）：**

```bash
sudo mysql -u root -p
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
CREATE USER 'dev'@'%' IDENTIFIED BY 'dev_password';
GRANT ALL PRIVILEGES ON *.* TO 'dev'@'%';
FLUSH PRIVILEGES;
```

**连接字符串：**

```
mysql://dev:dev_password@localhost:3306/your_database
```

### 9.2 PostgreSQL

```bash
sudo apt install -y postgresql postgresql-contrib
sudo service postgresql start
```

**创建用户和数据库：**

```bash
sudo -u postgres psql
```

```sql
CREATE USER dev WITH PASSWORD 'dev_password';
CREATE DATABASE your_database OWNER dev;
GRANT ALL PRIVILEGES ON DATABASE your_database TO dev;
```

**连接字符串：**

```
postgresql://dev:dev_password@localhost:5432/your_database
```

### 9.3 MongoDB

```bash
# 导入 MongoDB 官方 GPG 密钥
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor

# 添加仓库
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] http://repo.mongodb.org/apt/debian bookworm/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt update
sudo apt install -y mongodb-org
sudo service mongod start
```

**验证：**

```bash
mongosh --eval "db.runCommand({ connectionStatus: 1 })"
```

**连接字符串：**

```
mongodb://localhost:27017/your_database
```

### 9.4 Redis

```bash
sudo apt install -y redis-server
sudo service redis-server start
```

**验证：**

```bash
redis-cli ping  # 输出 PONG
```

**连接字符串：**

```
redis://localhost:6379
```

### 9.5 数据库管理工具

```bash
# 安装 Adminer（PHP 轻量级管理工具）
sudo apt install -y adminer

# 或者使用命令行工具
npm install -g mycli       # MySQL 客户端增强
npm install -g pgcli       # PostgreSQL 客户端增强
npm install -g redis-commander  # Redis Web 管理界面
```

---

## 十、项目初始化与常见配置

### 10.1 初始化新项目

```bash
mkdir my-project && cd my-project
nvm use 20
npm init -y
```

### 10.2 TypeScript 项目配置

```bash
npm install -D typescript @types/node ts-node
npx tsc --init
```

### 10.3 ESLint + Prettier 配置

```bash
npm install -D eslint prettier eslint-config-prettier eslint-plugin-prettier
npx eslint --init
```

### 10.4 创建 `.nvmrc` 文件

在项目根目录创建 `.nvmrc` 文件来锁定 Node.js 版本：

```bash
echo "20" > .nvmrc
```

进入项目目录时自动切换版本：

```bash
# 在 ~/.bashrc 或 ~/.zshrc 中添加
autoload -U add-zsh-hook
load-nvmrc() {
  if [[ -f .nvmrc && -r .nvmrc ]]; then
    nvm use
  fi
}
add-zsh-hook chpwd load-nvmrc
```

### 10.5 Docker 支持（可选）

安装 Docker：

```bash
# 安装 Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
```

在 WSL 中使用 Docker 推荐启用 Docker Desktop 的 WSL 2 集成，或直接在 WSL 中安装 Docker Engine。

---

## 十一、常见问题与解决方案

### 11.1 NVM 命令找不到

**问题：** 重新打开终端后 `nvm` 命令不存在。

**解决：** 检查 `~/.bashrc` 或 `~/.zshrc` 中是否包含 NVM 的初始化脚本。

```bash
# 手动加载
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

### 11.2 npm install 权限错误

**问题：** `npm install -g` 报 EACCES 错误。

**解决：** 配置 npm 全局路径（见 4.2 节），避免使用 sudo。

### 11.3 Node.js 版本不匹配

**问题：** 项目要求 Node.js 18，但系统默认是 20。

**解决：** 使用 `.nvmrc` 文件 + 钩子自动切换版本（见 10.4 节）。

### 11.4 npm install 速度慢

**问题：** 依赖安装超时或极慢。

**解决：**

```bash
# 1. 切换镜像源
npm config set registry https://registry.npmmirror.com

# 2. 设置超时时间
npm config set fetch-timeout 300000

# 3. 清理缓存重试
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

### 11.5 编译原生模块失败

**问题：** `node-gyp rebuild` 报错。

**解决：**

```bash
# 确保编译工具链完整
sudo apt install -y build-essential python3 make g++

# 如果是 node-canvas 等需要系统库的模块
sudo apt install -y libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
```

### 11.6 端口被占用

**问题：** WSL 中启动服务提示端口已被占用。

**解决：**

```bash
# 查找占用端口的进程
sudo lsof -i :3000

# 终止进程
kill -9 <PID>

# 或者使用 fuser
fuser -k 3000/tcp
```

Windows 和 WSL 共享 localhost，确保端口在两边都没有被占用。

### 11.7 文件权限问题

**问题：** 在 `/mnt/c/` 下 `chmod` 不生效。

**解决：** 项目放在 WSL 原生文件系统中（`/home/username/`），或修改 `/etc/wsl.conf`：

```ini
[automount]
options = "metadata,uid=1000,gid=1000,umask=022,fmask=111,dmask=000"
```

### 11.8 WSL 网络代理配置

如果使用代理工具：

```bash
# 在 ~/.bashrc 中添加
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

# 或获取 Windows 主机 IP
export http_proxy=http://$(hostname).local:7890
```

### 11.9 内存与性能问题

**问题：** WSL 占用过多内存。

**解决：** 在 Windows 的 `.wslconfig` 中限制资源：

```ini
[wsl2]
memory=4GB
processors=2
swap=2GB
localhostForwarding=true
```

然后重启 WSL：

```bash
wsl --shutdown
```

---

## 十二、完整环境检查清单

```bash
# WSL 版本
wsl --status

# Node.js 版本
node --version

# npm 版本
npm --version

# 包管理器
yarn --version  # 如果安装了
pnpm --version  # 如果安装了

# 数据库状态
sudo service mysql status
sudo service postgresql status
sudo service mongod status
sudo service redis-server status

# 全局工具
ls $(npm root -g)

# 当前 Node.js 路径
which node
```

配置完成后，WSL 将提供一个完整、高效的 Node.js 开发环境，兼具 Linux 的兼容性和 Windows 的桌面体验。
