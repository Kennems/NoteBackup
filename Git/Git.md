# Git

git 由 linus 开发。

## Git流程图

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

1. clone（克隆)）： 从远程仓库中克隆代码到本地仓库
2. checkout（检出）：从本地仓库中检出一个仓库分支然后进行修订
3. add（添加）：在提交前先将代码提交到暂存区
4. commit（提交）：提交到本地仓库，本地仓库中保存修改的各个历史版本
5. fetch（抓取）：从远程库，抓取到本地仓库，不进行任何的合并动作，一般操作比较少。
6. pull（拉取）：从远程库拉到本地库，自动进行合并（merge），然后放到工作区，相当于fetch + merge
7. push（推送）：修改完成后，需要和团队成员共享代码时，将代码推送到远程仓库

## Git基本配置

```bash
git config --global user.name "xxx"
git config --global user.email "xxx@gmail.com"
```

```bash
git config --global user.name
git config --global user.email
```

给一些长命令起别名：

在 ~/.bashrc 中添加：

```bash
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'
alias ll='ls -al'
```

之后 source ~/.bashrc 使其生效。

### 版本切换：

![image-20240814114817965](https://cdn.jsdelivr.net/gh/kennems/blog-image/image-20240814114817965.png)

```bash
git reset --hard commitId
```

查看所有日志：

```bash
git reflog
```

## 分支

查看本地分支

```bash
git branch
```

创建本地分支

```bash
git branch 分支名
```

切换分支

```bash
git checkout 分支名
```

切换并创建分支

```bash
git checkout -b 分支名
```

### 合并分支

```bash
git merge
```

### 删除分支

删除分支时，需要做各种检查

```bash
git branch -d b1 
```

不做任何检查，强制删除

```bash
git branch -D b1
```

## 解决冲突

```bash
git merge 需要合并的工作区
```

### 远程仓库

```bash
git push -f --set-upstream [远端名称[本地分支名][:远端分支名]]
```

```bash
git remote add origin 源
```

### 设置本地分支与远端分支绑定

```bash
git push --set-upstream origin main
```

设置好之后，直接

```bash
git push 
```

##  克隆

```bash
git clone 地址
```

## 从远程仓库抓取和拉取

抓取，将仓库里的更新都抓取到本地，但不会进行合并

```bash
git fetch [remote name] [branch name]
```

拉取，就是将远端仓库的修改拉到本地并自动进行合并，等同于 fetch + merge

```bash
git pull [remote name] [branch name]
```

## 解决合并冲突



