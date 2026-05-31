---
title : '操作系统 (Operating System)'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-05-04T12:54:39+08:00
description : "操作系统是计算机硬件和应用之间的一层软件，核心目标是**方便用户使用硬件**并**高效地管理硬件资源**。"
image : img/cat.jpg
draft : false
categories : ["计算机基础"]
tags : ["学习笔记", "计算机基础", "操作系统"]
---
# 操作系统 (Operating System)

操作系统是计算机硬件和应用之间的一层软件，核心目标是**方便用户使用硬件**并**高效地管理硬件资源**。

## 一、进程管理

### 1.1 进程与线程

| 概念 | 定义 |
|------|------|
| **进程** | 程序的一次执行实例，拥有独立的地址空间和资源 |
| **线程** | CPU调度的基本单位，共享所属进程的地址空间 |
| **协程** | 用户态轻量级线程，由程序自身管理调度 |

进程 vs 线程：
- 进程是**资源分配**的基本单位，线程是**CPU调度**的基本单位
- 进程间隔离性强（独立地址空间），线程间共享数据方便
- 进程切换开销大，线程切换开销小

### 1.2 进程控制块 (PCB)

PCB是操作系统管理进程用的核心数据结构，保存在内核中：

```c
struct task_struct {
    pid_t pid;                     // 进程唯一标识符
    long state;                    // 进程状态
    struct mm_struct *mm;          // 内存空间信息
    struct files_struct *files;    // 打开的文件描述符表
    struct thread_struct thread;   // CPU上下文（寄存器等）
    struct list_head tasks;        // 链表指针
    unsigned int priority;         // 调度优先级
    unsigned long stack;           // 内核栈指针
};
```

Linux中PCB为`task_struct`，通过链表串联所有进程；`current`宏指向当前运行的进程PCB。

### 1.3 进程五状态模型

```
    ┌──────────┐
    │   New    │
    └────┬─────┘
         │ admit
         ↓
    ┌──────────┐     dispatch     ┌──────────┐
    │  Ready   │ ───────────────→ │ Running  │
    └────┬─────┘                  └────┬─────┘
         │ ↑                          │
         │ │  preempt                  │ I/O or event wait
         │ │                          ↓
         │ │                    ┌──────────┐
         │ └────────────────────│  Wait    │ ←── I/O completion
         │                     └──────────┘
         │
         ↓
    ┌──────────┐
    │ Terminated│
    └──────────┘
```

- **就绪态**：已具备运行条件，等待CPU
- **运行态**：占用CPU执行指令
- **阻塞态**：等待I/O或某个事件

### 1.4 CPU调度算法

| 算法 | 原则 | 优点 | 缺点 |
|------|------|------|------|
| **FCFS** | 先来先服务 | 公平、简单 | 平均等待时间长（convoy effect） |
| **SJF** | 最短作业优先 | 最小平均等待时间 | 需要预知运行时间，长任务可能饿死 |
| **RR** | 时间片轮转 | 响应时间短，交互性好 | 时间片大小影响性能 |
| **优先级调度** | 按优先级调度 | 支持紧急任务 | 低优先级可能饿死 |
| **多级反馈队列** | 多队列+动态反馈 | 综合性能好 | 实现复杂 |

**时间片选择**：
- 时间片太大 → 退化为FCFS
- 时间片太小 → 上下文切换开销过大
- 典型值：10-100ms

**多级反馈队列**（MLFQ）核心思想：
1. 多个就绪队列，优先级从高到低
2. 高优先级队列时间片短，低优先级队列时间片长
3. 新进程放入最高优先级队列
4. 用完时间片仍未完成则降级到下一队列
5. 定期将进程提升到更高优先级（防止饿死）

## 二、内存管理

### 2.1 内存管理的核心问题

- **地址转换**：逻辑地址 → 物理地址
- **内存分配**：连续分配 vs 非连续分配
- **内存保护**：进程间地址空间隔离
- **虚拟内存**：扩大可用内存空间

