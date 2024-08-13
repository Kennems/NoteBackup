# Namomo Winter Camp 0107开幕式



因为算法而在这里 Camp

狄利克雷卷积 

为什么要学算法？

不要忘记初衷，不仅要仰望星空，也要脚踏实地

为什么要办Camp？

身处沙漠，绍兴一中 2018年开始 让更多人的努力获得回报

相信自己能学会 我可以 你真棒 我能行 熬过自己

codeforces atcoder 学完基础只是持续刷题

责任 自己的责任 对自己负责 同舟共济

因为纯粹 所以简单

心怀热爱 奔赴山海

不要想那么多 只有热爱 才能走的长远 世界冠军为目标

1. Learn the fundamentals. Start by learning the fundamentals of programming, such as variables, data types, functions, and control flow. This will give you the foundation you need to build more complex programs.

2. Practice, practice, practice. The best way to become a good programmer is to practice writing code. Take online courses, read tutorials, and work through coding exercises to hone your skills.

3. Take on real-world projects. Once you've built up your skills, take on real-world projects to apply the knowledge you've gained. You'll learn a lot more by actually building something than you will by just reading about it.

4. Stay up to date. Technology is constantly changing, so it's important to stay up to date on the latest trends and tools. Read blogs, attend conferences, and follow industry leaders to stay ahead of the curve.

5. Be open to feedback. As a programmer, you'll need to be open to feedback from others. Listen to constructive criticism and use it to improve your skills and work.

6. Collaborate with others. Don't be afraid to reach out to other programmers and collaborate on projects. Working with others is a great way to learn new skills and share ideas.

# day1 魔改常用算法

## 魔改bfs

无向图，n个点，m条边，边长为1。求两两之间的最短路。

visited_vertices=bitset 

压位bfs lowbit 

## 魔改bfs-2

无向图，n个点，m条边，边长为1或100000。在a点可以使用魔法到b点，只有一些点可以用魔法。求a到个点之间的最短路。

01bfs  队列改双端队列

## 魔改二分

二分，求x 位置，给一个pos可能正确也可能错误，在（log(答案-pos)+1)时间内找到

 求答案方向，之后倍增，之后再二分（或者从大到小的倍增）

## 魔改二分-2

给定一排零食，重量为w[i]。每个大礼包可以承受T的重量。现在顺序把零食临时装包，只要装不下就会启用下一 包。问装了几包。复杂度O(ans log（n/ans))。

琴生不等式 凸函数 最差为ans次二分

前缀和 倍增

## 魔改二分-3

给定n种不同的鸡蛋和一个n层的楼。要求测试每种鸡蛋最高在几层扔下不会碎。鸡蛋每种有O(log n)个。每走一层花费1的体力。鸡蛋可以背在身上任意多个。要求花费最多O(n log(n))，完成测试。

每个鸡蛋二分 每种鸡蛋O(log n) 体力O(n2)

背着鸡蛋逐层测试 每种鸡蛋所需 O(n) 体力O(n)

分块，先上B层试一下，碎的鸡蛋就在1到B-1层之间，下楼是一遍

![image-20230107141858850](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107141858850.png)

![image-20230107141942585](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107141942585.png)

## 魔改双指针

![image-20230107144557095](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107144557095.png)

![image-20230107144402826](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107144402826.png)

## 魔改kmp

给定长度为n的数组a和1到m的排列b。问a中是否有长度为m的一段，数字间大小关系和b中一样。

## 计算几何 Geometry板子

### 实数比较

![image-20230107145807710](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107145807710.png)

### 点

![image-20230107145940808](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107145940808.png)

### 其他函数

![image-20230107150357385](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107150357385.png)

### 点积和差积

![image-20230107150837876](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107150837876.png)

### 点积的性质：

做功 方向性和投影长度 三维

### 叉积的性质：

面积 绕向 判定平行

### 如何极角排序？。。

比较函数：分为上下两部分之后分别用叉积比较大小

![image-20230107152211157](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107152211157.png)

### 旋转

复数或矩阵

![image-20230107153044319](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230107153044319.png)

### crossop。。



![image-20230107153949454](D:\文档\文本\学习笔记\typora_imag\image-20230107153949454.png)

直线平行

![image-20230107154422096](D:\文档\文本\学习笔记\typora_imag\image-20230107154422096.png)

### 直线交点

![image-20230107154442638](D:\文档\文本\学习笔记\typora_imag\image-20230107154442638.png)

![image-20230107154627145](D:\文档\文本\学习笔记\typora_imag\image-20230107154627145.png)

