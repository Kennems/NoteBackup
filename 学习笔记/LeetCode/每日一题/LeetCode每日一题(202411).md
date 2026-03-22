---
title : 'LeetCode每日一题(202411)'
date : 2024-11-01T15:37:01+08:00
lastmod: 2024-11-07T15:37:01+08:00
description : "每日一题（202411）" 
categories : ["LeetCode"]
tags : ["LeetCode笔记"]
---


# LeetCode每日一题(2411)

## [632. 最小区间](https://leetcode.cn/problems/smallest-range-covering-elements-from-k-lists/)

### 题目大意

给定 `k` 个非递减排列的整数列表，找到一个最小的区间 `[a,b]`，使得每个列表中至少包含一个数位于该区间内。

- 如果两个区间的宽度相同，取起点更小的区间。
- 目标是找到最优解满足题目条件。

```py
class Solution:
    def smallestRange(self, nums: List[List[int]]) -> List[int]:
        # 初始化区间边界为一个极大范围
        rangeLeft, rangeRight = -10**9, 10**9
        
        # 找到初始最大值，用于区间右端点计算
        maxValue = max(vec[0] for vec in nums)
        
        # 将每个列表的第一个元素连同其所在的列表索引和元素索引加入优先队列（最小堆）
        priorityQueue = [(vec[0], i, 0) for i, vec in enumerate(nums)]
        heapq.heapify(priorityQueue)  # 构造最小堆

        while True:
            # 从堆中取出最小值及其对应的列表和索引
            minValue, row, idx = heapq.heappop(priorityQueue)
            
            # 如果当前区间 [minValue, maxValue] 小于之前的最优区间，则更新结果
            if maxValue - minValue < rangeRight - rangeLeft:
                rangeLeft, rangeRight = minValue, maxValue
            
            # 如果该列表已到达末尾，则结束循环
            if idx == len(nums[row]) - 1:
                break
            
            # 更新最大值为下一个元素的值（如果大于当前最大值）
            maxValue = max(maxValue, nums[row][idx + 1])
            
            # 将下一个元素加入堆中
            heapq.heappush(priorityQueue, (nums[row][idx + 1], row, idx + 1))
        
        # 返回结果区间
        return [rangeLeft, rangeRight]
```