### 2.2 分页 (Paging)

- 物理内存划分为固定大小的**页框/帧**（frame），典型大小4KB
- 进程逻辑地址空间也划分为同样大小的**页**（page）
- 通过**页表**将逻辑页号映射到物理帧号

```
逻辑地址 = 页号 + 页内偏移
物理地址 = 帧号 + 页内偏移
```

**页表项结构**：

```c
struct page_table_entry {
    unsigned int present    : 1;  // 是否在内存中
    unsigned int writable   : 1;  // 是否可写
    unsigned int user       : 1;  // 用户态可访问
    unsigned int accessed   : 1;  // 是否被访问（用于页面置换）
    unsigned int dirty      : 1;  // 页面是否被修改
    unsigned int frame      : 20; // 物理帧号
};
```

**多级页表**：64位系统中一级页表过大，采用多级页表节省内存（只创建实际使用的页表项）。

### 2.3 分段 (Segmentation)

- 按逻辑段划分（代码段、数据段、堆栈等）
- 段号 + 段内偏移 → 物理地址
- **段表**：段基址 + 段界限
- 优点：便于共享和保护，符合用户视角

### 2.4 虚拟内存

基于**局部性原理**（时间局部性、空间局部性），只将当前需要的部分页面装入内存：

- **按需调页**：运行进程时只调入必需的页面
- **页面置换**：内存不足时淘汰某些页

### 2.5 页面置换算法

| 算法 | 策略 | 评价 |
|------|------|------|
| **FIFO** | 淘汰最先进入的页 | 实现简单，可能淘汰常用页（Belady异常） |
| **LRU** | 淘汰最久未使用的页 | 性能好，硬件开销大 |
| **Clock/NRU** | 近似LRU，使用引用位 | 常用折中方案 |
| **LFU** | 淘汰使用频率最低的页 | 维护计数开销高 |
| **OPT** | 淘汰未来最远使用的页 | 理论最优，不可实现 |

**LRU近似实现**（Clock算法）：
```
1. 所有页框形成循环链表
2. 每个页框有引用位 R
3. 指针循环扫描，遇到 R=1 则置为 0 继续
4. 遇到 R=0 则淘汰该页
```

**Belady异常**：FIFO算法下，增加页框数反而导致缺页率上升。

### 2.6 分页 vs 分段

| 维度 | 分页 | 分段 |
|------|------|------|
| 出发点 | 提高内存利用率 | 满足用户逻辑视角 |
| 大小 | 固定（由硬件决定） | 可变（由程序决定） |
| 共享 | 地址空间共享困难 | 逻辑段共享方便 |

现代操作系统通常使用**段页式**：段中分页，兼具二者优点。

## 三、文件系统

### 3.1 文件与inode

**inode**（索引节点）是文件系统中最核心的数据结构，存储除文件名外的所有元数据：

```c
struct inode {
    unsigned long  i_ino;          // inode号
    umode_t        i_mode;         // 文件类型 + 权限
    uid_t          i_uid;          // 所有者
    gid_t          i_gid;          // 所属组
    loff_t         i_size;         // 文件大小
    struct timespec i_atime;       // 最后访问时间
    struct timespec i_mtime;       // 最后修改时间
    struct timespec i_ctime;       // 元数据变更时间
    unsigned int   i_blocks;       // 占用的块数
    union {
        struct {                   // 普通文件
            struct address_space *i_mapping;
        };
        struct {                   // 目录文件
            struct dentry *i_dentry;
        };
    };
};
```

- 文件名存储在目录项（dentry）中
- 硬链接：多个文件名指向同一个inode
- 软链接：保存目标路径的独立文件

### 3.2 目录结构

目录本身是一个文件，内容为文件名到inode号的映射表：

```
dentry {
    inode号 (4B) | 文件名 (变长)
       1234      |   .bashrc
       5678      |   .profile
       ...       |   ...
}
```

### 3.3 文件系统类型

