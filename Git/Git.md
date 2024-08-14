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

```bash
git reflog
```

