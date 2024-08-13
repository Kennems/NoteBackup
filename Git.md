# Git

```
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
```

Linux登录GitHub并push到仓库

`http`方式

1. VPN开启TUN模式
2. 获取GitHub`token`, 在push的时候选择用户名和`token`而不是密码

`ssh`方式

- 先生成`ssh_rsa`

- ```
  ssh-keygen
  ```

- 然后添加ssh

```bash
git remote add origin git@github.com:showguan/test.git
```

然后直接`push`即可

## Git Bash设置home目录

```bash
setx HOME "C:\Projects"
```