| 类型 | 特点 | 适用场景 |
|------|------|----------|
| **ext4** | 日志文件系统，支持大文件 | Linux默认 |
| **NTFS** | 日志+权限+压缩加密 | Windows |
| **FAT32** | 兼容性好，单文件<4GB | U盘 |
| **APFS** | 快照+克隆+加密 | macOS |

### 3.4 文件存储实现

**连续分配**：一个文件占据连续的磁盘块（FAT），简单但碎片化。
**索引分配**：每个文件有一个索引块，指向数据块：
```
inode → 直接块指针[0-11] → 数据块
       → 间接块指针 → 块指针[] → 数据块
       → 二级间接 → ...
```
- 直接块：小文件快速访问
- 间接块：支持大文件

### 3.5 虚拟文件系统 (VFS)

VFS提供统一抽象层，使得用户通过统一接口操作不同文件系统：

```
用户进程: open() / read() / write()
     ↓
   VFS层: vfs_open() / vfs_read() / vfs_write()
     ↓
具体文件系统: ext4_read() / ntfs_read() / fat_read()
     ↓
  块设备层: submit_bio()
     ↓
   设备驱动
```

VFS核心对象：
- `super_block`：已挂载文件系统的超级块信息
- `inode`：文件元数据抽象
- `dentry`：目录项缓存，加速路径解析
- `file`：已打开文件的抽象

### 3.6 挂载 (Mount)

挂载将文件系统附着到目录树：

```
$ mount /dev/sda1 /mnt/data
```

- 根文件系统 `/` 在启动时挂载
- 挂载点必须是一个已存在的目录

## 四、I/O管理

### 4.1 I/O设备分类

| 分类 | 特点 | 例子 |
|------|------|------|
| **块设备** | 以块为单位，可随机访问 | 磁盘、SSD |
| **字符设备** | 以字节流为单位，顺序访问 | 键盘、串口 |
| **网络设备** | 面向数据包 | 网卡 |

### 4.2 I/O控制方式

#### 程序循环查询 (Polling)
- CPU不断检测设备状态寄存器
- 简单但浪费CPU

#### 中断驱动 (Interrupt)
```
1. 进程发起I/O请求
2. CPU继续执行其他任务
3. 设备完成I/O后发中断信号
4. CPU暂停当前进程，执行中断处理程序
5. 恢复被中断的进程
```
- 适合低速I/O设备

#### DMA (直接存储器访问)
DMA控制器直接在设备和内存之间传输数据，不经过CPU：

```
1. CPU设置DMA：源地址、目标地址、传输字节数
2. CPU返回执行其他任务
3. DMA控制器独立完成数据传输
4. DMA传输完成后发中断通知CPU
```

- 适合高速I/O（磁盘、网卡等块传输）
- 解放CPU，实现I/O与计算并行

### 4.3 缓冲技术

| 缓冲类型 | 作用 |
|----------|------|
| **单缓冲** | 内核缓冲区，预处理输入 |
| **双缓冲** | 交替使用两个缓冲区，提高吞吐量 |
| **循环缓冲** | 多个缓冲区形成环形队列 |
| **缓冲池** | 动态管理多个缓冲区 |

**缓冲区溢出**：用户态进程通过`read()`/`write()`系统调用与内核缓冲区交互，再由内核异步与设备交互。

### 4.4 SPOOLing 技术

利用磁盘作为缓冲，将独占设备转化为共享设备：
- 输入井 + 输出井
- 典型应用：打印机

### 4.5 I/O软件层次

```
用户进程 (I/O请求)
    ↓
设备无关的I/O层 (缓冲、命名、保护)
    ↓
设备驱动层 (与硬件交互、中断处理)
    ↓
中断处理程序 (响应设备中断)
    ↓
硬件设备
```

## 五、死锁

### 5.1 死锁条件

满足以下**四个条件**同时成立时才会发生死锁：

