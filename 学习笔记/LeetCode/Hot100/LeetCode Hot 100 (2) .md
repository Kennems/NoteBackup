---
title : 'LeetCode Hot 100 精练🥸(2)'
date : 2024-12-08T10:00:01+08:00
lastmod: 2025-12-21T10:00:01+08:00
description : "LeetCode Hot 100 精练🥸(2)" 
categories : ["LeetCode"]
tags : ["LeetCode Hot 100"]
---

# LeetCode Hot 100 精练🥸(2)

记忆中的东西一定会消退，真正留下的才是学到的，一定要及时回顾。

## (21)[139. 单词拆分](https://leetcode.cn/problems/word-break/)

**1、题目大意**
 给定字符串 `s` 和单词字典 `wordDict`，判断是否能用字典中的单词（可重复使用、无需全部使用）按顺序拼接得到 `s`。

**2、实现思路（详细）**
 采用动态规划。
 设 `f[i]` 表示 `s` 的前 `i` 个字符是否可以被成功拆分。
 初始化：`f[0]=True`，表示空串可拆分。
 状态转移：

- 枚举位置 `i`（1 到 n）
- 枚举字典中的单词 `wd`，长度为 `l`
- 若 `i>=l` 且 `s[i-l+1..i] == wd`，则可以从状态 `f[i-l]` 转移
- 更新 `f[i] |= f[i-l]`
   最终返回 `f[n]`。
   通过遍历所有可能的结尾单词，判断是否存在一种合法拆分路径覆盖整个字符串。

**3、实现代码**

```py
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        n = len(s)
        s = "#" + s
        f = [True] + [False] * n

        for i in range(1, n + 1):
            for wd in wordDict:
                l = len(wd)
                if i < l:
                    continue
                if s[i - l + 1 : i + 1] == wd:
                    f[i] |= f[i - l]

        return f[n]
```

```go
func wordBreak(s string, wordDict []string) bool {
	n := len(s)
    s = "#" + s
	f := make([]bool, n+1)
	f[0] = true
	for i := 1; i <= n; i += 1 {
		f[i] = false
	}

	for i := 1; i <= n; i += 1 {
		for _, wd := range wordDict {
			l := len(wd)
			if i < l {
				continue
			}
			if s[i-l+1:i+1] == wd {
				f[i] = f[i-l] || f[i]
			}
		}
	}

	return f[n]
}
```

## (22)[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

**1、题目大意**
 给定一个非空整数数组，除一个元素只出现一次外，其余元素均出现两次，要求在线性时间、常量空间内找出这个只出现一次的元素。

**2、实现思路（详细）**
 利用异或运算的性质：

- `a ^ a = 0`
- `a ^ 0 = a`
- 异或满足交换律和结合律

将数组中所有元素依次异或：

- 成对出现的数字相互抵消为 `0`
- 最终剩下的就是只出现一次的元素

整个过程只需一次遍历，额外空间为一个变量，满足题目要求。

**3、实现代码（你的 AC 代码）**

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        x = 0
        for i in nums:
            x ^= i
        return x
```

```go
func singleNumber(nums []int) int {
    res := 0
    for _, num := range(nums) {
        res ^= num
    }
    return res
}
```

## (23)[647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)

**1、题目大意**
 给定字符串 `s`，统计其中所有回文子串的数量。子串必须连续，回文要求正反读一致。

**2、实现思路（详细）**
 采用「中心扩展法」。
 回文串的中心分两类：

- 单个字符为中心（奇数长度）
- 两个字符之间为中心（偶数长度）

将这两种情况统一处理：

- 枚举所有可能的中心，共 `2*n-1` 个
- 对于每个中心，向左右同时扩展
- 只要左右字符相等，就形成一个回文子串并计数
- 扩展直到越界或字符不相等

所有中心扩展完成后，得到回文子串总数。

**3、实现代码**

```py
class Solution:
    def countSubstrings(self, s: str) -> int:
        n = len(s)
        res = 0

        for i in range(2 * n - 1):
            l, r = i // 2, i // 2 + (i & 1)
            while l >= 0 and r < n and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1

        return res

```

```go
func countSubstrings(s string) int {
    n := len(s)
    res := 0

    for i := 0; i < 2 * n - 1; i += 1 {
        l, r := i / 2, (i + 1) / 2
        for l >= 0 && r < n && s[l] == s[r] {
            res += 1
            l -= 1; r += 1
        }
    }

    return res 
}
```

## (24)[128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

**1、题目大意**
 给定一个未排序整数数组，找出数值连续（相邻差为 1）的最长序列长度，要求时间复杂度为 O(n)。

**2、实现思路（详细）**
 使用哈希表动态维护连续区间的长度。
 核心思想：每个数只在“加入集合”的那一刻参与一次合并。

处理流程：

- 用字典 `d` 记录某个连续区间**端点**对应的区间长度
- 遍历每个数 `nm`，若已出现则跳过
- 查询 `nm-1` 结尾的区间长度 `left`，以及 `nm+1` 开头的区间长度 `right`
- 合并形成新区间，长度 `cur = left + right + 1`
- 更新最大长度
- 只在新区间的左右端点写入长度：
  - 左端点 `nm-left`
  - 右端点 `nm+right`
- 中间元素不存长度，避免重复计算

每个元素最多处理一次，整体为线性时间。

**3、实现代码**

```py
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        d = dict()
        res = 0

        for nm in nums:
            if nm not in d:
                left = d.get(nm - 1, 0)
                right = d.get(nm + 1, 0)

                cur = left + right + 1
                if cur > res:
                    res = cur

                d[nm] = ""
                d[nm - left] = cur
                d[nm + right] = cur

        return res
```

```go
func longestConsecutive(nums []int) int {
	d := make(map[int]int)
	res := 0

	for _, nm := range nums {
		if _, exists := d[nm]; !exists {
			left := d[nm-1]
			right := d[nm+1]

			cur := left + right + 1
			if cur > res {
				res = cur
			}

			d[nm] = 1

			d[nm-left] = cur
			d[nm+right] = cur
		}
	}

	return res
}
```

## (25)[124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

### 题目大意：

给定一棵二叉树，要求找出其中路径的最大和。路径可以经过任意节点，但每个节点最多只能被使用一次。路径的定义是从一个节点到另一个节点的连续路径，路径和是路径中所有节点值的和。

### 实现思路：

1. **DFS遍历：** 采用深度优先搜索 (DFS) 来遍历二叉树，计算以每个节点为起点的路径和。
2. **递归的返回值：** 对每个节点，我们需要计算其包含左子树和右子树的最大贡献值。为了计算最大路径和，我们需要考虑以下两种情况：
   - **路径和**：节点自身的值加上其左或右子树的最大路径和。
   - **路径通过当前节点**：考虑从当前节点出发，左子树、右子树都参与的路径。
3. **最大路径和的更新：** 递归过程中维护一个全局变量 `res`，记录当前最大的路径和。对于每个节点，计算其通过左子树和右子树的路径和，然后更新 `res`。
4. **递归返回：** 每次递归返回的是“当前节点及其左右子树路径的最大和”，即一个节点到其父节点的路径和。

```py
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        res = -inf
        def dfs(r):
            if not r:
                return 0 
            nonlocal res 
            d1 = dfs(r.left)
            d2 = dfs(r.right)

            cur = max(r.val, r.val + max(d1, d2))
            res = max(res, cur, r.val + d1 + d2)

            return cur
        
        dfs(root)

        return res