### 线段相交

![image-20230107155311940](D:\文档\文本\学习笔记\typora_imag\image-20230107155311940.png)

### WF2017 A

n个点的简单多边形，问能放进去的最长线段长度。n<=200

# day2 陈靖邦

![image-20230108130958083](D:\文档\文本\学习笔记\typora_imag\image-20230108130958083.png)

## 1

![image-20230108131457498](D:\文档\文本\学习笔记\typora_imag\image-20230108131457498.png)

![image-20230108131933415](D:\文档\文本\学习笔记\typora_imag\image-20230108131933415.png)

## 2济南A

## 3

![image-20230108132806436](D:\文档\文本\学习笔记\typora_imag\image-20230108132806436.png)

![image-20230108133339328](D:\文档\文本\学习笔记\typora_imag\image-20230108133339328.png)

![image-20230108133501837](D:\文档\文本\学习笔记\typora_imag\image-20230108133501837.png)



## 4![image-20230108133739144](D:\文档\文本\学习笔记\typora_imag\image-20230108133739144.png)

![image-20230108134222387](D:\文档\文本\学习笔记\typora_imag\image-20230108134222387.png)

![image-20230108134707207](D:\文档\文本\学习笔记\typora_imag\image-20230108134707207.png)



## 5![image-20230108135013526](D:\文档\文本\学习笔记\typora_imag\image-20230108135013526.png)

![image-20230108135538999](D:\文档\文本\学习笔记\typora_imag\image-20230108135538999.png)

![image-20230108135819203](D:\文档\文本\学习笔记\typora_imag\image-20230108135819203.png)

![image-20230108135950103](D:\文档\文本\学习笔记\typora_imag\image-20230108135950103.png)

![image-20230108140222261](D:\文档\文本\学习笔记\typora_imag\image-20230108140222261.png)



## 6![image-20230108141026976](D:\文档\文本\学习笔记\typora_imag\image-20230108141026976.png)

![image-20230108141944079](D:\文档\文本\学习笔记\typora_imag\image-20230108141944079.png)

## 7

## 8

![image-20230108144030547](D:\文档\文本\学习笔记\typora_imag\image-20230108144030547.png)

![image-20230108144746204](D:\文档\文本\学习笔记\typora_imag\image-20230108144746204.png)

![image-20230108145052894](D:\文档\文本\学习笔记\typora_imag\image-20230108145052894.png)

## 9

树hash

杭州：图中所有生成树是否同构。

![image-20230108150336754](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230108150336754.png)

## 10

![image-20230108150846437](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230108150846437.png)

![image-20230108151407326](D:\文档\文本\学习笔记\typora_imag\image-20230108151407326.png)

## 总结

1、过200人的题，有脑子（逆向思维，贪心）

2、100人的题，细心，能写代码（分裂讨论

3、100以下的题，懂得分析模型（DP，经典算法理解）

4、50人的题，懂算法（线段树，树hash,单调队列）

出题过程：

![image-20230108152428403](D:\文档\文本\学习笔记\typora_imag\image-20230108152428403.png)

![image-20230108153459275](D:\文档\文本\学习笔记\typora_imag\image-20230108153459275.png)

![image-20230108154418510](D:\文档\文本\学习笔记\typora_imag\image-20230108154418510.png)

![image-20230108154918602](D:\文档\文本\学习笔记\typora_imag\image-20230108154918602.png)

![image-20230108155246626](D:\文档\文本\学习笔记\typora_imag\image-20230108155246626.png)

![image-20230108160310045](D:\文档\文本\学习笔记\typora_imag\image-20230108160310045.png)

![image-20230108160807784](D:\文档\文本\学习笔记\typora_imag\image-20230108160807784.png)

amc  project euler 

脚踏实地，不要自卑也不要自负

**陈立杰：**自己选择的路跪着也要走完  不是每个人都要做TCS  数学

要站在巨人的肩膀上为计算机添砖加瓦 ftiasch 他山之石可以功玉

# day3 dp wls

## 1

1到n排列，所有排列中逆序对恰好为k的数量。

```c++
f[i][j]表示1..i的排列，逆序对恰好为j的方案数。
依次考虑放进i+1,f[i+1][j]=
```

![image-20230109131434329](D:\文档\文本\学习笔记\typora_imag\image-20230109131434329.png)

![image-20230109132227269](D:\文档\文本\学习笔记\typora_imag\image-20230109132227269.png)

## 2