| 条件 | 解释 |
|------|------|
| **互斥** | 资源一次只能被一个进程占用 |
| **持有并等待** | 进程持有资源的同时等待其他资源 |
| **不可剥夺** | 资源不能被强制剥夺 |
| **循环等待** | 多个进程形成等待环路 |

### 5.2 死锁处理方法

#### 预防 (Prevention)
打破四个条件中的至少一个：

| 方法 | 打破的条件 | 代价 |
|------|-----------|------|
| 共享资源而非互斥 | 互斥 | 部分资源无法共享 |
| 一次性申请所有资源 | 持有并等待 | 资源利用率低 |
| 可剥夺资源 | 不可剥夺 | 实现复杂 |
| 资源有序分配 | 循环等待 | 编号复杂 |

#### 避免 (Avoidance)
**银行家算法**（Dijkstra）：
- 进程在申请资源时，系统计算分配后是否处于安全状态
- 安全状态：存在一个进程执行序列，使得所有进程都能完成

```c
bool is_safe(int available[], int max[][], int alloc[][]) {
    int work[] = available;
    bool finish[] = {false};

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (!finish[j] && need[j] <= work) {
                work += alloc[j];
                finish[j] = true;
                break;
            }
        }
    }
    return all(finish);
}
```

#### 检测 (Detection)
- 构建资源分配图
- 检测图中是否存在循环等待（环路）
- 检测算法类似银行家算法但不考虑max需求

#### 恢复 (Recovery)
- **进程终止**：终止所有死锁进程 / 逐个终止直到解除
- **资源剥夺**：选择性回滚，从检查点重启

## 六、进程同步

### 6.1 竞争条件

多个进程/线程访问共享数据，最终结果取决于访问的精确时序：
- 需要**同步机制**确保数据一致性

### 6.2 临界区 (Critical Section)

**互斥访问**三条原则：
1. **互斥**：同一时刻最多一个进程在临界区内
2. **前进**：无进程在临界区时，必须允许一个请求者进入
3. **有限等待**：进程不能无限等待进入临界区

### 6.3 信号量 (Semaphore)

由Dijkstra提出，通过**P（wait）**和**V（signal）**两个原子操作实现同步：

```c
struct semaphore {
    int count;           // 资源计数
    queue waiting_queue; // 阻塞队列
};

void sem_wait(semaphore *s) {
    s->count--;
    if (s->count < 0) {
        // 将当前进程加入等待队列并阻塞
        block(s->waiting_queue);
    }
}

void sem_signal(semaphore *s) {
    s->count++;
    if (s->count <= 0) {
        // 唤醒等待队列中的一个进程
        wakeup(s->waiting_queue);
    }
}
```

- **二值信号量**：count取值0/1，相当于互斥锁
- **计数信号量**：count > 1，管理多个同类资源

### 6.4 互斥锁 (Mutex)

信号量的简化版本，只有locked/unlocked两种状态：
```c
pthread_mutex_lock(&mutex);   // 加锁
// 临界区
pthread_mutex_unlock(&mutex); // 解锁
```

### 6.5 管程 (Monitor)

高级同步原语，封装共享变量和操作函数：
- 同一时刻只有一个线程能进入管程
- 条件变量：`wait()` 和 `signal()` 实现条件同步

```c
// 伪代码：管程实现生产者消费者
monitor ProducerConsumer {
    int buffer[N];
    int count = 0, in = 0, out = 0;
    condition not_full, not_empty;

    void produce(int item) {
        if (count == N) wait(not_full);
        buffer[in] = item;
        in = (in + 1) % N;
        count++;
        signal(not_empty);
    }

    int consume() {
        if (count == 0) wait(not_empty);
        int item = buffer[out];
        out = (out + 1) % N;
        count--;
        signal(not_full);
        return item;
    }
};
```

### 6.6 经典同步问题

