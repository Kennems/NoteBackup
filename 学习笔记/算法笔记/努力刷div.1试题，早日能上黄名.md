---
title : '努力刷div.1试题，早日能上黄名'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "**题目链接：** <https://codeforces.com/problemset/problem/1753/A1>"
image : img/cat.jpg
draft : false
categories : ["算法笔记"]
tags : ["学习笔记", "算法笔记"]
---
# 努力刷div.1试题，早日能上黄名

## 20221203 [Codeforces Round #829 (Div. 1)](https://codeforces.com/contest/1753)

### A1. Make Nonzero Sum (easy version)

**题目链接：** <https://codeforces.com/problemset/problem/1753/A1>

**难度：** \*1300（ constructive algorithms, dp, greedy ）

#### 题意简述

给定一个长度为 $n$ 的数组 $a$，$a_i \in \{-1, 1\}$（easy version 不含 0）。

要求将数组划分成若干个连续段 $[l_1, r_1], [l_2, r_2], \dots, [l_k, r_k]$，满足：

- 划分覆盖整个数组：$l_1 = 1$, $r_k = n$, $r_i + 1 = l_{i+1}$
- 定义第 $i$ 段的**交错和** $s_i = a_{l_i} - a_{l_i+1} + a_{l_i+2} - a_{l_i+3} + \dots \pm a_{r_i}$
- 要求所有段的交错和之和为 0：$\sum_{i=1}^k s_i = 0$

输出任意一种合法划分，若无解输出 $-1$。

#### 问题分析

设每个元素 $a_i$ 在最终表达式中的系数为 $c_i \in \{+1, -1\}$。从划分的定义可以推导出 $c_i$ 需要满足的性质：

1. **$c_1 = +1$**：第一段第一个元素永远是正号。
2. **不存在两个相邻的 $-1$**：因为在同一段内系数交替 $+1 \to -1 \to +1 \to \dots$，而新的一段总是从 $+1$ 开始，所以 $-1$ 后面一定是 $+1$，不可能出现 $-1$ 后面紧跟 $-1$。
3. **$c_{i} = -c_{i-1}$ 表示 $i$ 和 $i-1$ 在同一段**，**$c_{i} = +1$ 且 $c_{i-1}=+1$ 表示 $i$ 是新段的开始**（此时 $i-1$ 是上一段的结尾）。

因此问题转化为：构造 $c_i \in \{+1,-1\}$ 满足上述约束，使得 $\sum c_i a_i = 0$。

#### 解法思路（配对法）

考虑一个等价但更直观的模型——**单元素段 + 两元素段构造**：

- 长度为 1 的段 $[i,i]$：贡献为 $a_i$（相当于 $c_i = +1$）
- 长度为 2 的段 $[i,i+1]$：贡献为 $a_i - a_{i+1}$（相当于 $c_i = +1, c_{i+1} = -1$）

什么意思呢？如果我们把所有元素都看成单元素段，总和为 $S = \sum a_i$。如果我们将相邻的 $i$ 和 $i+1$ 合并成一个两元素段，那么贡献从 $a_i + a_{i+1}$ 变为 $a_i - a_{i+1}$，**总和的变化量为 $-2a_{i+1}$**。

于是核心思想浮出水面：

> 先假定所有元素都是单元素段（总和为 $S$），然后选择一些不相邻的位置 $i$ 作为「两元素段的第二个元素」，每选一个就将总和减少 $2a_i$，最终使总和归零。

**无解条件：** $S$ 是奇数。因为每次操作改变的总和是偶数 $(\pm 2)$，$S$ 为奇数时不可能归零。

设 $target = \dfrac{|S|}{2}$。

- 若 $S > 0$，需要减少总和 $\Rightarrow$ 选择 $a_i = 1$ 的位置作为两元素段的尾元素（将贡献 $+1$ 变为 $-1$，减少 $2$）
- 若 $S < 0$，需要增加总和 $\Rightarrow$ 选择 $a_i = -1$ 的位置作为两元素段的尾元素（将贡献 $-1$ 变为 $+1$，增加 $2$）

同时，选择的位置不能相邻（因为每个元素只能属于一段，两元素段的尾元素不能再作为下一段的头）。

#### 算法流程

```
for each test case:
  读入 n, a[0..n-1]
  S = sum(a)
  
  if (S 是奇数):
    输出 -1, continue
  
  need = |S| / 2
  // S > 0 时选 a_i = 1, S < 0 时选 a_i = -1
  target_val = (S > 0) ? 1 : -1
  
  vector<bool> pair_start(n, false) // pair_start[i] = true 表示 i 是两元素段的起点
  
  // 贪心选取不相邻的位置作为两元素段的尾元素
  for (int i = 1; i < n; ) {
    if (need > 0 && a[i] == target_val && !pair_start[i - 1]) {
      pair_start[i - 1] = true;   // 生成段 [i-1, i]
      need--;
      i += 2;  // 跳过 i（它已经是尾元素了）
    } else {
      i++;
    }
  }
  
  if (need > 0) {
    输出 -1   // 理论上不会发生，但以防万一
    continue
  }
  
  // 根据 pair_start 构造划分
  vector<pair<int,int>> segs;
  for (int i = 0; i < n; ) {
    if (pair_start[i]) {
      segs.push_back({i, i + 1});  // 两元素段
      i += 2;
    } else {
      segs.push_back({i, i});      // 单元素段
      i++;
    }
  }
  
  输出 segs.size()
  输出每个段 (l+1, r+1)  // 转为 1-based
```