1到n排列，所有排列中相邻数前面大于后面恰好为k对的数量。

无后效性。最优子结构。

![image-20230109133242448](D:\文档\文本\学习笔记\typora_imag\image-20230109133242448.png)

## 3

![image-20230109134650721](D:\文档\文本\学习笔记\typora_imag\image-20230109134650721.png)

![image-20230109135239930](D:\文档\文本\学习笔记\typora_imag\image-20230109135239930.png)

高维前缀和 sosdp

## 4

1到n排列，所有排列中相邻数前面大于后面恰好为k对的数量。 同2

![image-20230109141546538](D:\文档\文本\学习笔记\typora_imag\image-20230109141546538.png)

## 5

![image-20230109142307975](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230109142307975.png)

![image-20230109143929628](D:\文档\文本\学习笔记\typora_imag\image-20230109143929628.png)

## 6

![image-20230109144554936](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230109144554936.png)

![image-20230109145332472](D:\文档\文本\学习笔记\typora_imag\image-20230109145332472.png)

## 7

![image-20230109152449444](D:\文档\文本\学习笔记\typora_imag\image-20230109152449444.png)

![image-20230109153249501](D:\文档\文本\学习笔记\typora_imag\image-20230109153249501.png)

## 8

![image-20230109154314526](D:\文档\文本\学习笔记\typora_imag\image-20230109154314526.png)

## 9

![image-20230109155400916](D:\文档\文本\学习笔记\typora_imag\image-20230109155400916.png)

![image-20230109160816137](D:\文档\文本\学习笔记\typora_imag\image-20230109160816137.png)

## 10

![image-20230109161321190](D:\文档\文本\学习笔记\typora_imag\image-20230109161321190.png)



# day5 图论 dls

1 遍历（bfs/dfs)

2 最短路/最小生成树

3 拓扑排序

4 网络流/匹配

5 强连通/双连通

## 1

给出度数构造树和简单图：给出每个点的度数，是否能构造出一个树或者简单图

按度数从大到小：

![image-20230111183713186](D:\文档\文本\学习笔记\typora_imag\image-20230111183713186.png)

Havel-Hakim定理

erdox-Gallai theorem

## 2 二分图

构造简单图

# day6 数学知识 吴航

## 二项式定理：



## 中国剩余定理：



## 原根：![image-20230112132237077](D:\文档\文本\学习笔记\typora_imag\image-20230112132237077.png)

![image-20230112132436490](D:\文档\文本\学习笔记\typora_imag\image-20230112132436490.png)

统一底数

剩余系：

![image-20230112134248718](D:\文档\文本\学习笔记\typora_imag\image-20230112134248718.png)

![image-20230112134512888](D:\文档\文本\学习笔记\typora_imag\image-20230112134512888.png)

求x和y

## 线性筛：

![image-20230112140417877](D:\文档\文本\学习笔记\typora_imag\image-20230112140417877.png)

![image-20230112140951864](D:\文档\文本\学习笔记\typora_imag\image-20230112140951864.png)

## 马尔可夫链：

![image-20230112142929050](D:\文档\文本\学习笔记\typora_imag\image-20230112142929050.png)

![image-20230112143319129](D:\文档\文本\学习笔记\typora_imag\image-20230112143319129.png)



![image-20230112150252814](D:\文档\文本\学习笔记\typora_imag\image-20230112150252814.png)

![image-20230112145723183](D:\文档\文本\学习笔记\typora_imag\image-20230112145723183.png)

## 生成函数：



常系数线性递推：Fibonacci  



![image-20230112151304562](D:\文档\文本\学习笔记\typora_imag\image-20230112151304562.png)

## 伪蒙特卡洛(Quasi-monte carlo QMC) 

![image-20230112154614366](D:\文档\文本\学习笔记\typora_imag\image-20230112154614366.png)

![image-20230112154722195](D:\文档\文本\学习笔记\typora_imag\image-20230112154722195.png)

![image-20230112154118018](D:\文档\文本\学习笔记\typora_imag\image-20230112154118018.png)



## 傅里叶 fft 斯特林数 组合数 丢番图

![image-20230112154419629](D:\文档\文本\学习笔记\typora_imag\image-20230112154419629.png)

欧拉计划



# day7 数据结构 清华大学nzhtl1477

## luogu5463

树状数组

## luogu5482

离散化

## luogu1637

（线段树二分）

## luoguT22526



## luogu1115 最大子段和



## luogu1168

对顶堆

## luogu7706

## codechef DGCD

## luogu4344

## luogu4036