#### 生产者-消费者 (Bounded Buffer)
生产者写入缓冲区，消费者读取缓冲区，需互斥访问：
```c
semaphore mutex = 1;   // 互斥访问缓冲区
semaphore empty = N;   // 空槽位数
semaphore full  = 0;   // 已填充槽位数

// 生产者
void producer() {
    while (1) {
        item = produce();
        sem_wait(&empty);   // 等待空位
        sem_wait(&mutex);   // 互斥访问
        buffer[in] = item;
        in = (in + 1) % N;
        sem_signal(&mutex);
        sem_signal(&full);  // 增加已填计数
    }
}

// 消费者
void consumer() {
    while (1) {
        sem_wait(&full);    // 等待数据
        sem_wait(&mutex);   // 互斥访问
        item = buffer[out];
        out = (out + 1) % N;
        sem_signal(&mutex);
        sem_signal(&empty); // 增加空位
        consume(item);
    }
}
```

#### 读者-写者问题
- 多个读者可以同时读
- 写者必须独占访问

```c
semaphore rw_mutex = 1;  // 写者互斥
semaphore mutex = 1;     // 保护read_count
int read_count = 0;

// 读者
void reader() {
    sem_wait(&mutex);
    read_count++;
    if (read_count == 1) sem_wait(&rw_mutex); // 第一个读者阻止写者
    sem_signal(&mutex);
    // 读取数据
    sem_wait(&mutex);
    read_count--;
    if (read_count == 0) sem_signal(&rw_mutex); // 最后一个读者释放
    sem_signal(&mutex);
}

// 写者
void writer() {
    sem_wait(&rw_mutex);
    // 写入数据
    sem_signal(&rw_mutex);
}
```

**写者优先**变体：引入额外信号量防止写者饿死。

#### 哲学家就餐问题
五位哲学家共享五根筷子，需避免死锁：

解决方案：
1. 最多允许4位哲学家同时进餐
2. 奇数哲学家先取左筷子，偶数先取右筷子（破坏循环等待）
3. 使用管程封装筷子分配

```c
#define N 5
semaphore chopstick[N]; // 每根筷子是一个信号量

void philosopher(int i) {
    while (1) {
        think();
        if (i % 2 == 0) {          // 偶数先左后右
            sem_wait(&chopstick[i]);
            sem_wait(&chopstick[(i+1)%N]);
        } else {                   // 奇数先右后左
            sem_wait(&chopstick[(i+1)%N]);
            sem_wait(&chopstick[i]);
        }
        eat();
        sem_signal(&chopstick[i]);
        sem_signal(&chopstick[(i+1)%N]);
    }
}
```

## 七、补充知识点

### 7.1 常见系统调用

| 系统调用 | 功能 |
|----------|------|
| `fork()` | 创建子进程 |
| `exec()` | 替换进程映像 |
| `wait()` | 等待子进程结束 |
| `exit()` | 终止进程 |
| `brk()` / `mmap()` | 内存分配 |
| `open()` / `read()` / `write()` | 文件I/O |
| `ioctl()` | 设备控制 |
| `sched_yield()` | 主动让出CPU |

### 7.2 上下文切换开销

- 保存/恢复寄存器
- TLB刷新（进程切换时）
- 高速缓存缺失（cache miss）
- 调度器代码执行

### 7.3 饥饿 (Starvation)

进程无限期等待资源，但不同于死锁（死锁中进程均被阻塞，饥饿中某些进程仍在推进）：
- SJF中长任务饥饿
- 优先级调度中低优先级任务饥饿
- 读者-写者中写者饥饿

### 7.4 原子操作

硬件层面保证的不可中断操作：
- **Test-and-Set**（TSL指令）
- **Compare-and-Swap**（CAS）
- **Load-Linked / Store-Conditional**（LL/SC）

```c
// Test-and-Set 实现互斥锁
int test_and_set(int *lock) {
    int old = *lock;
    *lock = 1;      // 原子操作
    return old;
}

void lock(volatile int *lock) {
    while (test_and_set(lock) == 1);  // 自旋等待
}

void unlock(volatile int *lock) {
    *lock = 0;
}
```