**时间复杂度：** $O(n)$ 每个测试用例。

**正确性证明要点：**

| 条件 | 说明 |
|------|------|
| $|S|$ 为偶数 | 每次配对减少 $2a_i$，变化量为偶数，$S$ 奇则无解 |
| 恰好选择 $\dfrac{|S|}{2}$ 个位置 | 每个选中位置改变量 $2$，总数改变 $2 \cdot \dfrac{|S|}{2} = |S|$ |
| 选择的位置不相邻 | `i += 2` 确保跳过尾元素，不会有两个相邻段交叠 |
| 不能选 $i=0$ 作为尾元素 | 循环从 $i=1$ 开始，$a_0$ 只能是头元素 |

#### 为什么这样构造一定不会遇到 need > 0 却无法继续选的情况？

因为数组长度 $n$ 足够容纳 $\dfrac{|S|}{2}$ 个不相邻的位置（至多 $\lceil n/2 \rceil$ 个），且 $|S| \le n$，所以 $\dfrac{|S|}{2} \le \dfrac{n}{2} \le \lceil n/2 \rceil$，一定有解。（但要注意 $n=1$ 时 $|S|=1$ 为奇数，已经在奇偶判断中输出了 $-1$。）

#### C++ 实现

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
  int n;
  cin >> n;
  vector<int> a(n);
  int sum = 0;
  for (int i = 0; i < n; ++i) {
    cin >> a[i];
    sum += a[i];
  }

  if (sum % 2 != 0) {
    cout << "-1\n";
    return;
  }

  int need = abs(sum) / 2;
  int target_val = (sum > 0) ? 1 : -1;

  vector<bool> pair_start(n, false);

  for (int i = 1; i < n;) {
    if (need > 0 && a[i] == target_val && !pair_start[i - 1]) {
      pair_start[i - 1] = true;
      need--;
      i += 2;
    } else {
      i++;
    }
  }

  // 根据题目数据保证，need 此时一定为 0
  // 但为安全可加 check：
  // if (need > 0) { cout << "-1\n"; return; }

  vector<pair<int, int>> segs;
  for (int i = 0; i < n;) {
    if (pair_start[i]) {
      segs.push_back({i + 1, i + 2});
      i += 2;
    } else {
      segs.push_back({i + 1, i + 1});
      i++;
    }
  }

  cout << segs.size() << "\n";
  for (auto &[l, r] : segs) {
    cout << l << " " << r << "\n";
  }
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int t;
  cin >> t;
  while (t--) {
    solve();
  }
  return 0;
}
```

#### 示例验证

**输入：**
```
4
4
1 1 1 1
6
-1 1 1 1 1 1
3
1 -1 1
1
1
```

**运行过程：**

| 用例 | $S$ | $need$ | 选择 | 划分 | 验证 |
|------|-----|--------|------|------|------|
| `[1,1,1,1]` | $4$ | $2$ | $i=1(a_1=1)$, $i=3(a_3=1)$ | $[1,2],[3,4]$ | $(1-1)+(1-1)=0$ |
| `[-1,1,1,1,1,1]` | $4$ | $2$ | $i=1(a_1=1)$, $i=3(a_3=1)$ | $[1,2],[3,4],[5,5],[6,6]$ | $(-1-1)+(1-1)+1+1=0$ |
| `[1,-1,1]` | $1$ | 奇数 | — | $-1$ | — |
| `[1]` | $1$ | 奇数 | — | $-1$ | — |

另一种常见输出方案（单段 $[1,4]$ 等同于 $(1-1)+(1-1)$ 合并）也是正确的，我们的算法给出的是另一种合法解。

#### 总结

| 要点 | 说明 |
|------|------|
| 核心技巧 | 将交错和问题转化为「单元素段 + 两元素段」构造，每合并一对改变总和 $2a_{i+1}$ |
| 关键约束 | 系数 $c_i$ 不能有相邻 $-1$ $\Leftrightarrow$ 两元素段的尾元素不能相邻 |
| 无解条件 | $\sum a_i$ 为奇数（easy version 中 $a_i = \pm 1$） |
| 贪心策略 | 从左到右扫描，能选就选，间隔跳跃 |
| 复杂度 | $O(n)$，线性 |"

