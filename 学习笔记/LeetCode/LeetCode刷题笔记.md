---
title : 'LeetCode 刷题笔记'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-05-04T12:55:35+08:00
description : "- [1. Two Sum（两数之和）](#1-two-sum两数之和)"
image : img/cat.jpg
draft : false
categories : ["LeetCode"]
tags : ["学习笔记", "LeetCode"]
---
# LeetCode 刷题笔记

## 目录

- [1. Two Sum（两数之和）](#1-two-sum两数之和)
- [3. 无重复字符的最长子串](#3-无重复字符的最长子串)
- [20. 有效的括号](#20-有效的括号)
- [53. 最大子数组和](#53-最大子数组和)
- [56. 合并区间](#56-合并区间)
- [70. 爬楼梯](#70-爬楼梯)
- [102. 二叉树的层序遍历](#102-二叉树的层序遍历)
- [121. 买卖股票的最佳时机](#121-买卖股票的最佳时机)
- [141. 环形链表](#141-环形链表)
- [146. LRU 缓存](#146-lru-缓存)
- [198. 打家劫舍](#198-打家劫舍)
- [200. 岛屿数量](#200-岛屿数量)
- [206. 反转链表](#206-反转链表)
- [215. 数组中的第K个最大元素](#215-数组中的第k个最大元素)
- [300. 最长递增子序列](#300-最长递增子序列)
- [1016. 子串能表示从 1 到 N 数字的二进制串](#1016-子串能表示从-1-到-n-数字的二进制串)

---

## 1. Two Sum（两数之和）

- **题目**：给定整数数组 `nums` 和整数目标值 `target`，找出和为目标值的两个数，返回下标。
- **分类**：数组 / 哈希表
- **思路**：遍历数组，用哈希表存储已访问元素的值→下标映射，对每个元素检查 `target - nums[i]` 是否在哈希表中。
- **代码**：
  ```python
  def twoSum(nums, target):
      seen = {}
      for i, v in enumerate(nums):
          complement = target - v
          if complement in seen:
              return [seen[complement], i]
          seen[v] = i
  ```
- **复杂度**：时间 O(n)，空间 O(n)

---

## 3. 无重复字符的最长子串

- **题目**：给定字符串 `s`，找出不含重复字符的最长子串长度。
- **分类**：滑动窗口 / 哈希表
- **思路**：维护滑动窗口 `[left, right]`，用哈希集合记录窗口内字符。右指针扩展，若遇重复则左指针收缩直至重复消除。
- **代码**：
  ```python
  def lengthOfLongestSubstring(s):
      used = set()
      left = ans = 0
      for right, ch in enumerate(s):
          while ch in used:
              used.remove(s[left])
              left += 1
          used.add(ch)
          ans = max(ans, right - left + 1)
      return ans
  ```
- **复杂度**：时间 O(n)，空间 O(字符集大小)

---

## 20. 有效的括号

- **题目**：判断字符串中的括号 `( ) [ ] { }` 是否有效匹配。
- **分类**：栈
- **思路**：遇到左括号入栈，遇到右括号检查栈顶是否匹配，匹配则弹出，否则返回 False。最后栈为空则有效。
- **代码**：
  ```python
  def isValid(s):
      pairs = {')': '(', ']': '[', '}': '{'}
      stack = []
      for ch in s:
          if ch in pairs:
              if not stack or stack[-1] != pairs[ch]:
                  return False
              stack.pop()
          else:
              stack.append(ch)
      return not stack
  ```
- **复杂度**：时间 O(n)，空间 O(n)

---

## 53. 最大子数组和

- **题目**：找出整数数组中和最大的连续子数组，返回最大和。
- **分类**：动态规划 / 分治
- **思路**：Kadane 算法。`dp[i]` 表示以 `nums[i]` 结尾的最大子数组和，状态转移 `dp[i] = max(nums[i], dp[i-1] + nums[i])`，空间优化为单变量。
- **代码**：
  ```python
  def maxSubArray(nums):
      cur = ans = nums[0]
      for v in nums[1:]:
          cur = max(v, cur + v)
          ans = max(ans, cur)
      return ans
  ```
- **复杂度**：时间 O(n)，空间 O(1)

---

## 56. 合并区间

- **题目**：给定区间集合，合并所有重叠区间。
- **分类**：排序 / 数组
- **思路**：按区间起点排序，遍历合并：若当前区间起点 ≤ 结果中最后一个区间的终点，则合并（更新终点为较大值），否则直接加入结果。
- **代码**：
  ```python
  def merge(intervals):
      intervals.sort(key=lambda x: x[0])
      merged = []
      for interval in intervals:
          if not merged or interval[0] > merged[-1][1]:
              merged.append(interval)
          else:
              merged[-1][1] = max(merged[-1][1], interval[1])
      return merged
  ```
- **复杂度**：时间 O(n log n)，空间 O(log n) 排序栈空间

---

## 70. 爬楼梯

- **题目**：爬 n 阶楼梯，每次可以爬 1 或 2 阶，有多少种不同方法？
- **分类**：动态规划
- **思路**：斐波那契数列。`dp[i] = dp[i-1] + dp[i-2]`，空间优化为滚动变量。
- **代码**：
  ```python
  def climbStairs(n):
      if n <= 2:
          return n
      a, b = 1, 2
      for _ in range(3, n + 1):
          a, b = b, a + b
      return b
  ```
- **复杂度**：时间 O(n)，空间 O(1)

---

## 102. 二叉树的层序遍历

- **题目**：按层序遍历二叉树，返回每层节点值组成的列表。
- **分类**：二叉树 / BFS
- **思路**：使用队列进行 BFS，每次处理一整层的节点，记录当前层的值。
- **代码**：
  ```python
  def levelOrder(root):
      if not root:
          return []
      from collections import deque
      q = deque([root])
      res = []
      while q:
          level = []
          for _ in range(len(q)):
              node = q.popleft()
              level.append(node.val)
              if node.left:
                  q.append(node.left)
              if node.right:
                  q.append(node.right)
          res.append(level)
      return res
  ```
- **复杂度**：时间 O(n)，空间 O(n)

---

## 121. 买卖股票的最佳时机

- **题目**：给定股价数组，只能买卖一次，求最大利润。
- **分类**：数组 / 动态规划
- **思路**：遍历时维护历史最低价，计算当天卖出能获得的利润，更新最大利润。
- **代码**：
  ```python
  def maxProfit(prices):
      min_price = float('inf')
      max_profit = 0
      for p in prices:
          if p < min_price:
              min_price = p
          else:
              max_profit = max(max_profit, p - min_price)
      return max_profit
  ```
- **复杂度**：时间 O(n)，空间 O(1)

---

## 141. 环形链表

- **题目**：判断链表是否有环。
- **分类**：链表 / 快慢指针
- **思路**：快慢指针，慢指针每次走一步，快指针每次走两步，若相遇则有环。
- **代码**：
  ```python
  def hasCycle(head):
      slow = fast = head
      while fast and fast.next:
          slow = slow.next
          fast = fast.next.next
          if slow is fast:
              return True
      return False
  ```
- **复杂度**：时间 O(n)，空间 O(1)

---

## 146. LRU 缓存

- **题目**：设计 LRU（最近最少使用）缓存，支持 `get` 和 `put` 操作，时间复杂度 O(1)。
- **分类**：设计 / 哈希表 + 双向链表
- **思路**：哈希表存 key→节点映射，双向链表维护访问顺序。get 时将节点移至头部，put 时若容量满则淘汰尾部节点。
- **代码**：
  ```python
  class DLinkedNode:
      def __init__(self, key=0, val=0):
          self.key = key
          self.val = val
          self.prev = None
          self.next = None

  class LRUCache:
      def __init__(self, capacity):
          self.cap = capacity
          self.cache = {}
          self.head = DLinkedNode()
          self.tail = DLinkedNode()
          self.head.next = self.tail
          self.tail.prev = self.head

      def _remove(self, node):
          node.prev.next = node.next
          node.next.prev = node.prev

      def _add_to_head(self, node):
          node.next = self.head.next
          node.prev = self.head
          self.head.next.prev = node
          self.head.next = node

      def get(self, key):
          if key not in self.cache:
              return -1
          node = self.cache[key]
          self._remove(node)
          self._add_to_head(node)
          return node.val

      def put(self, key, val):
          if key in self.cache:
              node = self.cache[key]
              self._remove(node)
              node.val = val
              self._add_to_head(node)
          else:
              if len(self.cache) >= self.cap:
                  last = self.tail.prev
                  self._remove(last)
                  del self.cache[last.key]
              node = DLinkedNode(key, val)
              self.cache[key] = node
              self._add_to_head(node)
  ```
- **复杂度**：get/put 均为 O(1)，空间 O(capacity)

---

## 198. 打家劫舍

- **题目**：沿街偷窃，不能偷相邻房屋，求最高金额。
- **分类**：动态规划
- **思路**：`dp[i] = max(dp[i-1], dp[i-2] + nums[i])`，空间优化为滚动变量。
- **代码**：
  ```python
  def rob(nums):
      prev = cur = 0
      for v in nums:
          prev, cur = cur, max(cur, prev + v)
      return cur
  ```
- **复杂度**：时间 O(n)，空间 O(1)

---

## 200. 岛屿数量

- **题目**：二维网格中 `1` 表示陆地，`0` 表示水，求岛屿数量（连通四方向）。
- **分类**：DFS / BFS / 并查集
- **思路**：遍历网格，遇到 `1` 则岛屿计数 +1，对该位置进行 DFS 将所有相连的 `1` 标记为 `0`。
- **代码**：
  ```python
  def numIslands(grid):
      if not grid:
          return 0
      m, n = len(grid), len(grid[0])
      dirs = [(1,0), (-1,0), (0,1), (0,-1)]

      def dfs(i, j):
          if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == '0':
              return
          grid[i][j] = '0'
          for dx, dy in dirs:
              dfs(i + dx, j + dy)

      count = 0
      for i in range(m):
          for j in range(n):
              if grid[i][j] == '1':
                  count += 1
                  dfs(i, j)
      return count
  ```
- **复杂度**：时间 O(m×n)，空间 O(m×n) 递归栈

---

## 206. 反转链表

- **题目**：反转单链表。
- **分类**：链表
- **思路**：迭代法，三个指针 prev、cur、next 逐步反转指向。
- **代码**：
  ```python
  def reverseList(head):
      prev = None
      cur = head
      while cur:
          nxt = cur.next
          cur.next = prev
          prev = cur
          cur = nxt
      return prev
  ```
- **复杂度**：时间 O(n)，空间 O(1)

---

## 215. 数组中的第K个最大元素

- **题目**：在未排序数组中找到第 K 个最大的元素。
- **分类**：快速选择 / 堆
- **思路**：维护大小为 K 的最小堆，遍历数组，堆中始终保留当前最大的 K 个元素，堆顶即为答案。
- **代码**：
  ```python
  import heapq

  def findKthLargest(nums, k):
      heap = nums[:k]
      heapq.heapify(heap)
      for v in nums[k:]:
          if v > heap[0]:
              heapq.heapreplace(heap, v)
      return heap[0]
  ```
- **复杂度**：时间 O(n log k)，空间 O(k)

---

## 300. 最长递增子序列

- **题目**：给定整数数组，求最长严格递增子序列的长度。
- **分类**：动态规划 / 贪心 + 二分
- **思路**（贪心+二分）：维护 `tails[i]` 表示长度为 `i+1` 的递增子序列的最小末尾值。遍历数组，二分找到第一个 ≥ 当前值的位置并替换。
- **代码**：
  ```python
  import bisect

  def lengthOfLIS(nums):
      tails = []
      for v in nums:
          idx = bisect.bisect_left(tails, v)
          if idx == len(tails):
              tails.append(v)
          else:
              tails[idx] = v
      return len(tails)
  ```
- **复杂度**：时间 O(n log n)，空间 O(n)

---

## 1016. 子串能表示从 1 到 N 数字的二进制串

- **题目**：给定二进制字符串 `s` 和整数 `n`，判断 `s` 的所有子串是否能表示 `[1, n]` 中的所有整数的二进制表示。
- **分类**：字符串 / 数学
- **思路**：从 `n` 向下遍历到 `1`，将每个数字转为二进制字符串，检查是否是 `s` 的子串。一旦发现缺失即返回 False。优化：当 `n > len(s) * 2` 时可直接返回 False（海莱原则：长度 m 的字符串最多有 `m * (m+1) / 2` 个子串，但更重要的是从 n/2+1 到 n 的数必须互不包含）。
- **代码**：
  ```python
  def queryString(s, n):
      # 优化：若 n > 2 * len(s) 则必然无法全部表示
      if n > 2 * len(s):
          return False
      for i in range(n, 0, -1):
          if bin(i)[2:] not in s:
              return False
      return True
  ```
- **复杂度**：时间 O(n × L) 其中 L 为二进制串平均长度（最坏约 10^6 级别，实际 n 较小时可接受），空间 O(1)

### 进一步分析

该题的核心观察是：长度固定的子串能表示的不同二进制数有限。对于长度 `k` 的子串，最多表示 `2^k` 个不同的数。字符串 `s` 长度为 `m`，则所有子串共有 `m*(m+1)/2` 个，但实际互不相同的二进制数更少。在面试中，`bin(i)[2:] not in s` 的朴素检查在 n ≤ 10^5 时是足够的；若追求更高效率，可预先将所有子串存入哈希集合。

---

## 刷题策略

| 类别 | 推荐题数 | 代表题目 |
|------|---------|---------|
| 数组 / 哈希 | 10 | 1, 49, 128, 560 |
| 链表 | 8 | 206, 141, 21, 160 |
| 二叉树 | 12 | 102, 94, 236, 543 |
| 动态规划 | 15 | 53, 70, 198, 300, 322 |
| 字符串 | 6 | 3, 20, 5, 1143 |
| 图 / DFS / BFS | 8 | 200, 207, 994, 695 |
| 堆 / 排序 | 5 | 215, 347, 23 |
| 设计题 | 3 | 146, 155, 208 |

> 持续更新中……
