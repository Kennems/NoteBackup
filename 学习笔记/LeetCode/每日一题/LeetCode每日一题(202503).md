---
title : 'LeetCode每日一题（202503）'
date : 2025-03-16T15:37:01+08:00
lastmod: 2025-03-16T15:37:01+08:00
description : "每日一题（202503）" 
image : img/cat.jpg
draft : false    
categories : ["LeetCode"]
tags : ["LeetCode笔记"]
---

# 每日一题（202503）







## 0316[2272. 最大波动的子字符串](https://leetcode.cn/problems/substring-with-largest-variance/)

下面提供两种解法的详细说明，包括题目大意、实现思路以及 AC 代码。

------

### 1. 题目大意

给定一个只包含小写英文字母的字符串 s，要求求出 s 所有子字符串中的“波动”最大值。
 其中“波动”定义为：子字符串中出现次数最多的字符与出现次数最少（但至少出现一次）的字符次数之差。例如，对于字符串 `"aababbb"`，存在子字符串 `"babbb"` 的波动为 3，所以返回 3；而对于 `"abcde"`，由于每个字符最多只出现一次，所以所有子字符串的波动均为 0。

------

### 2. 实现思路

#### 解法一：基于双字符枚举 + 改进 Kadane 算法

- **双字符枚举**
   只要考虑子字符串中两个字符的波动，因此可以枚举 s 中所有不同字符的两两组合。对每一对 (a, b) 都可以计算只包含这两种字符的子字符串的波动值。
- **Kadane 算法变形**
   将遍历 s 时遇到字符 a 记为 +1，遇到字符 b 记为 -1。利用类似 Kadane 算法的思想，统计连续区间的和，并通过标记是否至少出现过 a 和 b（分别用标志位 `mx` 与 `mn`）来判断当前区间是否合法。如果累计和为负，则重置状态。
- **两种顺序的考虑**
   为确保统计完整，需分别计算 fun(a, b) 与 fun(b, a) 后取最大值，因为不同的遍历顺序可能会使得某些子串被遗漏。

#### 解法二：基于全排列枚举 + 状态转移

- **全排列枚举**
   直接枚举所有小写字母的有序对（利用 `permutations(ascii_lowercase, 2)`），这样可以同时覆盖 (a, b) 与 (b, a) 两种情况。
- **状态转移思想**
   设两个状态：
  - **f0**：表示当前子数组（只考虑 a 和 b 的序列）中“还未”遇到 b（或者说还未确定包含 b）的累计值。
  - **f1**：表示当前子数组中已经包含至少一个 b 的累计值。
     初始时，令 `f0 = 0`，`f1 = -∞`（因为还没有遇到 b，所以 f1 无法参与比较）。
  - 当遇到字符 a 时：
    - f0 更新为 `max(f0, 0) + 1`
    - 如果之前已经遇到 b，则 f1 同时增加 1
  - 当遇到字符 b 时：
    - 利用 `max(f0, 0)` 重置状态，同时 f0 与 f1 都减 1，这一步确保子数组中 b 至少出现一次。
       在遍历过程中不断更新答案，最终返回最大的 f1 值。

------

### 3. AC代码

#### 解法一代码

```python
from itertools import combinations

class Solution:
    def largestVariance(self, s: str) -> int:
        def fun(a, b):
            ans = cur = 0
            # mx 用于标记是否至少出现过字符 a
            # mn 用于标记是否至少出现过字符 b
            mx = mn = False
            for ch in s:
                if ch == a:
                    cur += 1
                    mx = True
                elif ch == b:
                    cur -= 1
                    mn = True
                # 当累计和为负时，重置累计和与标记
                if cur < 0:
                    cur = 0
                    mx = mn = False
                # 如果已经同时遇到 a 和 b，则更新答案
                if mx and mn:
                    ans = max(ans, cur)
                # 如果当前只遇到了 a，则暂时减1以保留未来包含 b 的可能性
                elif mx and not mn:
                    ans = max(ans, cur - 1)
            return ans

        st = set(s)
        res = 0
        for a, b in permutations(st, 2):
            res = max(res, fun(a, b))
        return res
```

#### 解法二代码

```python
from itertools import permutations
from string import ascii_lowercase
from math import inf

class Solution:
    def largestVariance(self, s: str) -> int:
        ans = 0
        # 枚举所有小写字母的有序对
        for a, b in permutations(ascii_lowercase, 2):
            f0, f1 = 0, -inf
            for ch in s:
                if ch == a:
                    f0 = max(f0, 0) + 1
                    # 若之前已经有 b 出现，则 f1 累加
                    f1 = f1 + 1 if f1 != -inf else -inf
                elif ch == b:
                    # 遇到 b 时，从 f0 或 0 重置状态，并减去 1
                    f0 = max(f0, 0) - 1
                    f1 = max(f0, 0) - 1
                # 更新答案
                ans = max(ans, f1)
        return ans
```

------