```

```go
func maxPathSum(root *TreeNode) int {
	res := math.MinInt64

	var dfs func(*TreeNode) int
	dfs = func(r *TreeNode) int {
		if r == nil {
			return 0
		}

		d1 := dfs(r.Left)
		d2 := dfs(r.Right)

		cur := max(r.Val, r.Val+max(d1, d2))
		res = max(res, max(cur, r.Val+d1+d2))

		return cur
	}

	dfs(root)
	return res
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## (26)[322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

**1、题目大意**
 给定若干种面额的硬币和目标金额 `amount`，每种硬币可无限使用，求凑成目标金额所需的最少硬币数，若无法凑成返回 `-1`。

**2、实现思路**
 这是典型的完全背包动态规划问题。

定义状态：

- `f[j]` 表示凑成金额 `j` 所需的最少硬币数

初始化：

- `f[0] = 0`，金额为 0 不需要硬币
- 其余初始化为无穷大，表示暂时不可达

状态转移：

- 依次遍历每种硬币 `coins[i]`
- 对金额 `j` 从 `coins[i]` 到 `amount` 正向遍历
- 若能从 `j - coins[i]` 转移，则
   `f[j] = min(f[j], f[j - coins[i]] + 1)`

完全背包使用正向遍历金额，保证同一硬币可重复使用。
 最终检查 `f[amount]` 是否仍为无穷大，决定返回结果。

**3、实现代码**

```py
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        n = len(coins)
        f = [0] + [inf] * amount

        for i in range(n):
            for j in range(coins[i], amount + 1):
                f[j] = min(f[j], f[j - coins[i]] + 1)

        return f[amount] if f[amount] != inf else -1
```

```go
func coinChange(coins []int, amount int) int {
    n := len(coins)
    f := make([]int, amount + 1)
    inf := 0x3f3f3f3f
    f[0] = 0
    for i := 1; i <= amount; i += 1 {
        f[i] = inf
    }

    for i := 1; i <= amount; i += 1 {
        for j := 0; j < n; j += 1 {
            if i >= coins[j] {
                f[i] = min(f[i], f[i - coins[j]] + 1)
            }
        }
    }

    if f[amount] < inf {
        return f[amount]
    } else {
        return -1
    }
}
```

## (27) [494. 目标和](https://leetcode.cn/problems/target-sum/)

**题目大意**：给定一个非负整数数组 `nums` 和一个目标整数 `target`，通过给数组中的每个整数前添加 '+' 或 '-'，然后串联起来构造表达式，返回可以构造的表达式数目，使其运算结果等于目标数。

### 记忆化搜索

**实现思路**：这个问题可以转化为一个背包问题。我们可以将问题转化为在数组中选取一些数，使得它们的和等于 `target`。定义一个递归函数 `dfs(i, s)`，表示在数组 `nums` 中考虑第 i 个数，使得目前的和为 `s` 的表达式数目。递归的终止条件是遍历完所有数，如果 `s` 等于 `0`，表示找到了一种构造方式，返回 `1`，否则返回 `0`。递归过程中，如果 `s` 小于当前数 `nums[i]`，说明无法选取当前数，直接跳过；否则，递归考虑选取当前数和不选取当前数两种情况。利用缓存装饰器 `@cache` 可以将重复计算的结果进行缓存，提高计算效率。最后返回 `dfs(0, target)` 即可得到结果。

```py
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        target += sum(nums)
        if target<0 or target&1:
            return 0
        target//=2
        @cache
        def dfs(i, s):
            if i==len(nums):
                return 1 if s==0 else 0
            if s<nums[i]:
                return dfs(i+1, s)
            else:
                return dfs(i+1, s) + dfs(i+1, s-nums[i])
        return dfs(0, target)
```

### 动态规划

**实现思路**：

- **问题转化**：假设数组 `nums` 的元素和为 `s`，我们希望找到一种方式使得加正号和负号的和等于 `target`。通过推导，得到目标问题转化为：$p + n = s$， $p - n = target$ 从数组中选出和为 `(s + target) / 2` 的子集。如果 `s + target` 为负数或奇数，直接返回 0。
- **背包问题**：将问题转化为一个经典的 0-1 背包问题。我们需要求解从 `nums` 中选取一些元素，使得它们的和恰好为 `(s + target) / 2`。
- **动态规划**：定义 `dp[j]` 为和为 `j` 的子集数目。初始化 `dp[0] = 1`，表示和为 0 的方案有 1 种。然后遍历每个元素，并更新 `dp` 数组。

```py
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        target += sum(nums)
        if target < 0 or target % 2:
            return 0
        target //= 2

        n = len(nums)
        f = [[0] * (target + 1) for _ in range(n + 1)]
        f[0][0] = 1
        
        for i in range(1, n + 1):
            for j in range(target + 1):
                if j < nums[i - 1]:
                    f[i][j] = f[i - 1][j]
                else:
                    f[i][j] = f[i - 1][j] + f[i - 1][j - nums[i - 1]]

        return f[n][target]

```

## (28)[461. 汉明距离](https://leetcode.cn/problems/hamming-distance/)

**1、题目大意**
 给定两个非负整数 `x` 和 `y`，计算它们对应二进制位上不同的位置数量，即汉明距离。

**2、实现思路（详细）**
 逐位比较两个数的二进制表示。

做法：

- 对 0～31 位逐位遍历
- 通过右移并与 `1` 取位值
- 若某一位上 `x` 与 `y` 不相同，则计数加一

由于整数范围固定，遍历 32 位即可，时间复杂度为常数级。

**3、实现代码**

```py
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        cnt = 0
        for i in range(32):
            if (x >> i & 1) != (y >> i & 1):
                cnt += 1
        return cnt
```

```go
func hammingDistance(x int, y int) int {
    res := 0
    
    for i := 0; i < 32; i += 1 {
        if (x >> i & 1) != (y >> i & 1) {
            res += 1
        }
    }

    return res
}
```

## (29)[448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)

**1、题目大意**
 给定长度为 `n` 的数组 `nums`，元素取值在 `[1, n]` 内，找出所有在该范围内但未出现在数组中的数字，要求时间 O(n)，不使用额外空间。

**2、实现思路（详细）**
 利用原数组作为标记空间。

关键观察：

- 数字 `x` 对应索引 `x-1`
- 通过对对应位置加上 `n`，可以标记该数字出现过

处理步骤：

- 遍历数组中的每个元素 `num`
- 取其原始值 `(num-1)%n`，防止被多次加 `n` 后影响索引
- 对索引位置加 `n`，表示该数字出现
- 第二次遍历数组，若某位置的值仍 `<= n`，说明该位置对应的数字从未被标记，即未出现

整个过程只修改原数组，满足常量额外空间要求。

**3、实现代码**

```py
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        n = len(nums)
        for num in nums:
            nums[(num - 1) % n] += n
        return [i + 1 for i, num in enumerate(nums) if num <= n]
```

```go
func findDisappearedNumbers(nums []int) []int {
    n := len(nums)
    res := make([]int, 0)

    for _, num := range(nums) {
        nums[(num - 1) % n] += n 
    }

    for i := 0; i < n ; i += 1 {
        if nums[i] <= n {
            res = append(res, i + 1)
        }
    }

    return res
}
```

## (30)[438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

**1、题目大意**
 给定字符串 `s` 和 `p`，在 `s` 中找出所有长度等于 `p` 且与 `p` 互为字母异位词的子串起始下标。

**2、实现思路**
 使用滑动窗口配合计数哈希。

核心定义：

- 用 `d` 统计 `p` 中每个字符所需的次数
- 用 `cnt` 表示当前窗口中还缺多少个字符才能构成一个异位词

窗口维护：

- 右指针 `i` 向右扩展窗口
  - 若 `s[i]` 在 `d` 中，且该字符仍有需求，则 `cnt--`
  - 对应字符计数减一
- 当窗口长度超过 `p` 长度时，移动左指针 `j` 缩小窗口
  - 若移出的字符在 `d` 中，恢复其计数
  - 若恢复后计数为正，说明缺失字符增加，`cnt++`

判定条件：

- 当 `cnt == 0` 时，说明当前窗口恰好匹配 `p` 的字符组成
- 此时记录左指针 `j` 作为起始索引

整个过程每个指针只移动一次，时间复杂度 O(n)。

**3、实现代码**

```py
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        n, m = len(s), len(p)
        cnt = m
        res = []

        d = Counter(p)
        j = 0
        for i in range(n):
            while i - j + 1 > m:
                if s[j] in d:
                    d[s[j]] += 1
                    if d[s[j]]:
                        cnt += 1
                j += 1

            if s[i] in d:
                if d[s[i]]:
                    cnt -= 1
                d[s[i]] -= 1

            if cnt == 0:
                res.append(j)

        return res
```

```go
func findAnagrams(s string, p string) []int {
	n, m := len(s), len(p)
	cnt := m
	res := []int{}

	d := make(map[byte]int)
	for i := 0; i < m; i++ {
		d[p[i]]++
	}

	j := 0
	for i := 0; i < n; i++ {
		for i-j+1 > m {
			if v, ok := d[s[j]]; ok {
				d[s[j]]++
				if v+1 > 0 {
					cnt++
				}
			}
			j++
		}

		if v, ok := d[s[i]]; ok {
			if v > 0 {
				cnt--
			}
			d[s[i]]--
		}

		if cnt == 0 {
			res = append(res, j)
		}
	}

	return res
}
```

## (31) [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

**1、题目大意**
 给定一棵二叉树和目标和 `targetSum`，统计所有**向下**的路径中，节点值之和等于 `targetSum` 的路径数量。路径可从任意节点开始、在任意节点结束。

**2、实现思路（详细）**
 使用「前缀和 + DFS 回溯」。

核心定义：

- `cur` 表示从根到当前节点的路径和
- `ps[x]` 表示前缀和为 `x` 出现的次数

关键转化：

- 若存在某个祖先前缀和为 `cur - targetSum`
- 则从该祖先之后到当前节点的路径和等于 `targetSum`

递归过程：

- 进入节点时更新当前前缀和 `cur += r.val`
- 累加 `ps[cur - targetSum]` 到答案
- 将当前前缀和加入哈希表
- 递归左右子树
- 回溯时撤销当前前缀和计数，保证不影响其他路径

初始化 `ps[0] = 1`，用于处理从某节点直接开始的合法路径。
 整体时间复杂度 O(n)。

**3、实现代码**

```py
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        ps = defaultdict(int)
        ps[0] += 1

        def dfs(r, cur):
            if not r:
                return 0

            res = 0
            cur += r.val
            res += ps[cur - targetSum]
            ps[cur] += 1
            res += dfs(r.left, cur)
            res += dfs(r.right, cur)
            ps[cur] -= 1

            return res

        return dfs(root, 0)
```

```go
func pathSum(root *TreeNode, targetSum int) int {
	ps := make(map[int]int)
	ps[0] = 1

	var dfs func(*TreeNode, int) int
	dfs = func(r *TreeNode, cur int) int {
		if r == nil {
			return 0
		}

		res := 0
		cur += r.Val
		res += ps[cur-targetSum]

		ps[cur]++
		res += dfs(r.Left, cur)
		res += dfs(r.Right, cur)
		ps[cur]--

		return res
	}

	return dfs(root, 0)
}
```

## (32)[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

**1、题目大意**
 给定一个只包含正整数的数组，判断是否能将其分成两个子集，使两个子集的元素和相等。

**2、实现思路**
 将问题转化为 0/1 背包判定问题。

关键判断：

- 若数组总和为奇数，必然无法平分
- 设目标和 `t = sum(nums) / 2`
- 若最大元素大于 `t`，也不可能成立

状态定义：

- `f[j]` 表示是否可以选取若干元素，使其和恰好为 `j`

初始化：

- `f[0] = True`，表示不选任何数即可凑成 0

状态转移：

- 依次遍历每个数 `num`
- 金额 `j` 从 `t` 递减到 `num`
- 若 `f[j-num]` 为真，则 `f[j]` 可达

使用倒序遍历确保每个元素只使用一次。
 最终判断 `f[t]` 是否为真。

**3、实现代码**

```py
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        s = sum(nums)
        if s & 1:
            return False
        mx = max(nums)
        t = s // 2
        if mx > t:
            return False

        n = len(nums)
        f = [False] * (t + 1)
        f[0] = True
        for i, num in enumerate(nums):
            for j in range(t, num - 1, -1):
                f[j] |= f[j - num]

        return f[t]

```

```go
func canPartition(nums []int) bool {
	s := 0
	mx := 0
	for _, v := range nums {
		s += v
		if v > mx {
			mx = v
		}
	}

	if s&1 == 1 {
		return false
	}

	t := s / 2
	if mx > t {
		return false
	}

	f := make([]bool, t+1)
	f[0] = true

	for _, num := range nums {
		for j := t; j >= num; j-- {
			if f[j-num] {
				f[j] = true
			}
		}
	}

	return f[t]
}

```

## (33)[406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

**1、题目大意**
 给定若干人的身高 `hi` 和其前面身高不低于自己的人数 `ki`，要求重建一个队列，使每个人都满足对应的 `(hi, ki)` 约束。

**2、实现思路**
 采用「排序 + 插入」的贪心策略。

核心思路：

- 先处理身高高的人，因为矮个子不会影响高个子的 `ki` 统计
- 身高相同的人，`ki` 小的应排在前面

具体步骤：

- 按身高 `hi` 降序排序，`ki` 升序排序
- 依次遍历排序后的数组
- 将当前人直接插入结果数组下标为 `ki` 的位置
  - 因为此时结果数组中只包含身高 ≥ 当前人的人
  - 插入位置正好保证前面有 `ki` 个身高不低于自己的人

排序保证顺序正确，插入保证局部约束成立，整体约束自然满足。

**3、实现代码**

```py
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda p: (-p[0], p[1]))

        res = []
        for p in people:
            res.insert(p[1], p)

        return res
```

```go
func reconstructQueue(people [][]int) [][]int {
	sort.Slice(people, func(i, j int) bool {
		if people[i][0] != people[j][0] {
			return people[i][0] > people[j][0]
		}
		return people[i][1] < people[j][1]
	})

	res := [][]int{}
	for _, p := range people {
		k := p[1]
		res = append(res, nil)
		copy(res[k+1:], res[k:])
		res[k] = p
	}

	return res
}

```

## (34)[399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

**1、题目大意**
 给定若干除法等式 `Ai / Bi = values[i]`，回答查询 `Cj / Dj` 的结果；若无法由已知等式推出，返回 `-1.0`。

**2、实现思路**
 将等式看作一张**带权有向图**。

建图方式：

- 对于 `a / b = v`，建立
  - 边 `a -> b`，权值 `v`
  - 边 `b -> a`，权值 `1 / v`

查询处理：

- 对每个查询 `(x, y)`，从 `x` 出发进行 DFS
- 在搜索过程中累计路径乘积 `ans`
- 若在搜索中到达 `y`，当前乘积即为答案
- 使用 `vis` 防止重复访问，避免死循环

特殊情况：

- 若 `x` 或 `y` 不在图中，或无法从 `x` 到达 `y`，结果为 `-1.0`

由于变量和等式数量较小，DFS 搜索即可满足要求。

**3、实现代码**

```py
class Solution:
    def calcEquation(
        self, equations: List[List[str]], values: List[float], queries: List[List[str]]
    ) -> List[float]:
        def dfs(cur):
            nonlocal ans, flag
            if cur not in g:
                return
            if cur == y:
                flag = ans
                return
            vis[cur] = True
            for nx, v in g[cur]:
                if not vis[nx]:
                    tmp = ans
                    ans *= v
                    dfs(nx)
                    ans = tmp

        g = defaultdict(list)
        res = []
        for equ, v in zip(equations, values):
            a, b = equ
            g[a].append((b, v))
            g[b].append((a, 1 / v))

        for x, y in queries:
            cur = x
            vis = defaultdict(bool)
            flag = -1.0
            ans = 1.0
            dfs(cur)
            res.append(flag)

        return res

```

```go
func calcEquation(
	equations [][]string,
	values []float64,
	queries [][]string,
) []float64 {

	type Edge struct {
		to  string
		val float64
	}

	g := make(map[string][]Edge)
	for i, eq := range equations {
		a, b := eq[0], eq[1]
		v := values[i]
		g[a] = append(g[a], Edge{b, v})
		g[b] = append(g[b], Edge{a, 1.0 / v})
	}

	res := make([]float64, 0, len(queries))

	for _, q := range queries {
		x, y := q[0], q[1]

		vis := make(map[string]bool)
		ans := 1.0
		flag := -1.0

		var dfs func(string)
		dfs = func(cur string) {
			if _, ok := g[cur]; !ok {
				return
			}
			if cur == y {
				flag = ans
				return
			}
			vis[cur] = true
			for _, e := range g[cur] {
				if !vis[e.to] {
					tmp := ans
					ans *= e.val
					dfs(e.to)
					ans = tmp
				}
			}
		}

		dfs(x)
		res = append(res, flag)
	}

	return res
}
```

## (35)[394. 字符串解码](https://leetcode.cn/problems/decode-string/)

**题目大意**：给定一个经过编码的字符串，其中编码规则为`k[encoded_string]`，表示`encoded_string`中的内容重复k次。要求解码该字符串。

**实现思路**：使用递归来解码字符串。遍历输入字符串，根据不同情况进行处理：

1. 若遇到数字，则累加数字直至遇到非数字字符。
2. 若遇到字母，则直接加入当前解码的字符串中。
3. 若遇到左括号'['，则递归调用解码函数，处理括号内的内容，直至遇到右括号']'。将括号内的解码结果乘以前面累积的数字，并加入当前解码的字符串中。
4. 若遇到右括号']'，则返回当前解码的字符串和当前索引。
5. 返回最终解码结果。

```py
class Solution:
    def decodeString(self, s: str) -> str:
        def decode(i, n):
            cur = ""
            num = 0
            while i < len(s):
                if s[i].isdigit():
                    num = num * 10 + int(s[i])
                elif s[i].isalpha():
                    cur += s[i]
                elif s[i] == "[":
                    ns, ni = decode(i + 1, num)  # next str, next index
                    cur += ns
                    i = ni
                    num = 0
                elif s[i] == "]":
                    return cur * n, i
                i += 1
            return cur * n, i

        res, _ = decode(0, 1)

        return res
```

```go
func decodeString(s string) string {
	var decode func(int, int) (string, int)

	decode = func(i int, n int) (string, int) {
		cur := ""
		num := 0

		for i < len(s) {
			c := s[i]
			if c >= '0' && c <= '9' {
				num = num*10 + int(c-'0')
			} else if c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z' {
				cur += string(c)
			} else if c == '[' {
				ns, ni := decode(i+1, num)
				cur += ns
				i = ni
				num = 0
			} else if c == ']' {
				res := ""
				for k := 0; k < n; k++ {
					res += cur
				}
				return res, i
			}
			i++
		}

		res := ""
		for k := 0; k < n; k++ {
			res += cur
		}
		return res, i
	}

	res, _ := decode(0, 1)
	return res
}

```

## (36)[347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

**1、题目大意**

统计数组中各元素出现次数，返回出现频率最高的前 k 个不同元素，返回顺序不限，答案唯一。

**2、实现思路**

1. **频率统计**
    使用哈希表统计每个数字在数组中出现的次数，键为数字，值为出现频率。
2. **按频率排序**
    将哈希表转为 `(元素, 频率)` 的键值对列表，根据频率从小到大排序。
3. **截取结果**
    排序后，频率最高的 k 个元素必然位于列表末尾，直接取最后 k 个键作为答案。
4. **返回元素值**
    只保留元素本身，忽略频率。

**3、实现代码**

```py
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        return [
            k for (k, _) in sorted(list(Counter(nums).items()), key=lambda e: e[1])[-k:]
        ]
```

```go
func topKFrequent(nums []int, k int) []int {
	cnt := make(map[int]int)
	for _, v := range nums {
		cnt[v]++
	}

	type Pair struct {
		val  int
		freq int
	}

	arr := make([]Pair, 0, len(cnt))
	for v, f := range cnt {
		arr = append(arr, Pair{v, f})
	}

	sort.Slice(arr, func(i, j int) bool {
		return arr[i].freq < arr[j].freq
	})

	res := make([]int, 0, k)
	for i := len(arr) - k; i < len(arr); i++ {
		res = append(res, arr[i].val)
	}

	return res
}
```

## (37)[338. 比特位计数](https://leetcode.cn/problems/counting-bits/)

**1、题目大意**

对区间 `[0, n]` 内的每个整数，计算其二进制表示中 `1` 的个数，返回长度为 `n+1` 的结果数组。

**2、实现思路**

1. **位运算核心性质**
    对任意正整数 `i`，表达式 `i & (i - 1)` 会将 `i` 的二进制表示中**最低位的 1 消除**。
2. **状态转移关系**
    若 `i` 的最低位 1 被消除后得到 `j = i & (i - 1)`，
    则 `i` 中 `1` 的个数 = `j` 中 `1` 的个数 + 1。
3. **动态规划构建结果**
   - 定义 `res[i]` 表示整数 `i` 的二进制中 `1` 的数量
   - 初始状态：`res[0] = 0`
   - 递推公式：`res[i] = res[i & (i - 1)] + 1`
4. **线性扫描**
    从 `1` 遍历到 `n`，利用已计算的小状态直接得到当前结果，整体时间复杂度 `O(n)`，空间复杂度 `O(n)`。

**3、实现代码**

```py
class Solution:
    def countBits(self, n: int) -> List[int]:
        res = [0]
        for i in range(1, n + 1):
            res.append(res[i & (i - 1)] + 1)
        return res
```

```go
func countBits(n int) []int {
	res := make([]int, n+1)
	for i := 1; i <= n; i++ {
		res[i] = res[i&(i-1)] + 1
	}
	return res
}
```

## (38)[337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

**1、题目大意**

给定一棵二叉树，每个节点表示一间房屋及其金额。相邻（父子）节点不能同时被选择，求在不触发警报的情况下可盗取的最大金额。

**2、实现思路**

1. **树形动态规划建模**
    对每个节点定义两种状态：
   - `f[node]`：选择当前节点时，子树能获得的最大金额
   - `s[node]`：不选择当前节点时，子树能获得的最大金额
2. **状态转移**
   - 若选择当前节点，则其左右子节点都不能选：
      `f[node] = node.val + s[left] + s[right]`
   - 若不选择当前节点，则左右子节点可选可不选，取最大值：
      `s[node] = max(f[left], s[left]) + max(f[right], s[right])`
3. **后序遍历计算**
    使用 DFS 后序遍历，确保子节点状态已计算，再计算当前节点状态。
4. **最终答案**
    根节点可选可不选，返回 `max(f[root], s[root])`。

时间复杂度 `O(n)`，空间复杂度 `O(n)`。

3、实现代码

```py
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        f = defaultdict(int)
        s = defaultdict(int)

        def dfs(node):
            if not node:
                return
            dfs(node.left)
            dfs(node.right)
            f[node] = node.val + s[node.left] + s[node.right]
            s[node] = max(f[node.left], s[node.left]) + max(f[node.right], s[node.right])

        dfs(root)
        return max(f[root], s[root])

```

```go
func rob(root *TreeNode) int {
	f := make(map[*TreeNode]int)
	s := make(map[*TreeNode]int)

	var dfs func(*TreeNode)
	dfs = func(node *TreeNode) {
		if node == nil {
			return
		}
		dfs(node.Left)
		dfs(node.Right)
		f[node] = node.Val + s[node.Left] + s[node.Right]
		s[node] = max(f[node.Left], s[node.Left]) + max(f[node.Right], s[node.Right])
	}

	dfs(root)
	return max(f[root], s[root])
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

```

## (39)[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

**1、题目大意**

给定股票每日价格数组，只允许一次买入和一次卖出（先买后卖），求能获得的最大利润；若无利润则返回 0。

**2、实现思路**

1. **一次遍历建模**
    在遍历价格数组时，同时维护两个量：
   - `mn`：到当前天为止的最低买入价格
   - `res`：当前能获得的最大利润
2. **核心逻辑**
    对于当前价格 `p`：
   - 若今天卖出，利润为 `p - mn`，用它更新最大利润
   - 更新最低价格 `mn = min(mn, p)`，为后续卖出做准备
3. **正确性保证**
   - 买入一定发生在卖出之前（`mn` 来自历史价格）
   - 遍历过程中已穷尽所有合法买卖组合

时间复杂度 `O(n)`，空间复杂度 `O(1)`。

**3、实现代码**

```py
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res, mn = 0, inf

        for p in prices:
            res = max(res, p - mn)
            mn = min(p, mn)

        return res
```

```go
func maxProfit(prices []int) int {
	res := 0
	mn := math.MaxInt64

	for _, p := range prices {
		if p-mn > res {
			res = p - mn
		}
		if p < mn {
			mn = p
		}
	}

	return res
}

```

## (40)[312. 戳气球](https://leetcode.cn/problems/burst-balloons/)

- **题意**：给定n个数字`num[0~n-1]`,每次戳破一个气球可以获得`nums[i-1]*nums[i]*nums[i+1]`个硬币，最两旁默认为1，问最终获得的最大硬币数量。
- **思路**：区间DP，逆向思维, 思考如何放气球能使硬币最大，`f[i][j]`代表**开区间**`i~j`可以获得的最大硬币， 先枚举小区间，再枚举大区间，枚举区间内每个数字，状态转移有：`f[i][j] = max(f[i][j], f[i][k]+f[k][j]+nums[i]*nums[k]*nums[j])`。最终输出`f[0][n+1]`

```py
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        n = len(nums)
        nums = [1] + nums + [1]
        f = [[0] * (n + 5) for _ in range(n + 5)]

        # 开区间 (left, right) 区间内能获得的最大硬币
        for l in range(2, n + 2):  # len:2~n+1
            for i in range(n + 1 - l + 1):  # left:0~n+1-len
                j = i + l  # right = left + len
                for k in range(i + 1, j):  # 开区间(i,j) 即[i+1, j-1]
                    f[i][j] = max(
                        f[i][j], f[i][k] + f[k][j] + nums[i] * nums[k] * nums[j]
                    )

        return f[0][n + 1]

```

```go
func maxCoins(nums []int) int {
	n := len(nums)
	arr := make([]int, n+2)
	arr[0], arr[n+1] = 1, 1
	for i := 0; i < n; i++ {
		arr[i+1] = nums[i]
	}

	f := make([][]int, n+5)
	for i := 0; i < n+5; i++ {
		f[i] = make([]int, n+5)
	}

	// 开区间 (i, j) 内能获得的最大硬币
	for l := 2; l <= n+1; l++ { // len:2~n+1
		for i := 0; i <= n+1-l; i++ { // left:0~n+1-len
			j := i + l
			for k := i + 1; k < j; k++ { // 开区间 (i, j)
				f[i][j] = max(f[i][j], f[i][k]+f[k][j]+arr[i]*arr[k]*arr[j])
			}
		}
	}

	return f[0][n+1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

```



## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

**题目大意**：给定一个长度为n的整数数组`height`，数组中的每个元素代表一条垂直线的高度。找出其中的两条线，使得它们与x轴构成的容器可以容纳最多的水。

**实现思路**：使用双指针法。初始化左指针l指向数组的起始位置，右指针r指向数组的末尾位置。设置变量`ret`用于记录当前最大容量，初始化为`0`。在每一轮循环中，计算当前容器的容量，即`min(height[l], height[r])`乘以r和l之间的距离，更新`ret`。然后根据指针所指向的高度的大小，移动指针，如果`height[l]<height[r]`，则移动左指针l向右一步，否则移动右指针r向左一步。直到左右指针相遇，循环结束，返回ret即可。

- 正确性证明： 对于左右端点 $ l, r$， 两点之间的距离为$len = r-l+1-1$(因为两个点之间算一段，所以长度要减一)，$ ret = (r-l)*min(height[l], height[r])$,假设 $height[l]<height[r]$, 对于左端点，其作为柱子能容纳的最多的水的数量已经为最大值，任意的 $l<x<r$ 作为右端点一定比$r$做端点更差，所以可以排除左端点$l$，计算下一点即$l+1$。从左右端点开始，对于每次排除的点，已经找到这个端点可能的最大值， 所以可以保证结果的正确性。

```py
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r = 0, len(height) - 1
        res = 0
        
        while l < r:
            res = max(res, (r - l) * min(height[l], height[r]))
            if height[l] < height[r]:
                l += 1
            elif height[l] > height[r]:
                r -= 1
            else:
                l += 1
                r -= 1

        return res

```

## [15. 三数之和](https://leetcode.cn/problems/3sum/)

**题目大意**：给定一个整数数组 `nums`，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k`，同时还满足 `nums[i] + nums[j] + nums[k] == 0`。返回所有满足条件的不重复的三元组。

### 枚举

**实现思路**：首先对数组 `nums` 进行排序。然后遍历数组，对于每个元素 `nums[i]`，设定两个指针 `l` 和 `r` 分别指向 `i+1` 和数组末尾。在 `l` 和 `r` 之间寻找和为 `0` 的两个数。具体地，如果当前元素与前一个元素相同，跳过；如果当前元素与 `l+1` 处的元素相同，跳过；在 `l` 和 r 之间利用双指针的方式找到满足条件的两个数，如果找到了满足条件的三元组，则添加到结果中。最后返回结果列表 `res`。

```py
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        nums.sort()
        res = []
        for i in range(n):
            l = i 
            r = n-1
            if i and nums[i]==nums[i-1]:
                continue
            for m in range(l+1, n):
                if m>l+1 and nums[m]==nums[m-1]:
                    continue
                while r>m and nums[r]+nums[m]+nums[l]>0:
                    r-=1
                if m==r: break
                if nums[r]+nums[m]+nums[l]==0:
                    res.append([nums[l], nums[m], nums[r]])
        return res
```

### 计数排序双指针

**实现思路**：首先，利用 `Counter` 函数统计每个数出现的次数，并检查是否有 0 出现至少三次，如果是则将 `[0, 0, 0]` 添加到结果中。然后，对不重复的数进行排序。遍历排序后的数，对于每个数 num，如果 num 不等于 0 且 num 出现次数大于 1 且 `-num*2` 也在 Counter 中，则将 `[num, num, -num*2]` 添加到结果中。然后，在负数部分，利用双指针的方式寻找满足条件的两个数。最后返回结果列表 `res`。

```py
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        c = Counter(nums)
        if 0 in c and c[0] >= 3:
            res.append([0,0,0])
        no_repeat_nums = sorted(c.keys())
        
        for i,num in enumerate(no_repeat_nums):
            if num != 0 and c[num] > 1 and -num*2 in c:
                res.append([num,num,-num*2])
            if num < 0:
                for num3 in no_repeat_nums[ bisect_left(no_repeat_nums,((-num+1)/2)) : bisect_right(no_repeat_nums,-num*2-1) ]:
                    num2 = -num-num3
                    if num2 in c:
                        res.append([num,num2,num3])
        return res
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

**题目大意**：给定一个链表，要求删除倒数第n个节点，并返回链表的头结点。

**实现思路**：使用双指针，首先让第一个指针从头节点开始向后移动n步，然后同时移动第一个指针和第二个指针，直到第一个指针到达链表末尾。这样第二个指针所指的位置就是倒数第n个节点的前一个节点，然后进行删除操作即可。需要注意的是要考虑边界情况，比如链表长度为1，删除头节点等情况。

```py
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if not head.next:
            return None

        p1 = head
        n -= 1
        while n:
            p1 = p1.next
            n -= 1

        pre = p2 = head
        while p1.next:
            p1 = p1.next
            pre = p2
            p2 = p2.next

        if p2 == head:
            head = head.next
        else:
            pre.next = pre.next.next

        return head

```

## [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)

**题目大意**：给定一个整数数组，要求找出这个数组的下一个排列，即比当前排列大的下一个排列，如果不存在则返回字典序最小的排列。

**实现思路**：要找到下一个排列，可以遵循以下步骤：

1. 从数组末尾开始，找到第一个相邻的两个数，满足 `nums[i]` < `nums[i+1]`。
2. 如果找到了这样的一对数，说明当前排列还不是最大的排列，可以进行下一步操作。
3. 在从右往左找到的第一个位置记为 i，再从数组末尾开始，找到第一个大于 `nums[i]` 的数，记为 `j`。
4. 交换 `nums[i]` 和 `nums[j]`。
5. 将从 `i+1` 位置开始到数组末尾的数逆序排列，以得到字典序最小的排列。
6. 如果步骤1中没有找到相邻的两个数，则说明当前排列已经是最大的排列，直接将整个数组逆序排列即可。

```py
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        n = len(nums)
        i = n - 2
        while i >= 0 and nums[i] >= nums[i + 1]:
            i -= 1

        if i >= 0:
            j = n - 1
            while j >= i and nums[j] <= nums[i]:
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]

        nums[i + 1 :] = reversed(nums[i + 1 :])
```

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

**题目大意**：
给定一个按升序排列的整数数组 `nums`，数组中的值互不相同。该数组经过未知的某个下标旋转，即原本排在数组开头的一部分元素被移动到数组末尾。给定一个目标值 `target`，如果该目标值存在于旋转后的数组中，则返回其下标，否则返回 -1。要求设计一个时间复杂度为 O(log n) 的算法解决此问题。

**实现思路**：

1. 使用二分查找算法来解决此问题，以满足 O(log n) 的时间复杂度要求。
2. 初始化左右指针 `l` 和 `r` 分别指向数组的首尾元素。
3. 在循环中，计算中间位置 `mid`，判断 `nums[mid]` 是否等于目标值 `target`，若是则直接返回 `mid`。
4. 若 `nums[0] <= nums[mid]`，说明左半段是有序的，此时判断目标值是否在左半段范围内，若是则将右指针移到 `mid-1`，否则将左指针移到 `mid+1`。
5. 若 `nums[0] > nums[mid]`，说明右半段是有序的，此时判断目标值是否在右半段范围内，若是则将左指针移到 `mid+1`，否则将右指针移到 `mid-1`。
6. 若循环结束仍未找到目标值，则返回 -1。

```py
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums:
            return -1
        n = len(nums)
        l, r = 0, n - 1
        while l <= r:
            mid = l + r >> 1
            if nums[mid] == target:
                return mid
            if nums[0] <= nums[mid]:
                if nums[0] <= target < nums[mid]:
                    r = mid - 1
                else:
                    l = mid + 1
            else:
                if nums[mid] < target <= nums[n - 1]:
                    l = mid + 1
                else:
                    r = mid - 1
        return -1
```


## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

**题目大意**：给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

**思路**：双指针，为了保证对于每一个右端点，都能找到最优的左端点，对所有的已经遍历的字符都要计数`-1`， 这样在每一个符合条件的右端点判断每个字符是否有多余，这样确保左端点最优。

```py
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        m, n = len(s), len(t)
        needCnt = n
        need = Counter(t)

        res = (0, m)

        left = 0
        for right, ch in enumerate(s):
            if need[ch] > 0:
                needCnt -= 1
            need[ch] -= 1 # 关键点

            if needCnt == 0:
                while need[s[left]]!=0: # 保证对于每个符合的右端点，都可以找到最优的左端点
                    need[s[left]] += 1
                    left += 1
                if right - left + 1 < res[1] - res[0] + 1:
                    res = (left, right)
                needCnt += 1
                need[s[left]] += 1
                left += 1
                
        return s[res[0]:res[1]+1] if res[1] < m else ""
```

```go
func findAnagrams(s string, p string) []int {
	n, m := len(s), len(p)
	cnt := m
	res := []int{}

	d := make(map[byte]int)
	for i := 0; i < m; i++ {
		d[p[i]]++
	}

	j := 0
	for i := 0; i < n; i++ {
		for i-j+1 > m {
			if v, ok := d[s[j]]; ok {
				d[s[j]]++
				if v+1 > 0 {
					cnt++
				}
			}
			j++
		}

		if v, ok := d[s[i]]; ok {
			if v > 0 {
				cnt--
			}
			d[s[i]]--
		}

		if cnt == 0 {
			res = append(res, j)
		}
	}

	return res
}
```







## [301. 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)

**题目大意：**

给你一个由若干括号和字母组成的字符串 `s` ，删除最小数量的无效括号，使得输入的字符串有效。

返回所有可能的结果。答案可以按 **任意顺序** 返回。

#### 回溯

**实现思路：**

1. **统计无效括号数量：**
   - 首先遍历字符串s，统计左括号和右括号的数量l和r，其中l表示未匹配的左括号数量，r表示需要删除的右括号数量。

2. **回溯算法：**
   - 编写回溯算法来尝试删除不同位置的括号，使得字符串有效。
   - 回溯过程中需要注意以下情况：
     - 已经匹配的括号数量ln和rn，以及当前位置i。
     - 遍历字符串s的每个字符，当遇到重复的字符时跳过，以避免重复结果。
     - 如果当前字符为左括号并且ln不为0，则递归调用删除当前左括号的情况，ln减一，否则继续。
     - 如果当前字符为右括号并且rn不为0，则递归调用删除当前右括号的情况，rn减一，否则继续。

3. **检查字符串有效性：**
   - 编写辅助函数`check`来检查字符串是否有效，即左右括号数量是否匹配。

4. **返回结果：**
   - 将所有有效的字符串结果添加到结果列表ret中，并返回。

```py
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        l, r = 0, 0
        ret = []
        n = len(s)

        for c in s:
            if c=='(':
                l+=1
            elif c==')':
                if l==0:
                    r+=1
                else:
                    l-=1

        def check(st):
            cnt=0
            for c in st:
                if c=='(':
                    cnt+=1
                elif c==')':
                    cnt-=1
                    if cnt<0:
                        return False
            return (cnt==0)
        
        def backtrace(st, start, ln, rn):
            if ln==0 and rn==0 and check(st):
                ret.append(st[:])
                return 
            for i in range(start, len(st)):
                if i>start and st[i]==st[i-1]:
                    continue
                if ln+rn>n-1-i+1:
                    break
                if ln and st[i]=='(':
                    backtrace(st[:i]+st[i+1:], i, ln-1, rn)
                elif rn and st[i]==')':
                    backtrace(st[:i]+st[i+1:], i, ln, rn-1)
                
        backtrace(s, 0, l, r)
        return ret
```

#### 广搜

这个实现使用了广度优先搜索（BFS）的思路来解决问题。

1. **BFS搜索：**
   - 使用一个集合`cur`来存储当前层的所有字符串，初始时将输入的字符串`s`加入集合`cur`中。
   - 在每一轮循环中，遍历当前集合`cur`中的所有字符串，如果其中有字符串是有效的，则将其加入结果列表`ret`中。
   - 如果结果列表`ret`不为空，则跳出循环，表示已经找到了所有有效的字符串。
   - 如果结果列表`ret`为空，则需要继续进行下一轮搜索，将当前集合`cur`中的每个字符串，通过删除一个括号的方式，生成所有可能的下一层字符串，加入到集合`nxt`中。
   - 更新当前集合`cur`为`nxt`，继续下一轮搜索。

2. **检查字符串有效性：**
   - 定义一个辅助函数`check`，用于检查字符串是否有效。
   - 遍历字符串中的每个字符，遇到左括号时增加计数器`cnt`，遇到右括号时如果`cnt`为0则返回False（表示右括号没有匹配的左括号），否则减少`cnt`。
   - 最终判断`cnt`是否为0，如果为0表示字符串有效。

3. **返回结果：**
   - 返回结果列表`ret`，其中存储了所有有效的字符串。

```py
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        ret = []
        
        def check(st):
            cnt=0
            for c in st:
                if c=='(':
                    cnt+=1
                elif c==')':
                    if cnt==0:
                        return False
                    cnt-=1
            return cnt==0
        
        cur = set([s])
        
        while True:
            for st in cur:
                if check(st):
                    ret.append(st)
            if len(ret):
                break
            nxt = set()
            for st in cur:
                for i in range(len(st)):
                    if i>0 and st[i]==st[i-1]:
                        continue
                    if st[i]=='(' or st[i]==')':
                        nxt.add(st[:i]+st[i+1:])
            cur = nxt
        return ret
```

## [297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)

不止第一次遇到了。

- **题目大意**：序列化是将一个数据结构或对象转换为连续的比特位的操作，从而可以存储在文件或内存中，并且通过网络传输到另一个计算机环境中。本题要求设计一个算法来实现二叉树的序列化和反序列化，即将二叉树转换为字符串并将字符串转换回原始的二叉树结构。

- **实现思路**：

1. 序列化：使用递归将二叉树转换为字符串，根节点值与左右子树序列化结果之间使用空格分隔，空节点用 '#' 表示。
2. 反序列化：使用递归将字符串转换为二叉树。首先定义一个辅助函数 DerWork()，用于递归构建二叉树。在该函数中，按照前序遍历的顺序，依次提取字符串中的节点值，并根据节点值构建二叉树节点。如果节点值为 '#'，表示空节点，返回 None。否则，创建节点并递归构建其左右子树。
3. 在反序列化过程中，使用一个全局变量 self.s 来记录当前待处理的字符串，每次递归处理后，将字符串中已经处理过的部分剔除，继续处理剩余部分。
4. 最终返回根节点即可。

```py
class Codec:
    def serialize(self, root):
        if not root:
            return "#"
        return (
            str(root.val) + " " + self.serialize(root.left) + " " + self.serialize(root.right)
        )

    def deserialize(self, data):
        self.s = data
        return self.DerWork()

    def DerWork(self):
        if len(self.s) == 0:
            return None

        try:
            idx = self.s.index(" ")
        except:
            idx = -1

        node = self.s if idx == -1 else self.s[:idx]
        self.s = "" if idx == -1 else self.s[idx + 1 :]
        
        if node == "#":
            return None

        t = TreeNode(int(node))
        t.left = self.DerWork()
        t.right = self.DerWork()
        return t
```

## [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

- **题目大意**：给定 n 个非负整数，表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1。求在该柱状图中，能够勾勒出的矩形的最大面积。

- **实现思路**：

1. 使用单调栈解决问题。创建两个数组 l 和 r，分别记录每个柱子向左和向右第一个比其高度小的柱子的索引位置。
2. 初始化一个空栈 stk。
3. 遍历柱子的高度列表 heights：
   - 若栈不为空且当前柱子的高度小于栈顶柱子的高度，则将栈顶元素弹出，并更新栈顶元素对应的 r 值为当前柱子的索引。
   - 若栈为空或者当前柱子的高度大于栈顶柱子的高度，则将当前柱子的索引入栈。
   - 在更新 r 值的同时，若栈不为空，则更新当前柱子的 l 值为栈顶元素的索引。
4. 遍历完成后，对于每个柱子 i，计算以该柱子为高度的矩形面积为 (r[i] - l[i] - 1) * heights[i]，取最大值即为所求的最大矩形面积。
5. 若柱状图为空，则返回 0。

```py
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        # 单调栈，找到每个高度能组成的最大矩形
        # 往左右两面找到第一个小于 自身高度的柱子，那么这两个柱子之间的柱子一定可以达到自身的高度，所以是自身高度可以组成的最大面积
        n = len(heights)
        l, r = [-1]*n, [n]*n 
        res = 0

        stk = []
        for i in range(n):
            h = heights[i]
            while stk and heights[stk[-1]] >= h:
                r[stk[-1]] = i
                stk.pop()
            if stk:
                l[i] = stk[-1]
            stk.append(i)

        # stk = []
        # for i in range(n-1, -1, -1):
        #     h = heights[i]
        #     while stk and heights[stk[-1]] >= h:
        #         stk.pop()
        #     if stk:
        #         r[i] = stk[-1]
        #     stk.append(i)
        
        for i in range(n):
            if heights[i] * (r[i]-l[i]-1) > res:
                res = heights[i] * (r[i]-l[i]-1)
        
        return res
```

## [85. 最大矩形](https://leetcode.cn/problems/maximal-rectangle/)

- **题目大意**：给定一个仅包含 0 和 1、大小为 rows x cols 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

- **实现思路**：

1. 将问题转化为矩形最大面积问题。
2. 定义变量 area 用于记录最大矩形的面积。
3. 创建二维数组 left，用于记录每个位置 (i, j) 左侧连续 1 的个数。
4. 遍历二维矩阵，初始化 left 数组：
   - 若当前位置为 '1'，则 left[i][j] 等于 left[i][j-1] + 1，否则为 0。
5. 遍历矩阵的每一列，对于每一列 j，使用单调栈来计算以当前列为底边的最大矩形的面积：
   - 初始化一个空栈 stk。
   - 定义两个数组 up 和 down，分别记录当前位置上方第一个小于等于其高度的位置和下方第一个小于等于其高度的位置。
   - 遍历矩阵的每一行 i，进行以下操作：
     - 当栈不为空且栈顶位置对应的 left 值大于等于当前位置的 left 值时，弹出栈顶位置，并更新 down 值。
     - 如果栈为空，则当前位置的 up 值为 -1，否则为栈顶位置。
     - 将当前行索引入栈。
   - 在计算当前列的矩形面积时，height 为 down[i] - up[i] - 1，宽度为 left[i][j]，计算当前列的最大面积并更新 area。
6. 返回最大面积 area。

```py
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        # 转化为矩形最大面积问题
        n, m = len(matrix), len(matrix[0])
        area = 0
        left = [[0]*m for _ in range(n)]
        
        for i in range(n):
            for j in range(m):
                if matrix[i][j]=='1':
                    left[i][j] += left[i][j-1]+1 if j else 1
                    
        for j in range(m):
            stk = []
            up, down = [-1]*n, [n]*n
            for i in range(n): #行
                while stk and left[ stk[-1] ][j]>=left[i][j]:
                    down[stk[-1]] = i
                    stk.pop()
                up[i] = stk[-1] if stk else -1
                stk.append(i)

            for i in range(n):
                height = down[i]-up[i]-1
                area = max(area, height*left[i][j])
        return area
```

## [301. 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)

- **题目大意**：给定一个由字母和括号组成的字符串s，要求删除最小数量的括号，使得字符串成为有效的括号组合，并返回所有可能的结果。


- **实现思路**：
  1. 首先定义一个辅助函数`check(st)`，用于检查字符串st是否为有效的括号组合。遍历字符串中的每个字符，维护一个计数器cnt，遇到左括号增加计数，遇到右括号减少计数，若出现cnt为负数，或者遍历结束后cnt不为0，则说明括号不匹配，返回False，否则返回True。

  2. 初始化一个空列表ret，用于存储结果。

  3. 初始化一个集合cur，初始时将输入字符串s作为唯一元素加入其中。

  4. 使用while循环，直到找到符合条件的结果为止：

     - 遍历集合cur中的每个字符串，检查其是否为有效括号组合，若是则将其加入结果列表ret。

     - 若结果列表ret不为空，则说明已找到符合条件的结果，结束循环。

     - 否则，初始化一个空集合nxt，用于存储下一轮迭代的候选字符串集合。

     - 遍历集合cur中的每个字符串，对于每个字符串，尝试删除一个字符（括号），生成新的字符串，并将其加入nxt中。

     - 更新cur为nxt，继续下一轮迭代。
  5. 返回结果列表ret。

```py
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        ret = []
        def check(st):
            cnt=0
            for c in st:
                if c=='(':
                    cnt+=1
                elif c==')':
                    if cnt==0:
                        return False
                    cnt-=1
            return cnt==0
        cur = set([s])
        while True:
            for st in cur:
                if check(st):
                    ret.append(st)
            if len(ret):
                break
            nxt = set()
            for st in cur:
                for i in range(len(st)):
                    if i>0 and st[i]==st[i-1]:
                        continue
                    if st[i]=='(' or st[i]==')':
                        nxt.add(st[:i]+st[i+1:])
            cur = nxt
        return ret
```


## [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)

题目大意：给定一个字符串s，统计并返回该字符串中回文子串的数目。回文字符串是指正着读和倒过来读一样的字符串。子字符串是字符串中的由连续字符组成的一个序列。即使是由相同字符组成的不同开始位置或结束位置的子串，也会被视作不同的子串。

实现思路：遍历字符串s的所有可能的中心位置，对于每个中心位置，向两边扩展，判断是否是回文串。在扩展的过程中，每当发现一个回文子串，就将计数器加1。最终返回计数器的值即可。

```py
class Solution:
    def countSubstrings(self, s: str) -> int:
        n = len(s)
        ans = 0
        for i in range(2*n - 1):
            l, r = i//2, i//2 + (i&1)
            while l>=0 and r<n and s[l]==s[r]:
                l-=1; r+=1
                ans+=1
        return ans
```

## [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

**题目大意**：给定一个只包含数字的字符串s，表示一个IP地址，要求返回所有可能的有效IP地址，即每个IP地址由四个整数组成（每个整数位于`0`到`255`之间，且不能含有前导0），整数之间用`'.'`分隔。不能重新排序或删除s中的任何数字，可以按任何顺序返回答案。

**实现思路**：使用深度优先搜索`（DFS）`算法，递归地搜索所有可能的IP地址组合。在搜索过程中，首先确定每个整数的范围，然后遍历可能的数字组合，逐步构建`IP`地址。递归的终止条件是已经找到了四个整数并且已经遍历完了整个字符串s。

```py
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        n = len(s)
        res, addr = [], ['0']*4

        def dfs(i, start):
            if i==4 or start==n:
                if i==4 and start==n:
                    res.append('.'.join(addr))
                return 
            
            if s[start]=='0':
                addr[i] = '0'
                dfs(i+1, start+1)
                return 
            
            num = 0
            for end in range(start, n):
                num = num*10 + int(s[end])
                if num in range(256):
                    addr[i] = str(num)
                    dfs(i+1, end+1)

        dfs(0, 0)           
        return res
```

## [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

**题目大意**：给定一个二叉搜索树的根节点，需要将其转换为累加树，即每个节点的新值等于原树中大于或等于该节点值的节点值之和。

**实现思路**：

### 递归解法

```py
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        s = 0
        def dfs(r):
            nonlocal s
            if not r:
                return 0
            dfs(r.right)
            s += r.val
            r.val = s 
            dfs(r.left)
        dfs(root)
        return root
```

### **(Morris 遍历)**

从根节点开始，采用反向中序遍历（右-根-左）的方式进行遍历。利用一个变量 s 记录累加和，初始值为 0。对于每个节点，首先判断其是否存在右子节点，如果不存在，则将其值加上累加和并更新累加和，然后将当前节点指向其左子节点；如果存在右子节点，则找到其中序遍历的后继节点，即右子树中最左边的节点。如果后继节点的左子节点为空，说明还未处理过该节点，则将后继节点的左子节点指向当前节点，并将当前节点指向其右子节点；如果后继节点的左子节点为当前节点，则说明已经处理过该节点，则将后继节点的左子节点置为空，将当前节点的值加上累加和并更新累加和，并将当前节点指向其左子节点。最后返回根节点。

```py
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def getSucc(node):
            succ = node.right
            while succ.left and succ.left!=node:
                succ = succ.left
            return succ

        newRoot = root
        s = 0
        while root:
            if not root.right:
                s += root.val
                root.val = s 
                root = root.left
            else:
                succ = getSucc(root)
                if not succ.left:
                    succ.left = root
                    root = root.right
                else:
                    succ.left = None
                    s += root.val
                    root.val = s 
                    root = root.left
        return newRoot
```

## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

**题目大意**：给定一个非空数组 `nums`，数组中只包含正整数。要求判断是否能将该数组分割成两个子集，使得这两个子集的元素和相等。

**实现思路**：

1. 首先计算数组 `nums` 的总和 s。
2. 如果总和 `s` 为奇数，那么无法分割成两个和相等的子集，直接返回 `False`。
3. 如果数组中的最大值大于总和的一半，则无法分割成两个和相等的子集，直接返回 `False`。
4. 初始化一个大小为总和一半加一的布尔数组 `f`，`f[i]` 表示是否存在子集的和为 `i`。
5. 将 `f[0]` 初始化为 `True`，表示子集的和为 `0`。
6. 遍历数组 `nums`，对于每个正整数 `num`，从总和一半开始向前遍历，更新数组 `f`，如果 `f[j-num]` 为 `True`，则说明存在一个子集的和为 `j-num`，加上当前的 `num` 后，和为 `j`，因此 `f[j]` 也为 `True`。
7. 最终返回 `f[s//2]`，表示是否存在一个子集的和为总和一半，即是否能分割成两个和相等的子集。

```py
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        s = sum(nums)
        if s & 1:
            return False
        mx = max(nums)
        t = s // 2
        if mx > t:
            return False

        n = len(nums)
        f = [False] * (t + 1)
        f[0] = True
        for i, num in enumerate(nums):
            for j in range(t, num - 1, -1):
                f[j] |= f[j - num]
        return f[t]
```





## [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

**题目大意**：
给定两个单词 word1 和 word2，求通过插入、删除或替换字符，将 word1 转换成 word2 所需的最少操作数。

#### 记忆化搜索

这段代码使用了递归的方式求解，其中 dfs 函数表示将 s 的前 i 个字符转换为 t 的前 j 个字符所需的最少操作数。递归的基本情况是当 i 小于 0 时，表示 s 已经遍历完，需要插入 t 的前 j+1 个字符；当 j 小于 0 时，表示 t 已经遍历完，需要删除 s 的前 i+1 个字符；当 s[i] 等于 t[j] 时，不需要额外操作，直接递归处理 i-1 和 j-1；当 s[i] 不等于 t[j] 时，可以选择插入、删除或替换操作，选择操作数最小的方案。递归的过程中利用了缓存装饰器 `@cache` 来提高效率。

```py
class Solution:
    def minDistance(self, s: str, t: str) -> int:
        n, m = len(s), len(t)
        
        @cache
        def dfs(i, j):
            if i < 0:
                return j + 1
            if j < 0:
                return i + 1
            if s[i] == t[j]:
                return dfs(i - 1, j - 1)
            return min(dfs(i - 1, j), dfs(i, j - 1), dfs(i - 1, j - 1)) + 1

        return dfs(n - 1, m - 1)
```

#### 递推

**实现思路**：
可以使用动态规划来解决这个问题。定义一个二维数组 f，其中 f[i][j] 表示将 word1 的前 i 个字符转换为 word2 的前 j 个字符所需的最少操作数。初始时，f[i][0] 表示将 word1 的前 i 个字符全部删除，需要的操作数为 i，而 `f[0][j]` 表示将 word2 的前 j 个字符全部插入到 word1 中，需要的操作数为 j。
然后，根据动态规划的状态转移方程，逐步计算 f[i][j] 的值。若 word1[i] 等于 word2[j]，则 f[i][j] 等于 f[i-1][j-1]，即不需要额外操作；若不相等，则可以考虑插入、删除或替换操作，选择操作数最小的方案。最终，返回 f[n][m]，其中 n 和 m 分别为 word1 和 word2 的长度。

```py
class Solution:
    def minDistance(self, s: str, t: str) -> int:
        n, m = len(s), len(t)
        f = [[0] * (m + 1) for _ in range(n + 1)]
        f[0] = list(range(m + 1))  # f[0][j] = j
        for i in range(n):
            f[i + 1][0] = i + 1
            for j in range(m):
                if s[i] == t[j]:
                    f[i + 1][j + 1] = f[i][j]
                else:
                    f[i + 1][j + 1] = min(f[i][j + 1], f[i + 1][j], f[i][j]) + 1

        return f[n][m]

```

## [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

### 题目大意：

给定一个整数 `n`，要求计算由 `n` 个节点组成的二叉搜索树（BST）的数量，节点的值从 1 到 `n` 且互不相同。返回满足条件的二叉搜索树的种数。

### 解题思路：

1. **二叉搜索树的性质**：
   - 对于二叉搜索树，任意节点的左子树中的值都小于该节点的值，右子树中的值都大于该节点的值。
   - 给定一个根节点 `i`（范围从 1 到 `n`），其左子树的节点值来自 `[1, i-1]`，右子树的节点值来自 `[i+1, n]`。
   - 通过递归计算左子树和右子树的不同构造方式，可以得到当前根节点 `i` 时，整个二叉搜索树的数量。
2. **动态规划**：
   - 用 `f[i]` 表示 `i` 个节点可以组成的不同二叉搜索树的数量。
   - 初始条件：`f[0] = 1`，`f[1] = 1`，这分别表示0个节点和1个节点的二叉搜索树只有1种（空树和单个节点）。
     - 左子树的节点数为 `j-1`，右子树的节点数为 `i-j`。
     - 其中，`f[j-1]` 表示左子树有 `j-1` 个节点时的二叉搜索树种数，`f[i-j]` 表示右子树有 `i-j` 个节点时的二叉搜索树种数。
3. **算法步骤**：
   - 使用动态规划数组 `f` 来存储从 `0` 到 `n` 个节点的不同二叉搜索树的数量。
   - 通过递推关系来填充数组 `f`。
   - 最终 `f[n]` 就是所求的结果。

- `f[i] += f[j - 1] * f[i - j]` 表示当前根节点 `j` 时，左子树和右子树的种数乘积，累加到 `f[i]`。

```py
class Solution:
    def numTrees(self, n: int) -> int:
        f = [0]*(n+1)
        f[0] = 1; f[1] = 1
        for i in range(2, n+1):
            for j in range(1, i+1):
                f[i] += f[i-j]*f[j-1]
        return f[n]
```


## [148. 排序链表](https://leetcode.cn/problems/sort-list/)

**题目大意**：给定一个链表的头结点`head`，要求将链表按升序排列，并返回排序后的链表。

**实现思路**：

1. 使用归并排序的思想对链表进行排序。
2. 编写递归函数`sortFun(head, tail)`，其中`head`表示当前待排序的子链表的头结点，`tail`表示当前待排序的子链表的尾结点的下一个结点（即尾结点的后继结点）。
3. 在`sortFun`函数中，使用快慢指针找到当前待排序子链表的中间结点`mid`，并将链表分为两部分，左边部分由`head`到`mid-1`，右边部分由`mid`到`tail-1`。
4. 递归调用`sortFun`函数对左右两部分进行排序，直至排序完成。
5. 编写`merge`函数，将已经排好序的左右两部分链表进行合并，合并过程中按照结点的值大小进行比较，将较小的结点连接到结果链表中。
6. 返回合并后的链表。

```py
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def sortFun(head, tail):
            if not head:
                return head
            if head.next == tail:
                head.next = None
                return head
            slow = fast = head

            while fast != tail:
                fast = fast.next
                slow = slow.next
                if fast != tail:
                    fast = fast.next
            
            mid = slow
            return merge(sortFun(head, mid), sortFun(mid, tail))
        def merge(h1, h2):
            dummy = ListNode(-1)
            cur = dummy
            while h1 and h2:
                if h1.val<=h2.val:
                    cur.next = h1
                    h1 = h1.next
                else:
                    cur.next = h2
                    h2 = h2.next
                cur = cur.next
            if h1:
                cur.next = h1
            if h2:
                cur.next = h2
            
            return dummy.next
        return sortFun(head, None)
```

## [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/)

**题目大意**：给定一个整数数组`nums`，找出一个连续子数组，使得对该子数组进行升序排序后，整个数组都变为升序排序。要求找出符合条件的最短子数组，并输出其长度。

**实现思路**：

1. 首先初始化最大值`mx`为负无穷，最小值`mn`为正无穷，以及最短子数组的左右边界`left`和`right`分别为`-1`。
2. 遍历数组`nums`，从左向右寻找右边界`right`，如果当前元素小于前面的最大值`mx`，则更新`right`为当前索引；否则更新最大值`mx`为当前元素。
3. 同时，从右向左寻找左边界`left`，如果当前元素大于后面的最小值`mn`，则更新`left`为当前索引；否则更新最小值`mn`为当前元素。
4. 最后返回右边界和左边界的差加`1`，即为最短子数组的长度。如果左边界仍为初始值`-1`，则返回`0`表示整个数组已经有序。

```py
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        n = len(nums)
        mx, right = -inf, -1
        mn, left = inf, -1

        for i in range(n):
            if mx > nums[i]:
                right = i 
            else:
                mx = nums[i]
            
            if mn < nums[n-1-i]:
                left = n-1-i 
            else:
                mn = nums[n-1-i]

        return 0 if left == -1 else right-left+1
```

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

### **动态规划法**（O(n) 时间复杂度）：

动态规划的思路是利用之前计算出的结果来优化当前的计算，从而避免重复计算。我们通过维护两个变量来完成该问题：

- `cur`: 当前子数组的和，表示包含当前元素的最大子数组和。
- `res`: 当前已知的最大子数组和。

**具体步骤**：

1. 初始化 `res` 为一个非常小的值（比如负无穷），用来记录当前的最大子数组和。

2. 使用变量 `cur` 来存储当前子数组的和。

3. 遍历数组中的每个元素，更新 `cur`

   的值：

   - 如果 `cur + num` 小于 `num`，则说明当前子数组不再有价值，应该从当前 `num` 重新开始计算子数组的和。
   - 更新 `cur` 为 `max(num, cur + num)`。

4. 每次更新 `cur` 后，更新 `res` 为 `max(res, cur)`，确保 `res` 始终记录着最大子数组和。

```py
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        res = -inf
        cur = 0

        for nm in nums:
            cur = max(nm, cur + nm)
            res = max(res, cur)
            
        return res
```

### **分治法**

将数组递归地分为两部分，分别求解左右部分的最大子数组和，并计算跨越中点的最大子数组和。

**状态类 `Status`**：用来存储子数组的信息，包括：

- `lSum`：左侧子数组的最大和。
- `rSum`：右侧子数组的最大和。
- `mSum`：整个子数组的最大和。
- `iSum`：子数组的总和。

**递归**：

- 基本情况：数组只有一个元素时，返回该元素的 `Status`。
- 递归分解：将数组分为左右两部分，递归计算每部分的最大子数组和。

**合并**：使用 `pushUp` 函数合并左右子数组的结果，计算跨越中点的最大子数组和，并返回合并后的结果。

```py
class Solution:
    # 状态类，用来存储子数组的信息
    class Status:
        def __init__(self, lSum, rSum, mSum, iSum):
            # lSum: 左侧部分的最大子数组和
            # rSum: 右侧部分的最大子数组和
            # mSum: 当前子数组的最大子数组和
            # iSum: 当前子数组的总和
            self.lSum = lSum 
            self.rSum = rSum
            self.mSum = mSum
            self.iSum = iSum

    # 主函数：求解整个数组的最大子数组和
    def maxSubArray(self, nums):
        # 调用递归函数，获取整个数组的最大子数组和
        return self.getInfo(nums, 0, len(nums) - 1).mSum

    # 递归函数：计算子数组 [l, r] 的最大子数组和
    def getInfo(self, nums, l, r):
        # 基本情况：当数组只包含一个元素时
        if l == r:
            # 返回这个元素的状态，lSum, rSum, mSum, iSum 都是这个元素的值
            return self.Status(nums[l], nums[l], nums[l], nums[l])
        # 计算中点
        mid = (l + r) // 2
        # 递归计算左半部分的最大子数组和
        lSub = self.getInfo(nums, l, mid)
        # 递归计算右半部分的最大子数组和
        rSub = self.getInfo(nums, mid + 1, r)
        # 合并左半部分和右半部分的结果
        return self.pushUp(lSub, rSub)

    # 合并函数：合并左右子数组的结果，计算跨越中点的最大子数组和
    def pushUp(self, l, r):
        # 计算当前子数组的总和（左半部分的总和 + 右半部分的总和）
        iSum = l.iSum + r.iSum
        # 计算左半部分的最大和，可能跨越右边界，所以选择 lSum 或 l.iSum + r.lSum
        lSum = max(l.lSum, l.iSum + r.lSum)
        # 计算右半部分的最大和，可能跨越左边界，所以选择 rSum 或 r.iSum + l.rSum
        rSum = max(r.rSum, r.iSum + l.rSum)
        # 计算当前子数组的最大和，比较左子数组的最大和，右子数组的最大和，和跨越中点的最大和
        mSum = max(max(l.mSum, r.mSum), l.rSum + r.lSum)
        # 返回合并后的结果，构造一个新的 Status 对象
        return self.Status(lSum, rSum, mSum, iSum)
```

