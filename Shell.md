# Shell

Ctrl+Alt+T 打开终端

一些常用的终端快捷键:

Ctrl+L 清空屏幕(功能相当于命令clear)

Ctrl+U 剪切文本直到行的起始(可以用于清空行)

Ctrl+K 剪切文本直到行的末尾

Ctrl+Y 粘贴最近剪切的文本

Ctrl+C 杀死当前进程(也可以用来清空当前行)

Ctrl+D 退出当前Shell(功能相当于命令exit) 或者 删除当前的字符

Ctrl+A 行首

Ctrl+E 行尾

Home/End 行首/行尾

Ctrl+F 向前移动一个字符

Ctrl+B 向后移动一个字符

**Ctrl+P 或 Ctrl+N 上下历史记录**,上下方向键 上下历史记录

Ctrl+Shift+C 复制

Ctrl+Shift+V 粘贴

还有Tab补全,按住Ctrl键进行块选择.

鼠标中键:粘贴(在gnome-terminal中使用"菜单键+P"也是可以粘贴的)

# Vim

**单词之间最常使用的命令：**

- w/W 移动到下一个word/WORD 开头。e/E移动到下一个word/WORD

结尾

- b/B回到上一个word/WORD开头，可以理解为backword
- word指的是以非空白符分割的单词，WORD以空白符分割的单词

**行间搜索移动：**

- f{char} 移动到chat字符上，t移动到char的前一个字符
- 如果第一次没搜到，可以用分号(;)和逗号(,) 继续搜该行下一个/上一个
- 大写F表示反过来搜前面的字符

**页面移动：**

- gg/G 移动到文件开头和结尾，可以使用ctrl + o 快速返回
- H/M/L跳转到屏幕的开头（Head)，中间（Middle）和结尾（Lower）
- Ctrl+u， CTRL + f 上下翻页（upward/forward），zz把屏幕置为中间

gi : 上次编辑的位置

ctrl + c 或者ctrl + [代替Esc

数字+命令表示多次执行命令

**删除：**

x/d

dwa包含单词后面的空格

diw不包含单词后面的空格

dt + {char} delete until the char， 一直删到符号前面

d0/d$

配合数字使用

**修改：**

r(replace), c(change), s(substitute)

**查询：**

- 使用/或者？进行前向或者反向搜索
- 使用n/N跳转到下一个或者上一个匹配
- 使用*或者#进行当前单词的前向和后向匹配

**替换命令**

- [range] s[ubstitute] /{pattern}/{string}/[flags]

  支持正则表达式

  range

  - 10，20表示10-20行，%表示全部

  Flags

  - g（global）表示全局范围内执行
  - c（confirm）表示确认，可以确认或者拒绝修改
  - n（number）报告匹配到的次数而不替换，可以用来匹配次数

**多文件**

Buffer Window Tab

- Buffer是指打开的一个文件的内存缓冲区
  - :ls列举当前的缓冲区，用 ： b n跳转到第n个缓冲区
  - :bpre :bnext :bfirst :blast
  - b buffer_name 加上tab补全来跳转
- window窗口时buffer可视化的分割区域
  - 分割 ：**sp**水平分割，**vs**垂直分割
  - ctrl w+w 或者hjkl 移动
- Tab可以组织窗口为一个工作区
  - tabnew 文件名
  - {N}gt切换到编号为{N}的标签页
  - gt切换到下一标签页
  - gT切换到上一标签页



