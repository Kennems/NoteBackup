# Shell编程

Shell编程主要是将用户手工操作的单个或者多个Linux指令，按照一定的逻辑关系，顺序堆积在文本文件（脚本）中。最终脚本文件中的Linux指令会以特定的Shell解释器（/bin/bash）去执行，进而满足企业业务的需求。

## 基本语法

### 变量

```bash
# 定义变量（=两边不能有空格）
name="world"
readonly PI=3.14          # 只读变量

# 使用变量
echo "Hello, $name"
echo "Hello, ${name}"     # 推荐加花括号

# 命令替换
now=$(date)
files=`ls`                # 反引号写法（旧式）
```

### 条件判断

```bash
if [ condition ]; then
    # commands
elif [ condition ]; then
    # commands
else
    # commands
fi
```

常用条件测试：

| 文件测试   | 含义         | 数值比较 | 含义       |
| ---------- | ------------ | -------- | ---------- |
| `-f`       | 是否为普通文件 | `-eq`    | 等于       |
| `-d`       | 是否为目录   | `-ne`    | 不等于     |
| `-e`       | 是否存在     | `-gt`    | 大于       |
| `-r` `-w` `-x` | 可读/写/执行 | `-lt` | 小于       |
| `-z "str"` | 字符串为空   | `-ge`    | 大于等于   |
| `-n "str"` | 字符串非空   | `-le`    | 小于等于   |
| `"a" = "b"`| 字符串相等   |          |            |

### 循环

```bash
# for 循环
for i in {1..5}; do
    echo "Number: $i"
done

for file in *.txt; do
    echo "Processing $file"
done

# while 循环
count=1
while [ $count -le 5 ]; do
    echo $count
    count=$((count + 1))
done
```

### 函数

```bash
greet() {
    local name=$1          # local 限定局部变量
    echo "Hello, $name!"
}

greet "Alice"
```

### 特殊变量

| 变量    | 含义                         |
| ------- | ---------------------------- |
| `$0`    | 脚本名称                     |
| `$1-$9` | 第 1-9 个参数                |
| `$#`    | 参数个数                     |
| `$@`    | 所有参数（每个独立）         |
| `$*`    | 所有参数（合并为一个字符串） |
| `$?`    | 上一条命令的退出状态码       |
| `$$`    | 当前 shell 的 PID            |

### 脚本模板

```bash
#!/bin/bash
set -euo pipefail   # 严格模式：遇错退出 | 未定义变量报错 | 管道错误传播

SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"

main() {
    echo "Starting script..."
    # your code here
}

main "$@"
```
