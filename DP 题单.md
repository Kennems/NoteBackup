# DP 题单

## 网格图DP

### [3148. 矩阵中的最大得分](https://leetcode.cn/problems/maximum-difference-score-in-a-grid/)

**题目大意**：
给定一个由正整数组成、大小为 `m x n` 的矩阵 `grid`。你可以从矩阵中的任一单元格移动到另一个位于正下方或正右侧的任意单元格。从值为 `c1` 的单元格移动到值为 `c2` 的单元格的得分为 `c2 - c1`。求能得到的最大总得分。

**实现思路**：

1. 首先，将原矩阵 `grid` 的每一行和每一列的左边和上边各增加一行或一列，用 inf 填充，这样可以避免边界情况的处理。
2. 初始化一个变量 `res` 用来记录最大总得分，初始值设为负无穷。
3. 遍历矩阵的每一个元素，计算当前元素与其上方和左方元素的最小值 `mn`，更新 res 为当前元素值减去 `mn` 的最大值，同时更新当前元素为 `mn`。
4. 返回 `res` 即为最大总得分。

```py
class Solution:
    def maxScore(self, g: List[List[int]]) -> int:
        m, n = len(g), len(g[0])
        for i in range(m):
            g[i] = [inf] + g[i]
        g = [[inf]*(n+1)] + g

        res = -inf

        for i in range(1, m+1):
            for j in range(1, n+1):
                mn = min(g[i-1][j], g[i][j-1])
                res = max(res, g[i][j] - mn)
                g[i][j] = min(g[i][j], mn)
        
        return res

```

### [2435. 矩阵中和能被 K 整除的路径](https://leetcode.cn/problems/paths-in-matrix-whose-sum-is-divisible-by-k/)

**题目大意**：
给定一个 `m x n` 的整数矩阵 `grid` 和一个整数 `k`，从矩阵的左上角 `(0, 0)` 出发，每一步只能往下或往右走，到达右下角 `(m - 1, n - 1)`。求路径和能够被 k 整除的路径数目。

**实现思路**：

1. 初始化一个三维数组 `f`，`f[i][j][s]` 表示在位置 `(i, j)` 时路径和模 `k` 为 `s` 的路径数目。
2. 从起点开始遍历矩阵，更新 f 数组。
3. 对于每个位置 `(i, j)`，计算其路径和模 k 的情况：
   - 如果 `(i, j)` 是起点，初始化 `f[i][j][grid[i][j] % k] = 1`。
   - 否则，根据上一步的状态更新当前状态：`f[i][j][(v+x)%k] += (f[i][j-1][v] + f[i-1][j][v]) % mod`，其中 v 是上一步的路径和模 k 的结果，x 是当前位置的值。
4. 返回终点 `(m, n)` 处路径和模 `k` 为 `0` 的路径数目。

这种动态规划方法能够有效地遍历矩阵并计算路径和模 k 的情况，最终得到满足条件的路径数目。

```py
class Solution:
    def numberOfPaths(self, g: List[List[int]], k: int) -> int:
        m, n = len(g), len(g[0])
        mod = int(1e9) + 7

        f = [[[0]*k for _ in range(n+1)] for _ in range(m+1)]

        for i in range(1, m+1):
            for j in range(1, n+1):
                x = g[i-1][j-1]
                if i==1 and j==1:
                    f[i][j][x%k] = 1
                    continue
                for v in range(k):
                    f[i][j][(v+x)%k] += (f[i][j-1][v] + f[i-1][j][v])%mod
        
        return f[m][n][0]
```



### [174. 地下城游戏](https://leetcode.cn/problems/dungeon-game/)

**题目大意**

骑士需要从地下城的左上角到达右下角以拯救公主。地下城是一个二维网格，每个房间有正值（增加健康点数）、负值（减少健康点数）或零值（无影响）。骑士每次只能向右或向下移动一步。我们需要计算骑士救出公主所需的最低初始健康点数。如果骑士的健康点数在任何时刻降至0或以下，他会立即死亡。

**实现思路**

1. **定义动态规划状态**:
   - 使用一个二维数组 `f`，其中 `f[i][j]` 表示骑士从房间 `(i, j)` 到达右下角所需的最小健康点数。
2. **初始化**:
   - 创建一个大小为 `(m+1) x (n+1)` 的二维数组 `f`，初始值设置为正无穷大，以方便处理边界条件。
   - 设置终点右下角的右边和下边的边界值 `f[m-1][n]` 和 `f[m][n-1]` 为1，因为到达右下角需要至少1点健康值。
3. **状态转移**:
   - 从右下角开始逆向遍历每个房间 `(i, j)`。
   - 计算从当前位置 `(i, j)` 移动到右边 `(i, j+1)` 或下边 `(i+1, j)` 所需的最小健康值 `mn`。
   - 当前房间 `(i, j)` 所需的最小健康点数为 `mn - dungeon[i][j]`，但至少为1点健康值。

```py
class Solution:
    def calculateMinimumHP(self, d: List[List[int]]) -> int:
        m, n = len(d), len(d[0])
        f = [[inf]*(n+1) for _ in range(m+1)]
        f[m-1][n] = f[m][n-1] = 1 # 入口

        for i in range(m-1, -1, -1):
            for j in range(n-1, -1, -1):
                mn = min(f[i][j+1], f[i+1][j])
                f[i][j] = max(mn - d[i][j], 1)
        
        return f[0][0]
```



## 划分型DP

### [410. 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/)

**题目大意**

给定一个非负整数数组 `nums` 和一个整数 `k`，需要将这个数组分成 `k` 个非空的连续子数组。要求设计一个算法，使得这 `k` 个子数组各自和的最大值最小。

**实现思路**

要使得每个子数组的和的最大值尽可能小，可以采用动态规划的方法。通过动态规划，可以将问题逐步分解，并通过存储中间结果减少重复计算。

1. **前缀和数组：**
   - 构建一个前缀和数组 `pre`，其中 `pre[i]` 表示 `nums` 前 `i` 个元素的和，这样在计算某个子数组的和时，可以通过减法快速得到。
   
2. **动态规划数组：**

   - 使用二维数组 `f`，其中 `f[i][j]` 表示将前 `i` 个元素分割成 `j` 个子数组时，各子数组和的最大值的最小值。
   - 初始化：`f[0][0] = 0`，表示没有元素分成 0 个子数组时，和为 0。其他位置初始化为无穷大。

3. **状态转移：**

   - 对于每个 `i` 和 `j`，遍历  ` m `，其中  ` m `表示分割点：

     - `f[i][j] = min(f[i][j], max(f[m][j-1], pre[i] - pre[m]))`
     - 这表示将 `i` 个元素分成 `j` 个子数组时的最大值取决于 `m` 之前的分割和 `m` 之后的分割。
   
4. **结果：**

   - 最终结果为 `f[n][k]`，即将 `n` 个元素分割成 `k` 个子数组时的最小的最大值。

```py
class Solution:
    def splitArray(self, nums: List[int], k: int) -> int:
        n = len(nums)
        pre = [0]*(n+1)
        for i in range(n):
            pre[i+1] = pre[i] + nums[i]
        
        f = [[inf]*(k+1) for _ in range(n+1)]
        f[0][0] = 0
        for i in range(1, n+1):
            for j in range(1, min(i, k) + 1):
                for m in range(i):
                    f[i][j] = min(f[i][j], max(f[m][j-1], pre[i] - pre[m]))
        
        return f[n][k]
```



### [1043. 分隔数组以得到最大和](https://leetcode.cn/problems/partition-array-for-maximum-sum/)

**题目大意**

给定一个整数数组 `arr` 和一个整数 `k`，要求将数组分隔为若干长度最多为 `k` 的连续子数组。每个子数组中的所有值变为该子数组中的最大值。返回将数组分隔变换后得到的元素最大和。

**实现思路**

1. **动态规划定义：** 使用数组 `f`，其中 `f[i]` 表示前 `i` 个元素可以得到的最大和。
2. **状态转移方程：** 对于每个位置 `i`，可以从前 `k` 个位置中的任意一个位置 `j` 划分一个子数组，使得子数组的最大值为 `mx`。
   - 状态转移方程：`f[i] = max(f[i], f[j] + mx * (i - j))`，其中 `mx` 是从 `j` 到 `i-1` 之间的最大值。
3. **初始化：** `f` 数组初始化为全零，因为没有元素时，最大和为零。
4. **计算最大和：** 从前往后遍历数组 `arr`，对于每个位置 `i`，检查从 `i` 往前最多 `k` 个位置，计算每个子数组的最大值，并更新 `f[i]`。
5. **返回结果：** 返回 `f[n]`，即前 `n` 个元素的最大和。

```py
class Solution:
    def maxSumAfterPartitioning(self, arr: List[int], k: int) -> int:
        n = len(arr)
        f = [0]*(n+1)

        for i in range(1, n+1):
            mx = arr[i-1]
            for j in range(i-1, max(-1, i-k-1), -1):
                mx = max(mx, arr[j])
                f[i] = max(f[i], f[j] + mx * (i-j))
        
        return f[n]
```

### [813. 最大平均值和的分组](https://leetcode.cn/problems/largest-sum-of-averages/)

**题目大意**

给定一个数组 `nums` 和一个整数 `k`，我们需要将数组 `nums` 分成最多 `k` 个非空连续子数组，使得这些子数组内的平均值总和最大。每个数必须被使用一次，并且分数不需要是整数。返回最大可能的分数，答案误差在 `10^-6` 内被视为正确。

**实现思路**

1. **前缀和数组**：
   - 我们使用一个前缀和数组 `pre` 来存储数组 `nums` 的前缀和，以便在 O(1) 时间内计算任何子数组的和。
   - `pre[i]` 表示从 `nums[0]` 到 `nums[i-1]` 的和。
2. **动态规划数组**：
   - 定义一个二维数组 `f`，其中 `f[i][j]` 表示将前 `i` 个元素分成 `j` 个子数组时的最大平均值和。
   - 初始时，`f[0][0]` 设置为 0，表示没有元素且没有子数组时的分数为 0。
3. **动态规划转移**：
   - 我们遍历每一个元素和每一个可能的子数组数。
   - 对于每一个 `f[i][j]`，我们尝试将前 `i` 个元素分成 `j` 个子数组，计算最后一个子数组的平均值和。
   - 更新 `f[i][j]` 为 `max(f[i][j], f[m][j-1] + (pre[i] - pre[m]) / (i - m))`，其中 `m` 表示前 `i` 个元素中某个位置，作为最后一个子数组的起始位置。
4. **返回结果**：
   - 最后，返回 `f[n][k]`，即将整个数组 `nums` 分成最多 `k` 个子数组时的最大平均值和。

```py
class Solution:
    def largestSumOfAverages(self, nums: List[int], k: int) -> float:
        n = len(nums)
        pre = [0] * (n+1)
        for i in range(n):
            pre[i+1] = pre[i] + nums[i]
        
        f = [[-inf]*(k+1) for _ in range(n+1)]
        f[0][0] = 0

        for i in range(1, n+1):
            for j in range(1, min(i, k) + 1):
                for m in range(i):
                    f[i][j] = max( f[i][j], f[m][j-1] + (pre[i]-pre[m])/(i-m) )
            
        return f[n][k]
```



### [1745. 分割回文串 IV](https://leetcode.cn/problems/palindrome-partitioning-iv/)

**题目大意**

给你一个字符串 `s`，如果可以将它分割成三个非空回文子字符串，那么返回 `true`，否则返回 `false`。回文字符串是指正着读和反着读都一样的字符串。

**实现思路**

1. **构建回文判断矩阵**：
   - 使用一个二维数组 `g` 来记录子字符串是否为回文。`g[i][j]` 表示子字符串 `s[i:j+1]` 是否为回文。
   - 初始化 `g`，所有单个字符的子字符串都是回文的。
   - 逆序遍历字符串的每个字符，对每对字符 `i` 和 `j`，如果 `s[i] == s[j]` 且 `s[i+1:j-1]` 是回文的，那么 `s[i:j+1]` 就是回文的。
2. **动态规划判断分割**：
   - 使用一个二维数组 `f`，其中 `f[i][j]` 表示前 `i` 个字符能否被分割成 `j` 个回文子字符串。
   - 初始状态 `f[0][0]` 为 `true`，表示空字符串可以被分割成 0 个回文子字符串。
   - 遍历字符串的每个字符 `i`，以及需要分割的数量 `j`，检查从某个位置 `m` 到 `i-1` 的子字符串是否为回文，并更新 `f[i][j]`。
3. **返回结果**：
   - 最终返回 `f[n][3]`，表示字符串 `s` 是否能被分割成 3 个回文子字符串。

```py
class Solution:
    def checkPartitioning(self, s: str) -> bool:
        n = len(s)
        g = [[True]*n for _ in range(n)]
        for i in range(n-1, -1, -1):
            for j in range(n-1, i, -1):
                g[i][j] = (s[i] == s[j]) and g[i+1][j-1]
        
        f = [[False]*4 for _ in range(n+1)]
        f[0][0] = True
        for i in range(1, n+1):
            for j in range(1, min(i, 3) + 1):
                for m in range(i):
                    f[i][j] |= (f[m][j-1] & g[m][i-1])
        
        return f[n][3]
```

### [2478. 完美分割的方案数](https://leetcode.cn/problems/number-of-beautiful-partitions/)

**题目大意**：给定一个字符串s，字符串中每个字符都是数字 '1' 到 '9'。另外给定两个整数k和`minLength`。如果对s的分割满足以下条件，则称其为一个完美分割：

1. 将s分成k段互不相交的子字符串。

2. 每个子字符串的长度都至少为`minLength`。
3. 每个子字符串的第一个字符是一个质数数字，最后一个字符是一个非质数数字，其中质数数字为 '2'，'3'，'5'和'7'。

要求返回s的完美分割数目，答案需要对`10^9 + 7`取模。

**实现思路**：首先定义两个辅助函数，`checkPrime`用于检查一个字符是否是质数数字，`checkPartition`用于检查是否可以在某个位置进行分割。然后初始化一个二维数组f，其中f[i][j]表示将前j个字符分成i段的完美分割数目。然后使用动态规划的思想进行求解，逐步填充f数组。

```python
class Solution:
    def beautifulPartitions(self, s: str, k: int, l: int) -> int:
        mod = int(1e9) + 7
        def checkPrime(c):
            return c in "2357"
        def checkPartition(j):
            return j==0 or j==n or not checkPrime(s[j-1]) and checkPrime(s[j])

        n = len(s)
        if k*l > n or not checkPrime(s[0]) or checkPrime(s[-1]):
            return 0
        f = [[0]*(n+1) for _ in range(k+1)]
        f[0][0] = 1

        for i in range(1, k+1):
            pre = 0
            for j in range(i*l, n-(k-i)*l + 1):
                if checkPartition(j-l):
                    pre = (pre + f[i-1][j-l])%mod
                if checkPartition(j):
                    f[i][j] = pre
                
        return f[k][n]
```



### [2463. 最小移动总距离](https://leetcode.cn/problems/minimum-total-distance-traveled/)

**题目大意**

在 X 轴上有一些机器人和工厂。给定一个整数数组 `robot`，其中 `robot[i]` 是第 `i` 个机器人的位置；以及一个二维整数数组 `factory`，其中 `factory[j] = [position_j, limit_j]` 表示第 `j` 个工厂的位置在 `position_j`，且第 `j` 个工厂最多可以修理 `limit_j` 个机器人。

所有机器人一开始都是坏的，需要沿着 X 轴的正方向或负方向移动，当经过一个未达到上限的工厂时，该工厂会维修该机器人，机器人停止移动。目标是最小化所有机器人移动的总距离。

**实现思路**

1. **数据排序**：将 `robot` 和 `factory` 按位置排序，以便后续计算最小移动距离。
2. **动态规划**：使用深度优先搜索（DFS）和记忆化搜索（缓存）来计算每个机器人移动到不同工厂的最小总距离。定义 `dfs(i, j)` 表示从第 `i` 个工厂开始维修第 `j` 个机器人及其后续机器人的最小总距离。
3. **递归求解**
   - 如果所有机器人都已经被维修，则返回 0。
   - 如果已经到达最后一个工厂，但剩余的机器人数量超过该工厂的修理上限，则返回无穷大表示不可行。
   - 考虑两种情况：
     1. 当前工厂不修理任何机器人，则直接跳到下一个工厂继续修理剩余的机器人。
     2. 当前工厂修理从第 `j` 个机器人开始的一部分机器人，计算其移动总距离，并递归求解剩余机器人的最小总距离。
   - 在两种情况中取最小值。
4. **返回结果**：从第 0 个工厂开始修理第 0 个机器人，返回所有机器人修理完毕的最小总距离。

```py
class Solution:
    def minimumTotalDistance(self, robot: List[int], factory: List[List[int]]) -> int:
        robot.sort(); factory.sort()
        m, n = len(robot), len(factory)

        @cache
        def dfs(i, j):
            if j==m:
                return 0
            if i==n-1:
                if m-j > factory[i][1]:
                    return inf
                return sum(abs(r - factory[i][0]) for r in robot[j:])
            
            res = dfs(i+1, j)

            s, k = 0, 0
            while k+1<=factory[i][1] and j+k<m:
                s += abs(robot[j+k] - factory[i][0])
                res = min(res, s + dfs(i+1, j+k+1))
                k+=1
            return res
        
        return dfs(0, 0)
```





## 背包DP

### [879. 盈利计划](https://leetcode.cn/problems/profitable-schemes/)

**题目大意**：
集团里有n名员工，可以完成各种工作创造利润。每种工作会产生一定的利润，同时要求一定数量的成员共同参与，且一个成员不能同时参与多个工作。工作的任何至少产生`minProfit`利润的子集被称为盈利计划，而且工作的成员总数不能超过`n`。问有多少种计划可以选择，返回结果模`10^9 + 7`的值。

**实现思路**：
使用动态规划来解决，定义一个二维数组`f[i][j]`，表示至多有i名员工和至少产生`j`利润的方案数。初始化`f[i][0]`为`1`，表示不选任何工作的方案数为`1`。然后遍历每一种工作，更新f数组，最终返回`f[n][minProfit]`即可。

```py
class Solution:
    def profitableSchemes(self, n: int, minProfit: int, group: List[int], profit: List[int]) -> int:
        mod = int(1e9) + 7
        f = [[0]*(minProfit+1) for _ in range(n+1)]

        for i in range(n+1):
            f[i][0] = 1
        
        l = len(group)
        for i in range(l):
            for j in range(n, group[i]-1, -1):
                for k in range(minProfit, -1, -1):
                    f[j][k] = (f[j][k] + f[j-group[i]][max(0, k-profit[i])])%mod
        
        return f[n][minProfit]
```

### [3082. 求出所有子序列的能量和](https://leetcode.cn/problems/find-the-sum-of-the-power-of-all-subsequences/)

**题目大意**

给定一个长度为 n 的整数数组 `nums` 和一个正整数 `k`。一个整数数组的能量定义为和等于 k 的子序列的数目。要求返回 `nums` 中所有子序列的能量和。结果可能很大，需要对 `10^9 + 7` 取余。
**实现思路**

1. 快速幂计算： 需要计算大数的幂，定义一个 `qp` 函数用于计算 a 的 k 次幂并取模。

2. 动态规划初始化： 初始化一个二维数组 `f`，其中 `f[j][c]` 表示从前 i 个数中取 c 个数，它们的和为 j 的方案数。
3. 动态规划递推： 遍历每一个数字 `v`，更新二维数组 `f`，对于每一个可能的和 `j`，遍历每一个子序列长度 `c`，更新方案数。
4. 结果计算： 遍历所有可能的子序列长度 `i`，累加能量和，每个子序列能量需要乘以 `2` 的相应次幂，因为选定的 `i` 个数与剩余的 `n-i` 个数可以组合出不同的子序列。

```py
class Solution:
    def sumOfPower(self, nums: List[int], k: int) -> int:
        def qp(a, k):
            res = 1
            while k:
                if k&1:
                    res = res*a%mod
                a = a*a%mod
                k>>=1
            return res
        
        mod = int(1e9) + 7
        n = len(nums)
        f = [[0]*(n+1) for _ in range(k+1)]
        f[0][0] = 1

        for i, v in enumerate(nums):
            for j in range(k, v-1, -1):
                for c in range(i+1, 0, -1):
                    f[j][c] += f[j-v][c-1]
        
        ans = 0
        for i in range(1, n+1):
            ans = (ans + f[k][i]*qp(2, n-i))%mod
        
        return ans 
```

### [2902. 和带限制的子多重集合的数目](https://leetcode.cn/problems/count-of-sub-multisets-with-bounded-sum/)

**题目大意**

给定一个非负整数数组 `nums` 和两个整数 `l` 和 `r`，返回 `nums` 中所有和在闭区间 `[l, r]` 之间的子多重集合的数目。子多重集合是从数组中选出一些元素构成的无序集合，每个元素可以出现`0`次或多次。结果需要对 $10^9+7$ 取余。

**实现思路**

1. **初始化与预处理**：
   - 计算数组 `nums` 的总和 `tot`。若 `tot` 小于 `l`，直接返回 0，因为任何子多重集合的和都不可能达到 `l`。
   - 使用 `Counter` 统计数组中每个元素的出现次数。
   - 设置一个数组 `f`，其中 `f[j]` 表示和为 `j` 的子多重集合的数目，初始值为 1（表示空集合的情况）。
2. **动态规划**：
   - 遍历 `nums` 中的每个唯一元素 `x` 及其出现次数 `c`。
   - 为每个元素更新动态规划数组 `f`。对于每个可能的和 `j`，更新 `f[j]` 时，考虑包含 `x` 的所有可能情况，包括 `0` 次到 `c` 次。
   - 新的数组 `g` 用来暂存更新后的数值，避免在更新过程中干扰原来的 `f`。

```py
class Solution:
    def countSubMultisets(self, nums: List[int], l: int, r: int) -> int:
        tot = sum(nums)
        if tot < l:
            return 0
        
        mod = int(1e9) + 7
        cnt = Counter(nums)
        r = min(r, tot)
        f = [cnt[0] + 1] + [0]*r
        del cnt[0]

        s = 0
        for x, c in cnt.items():
            s = min(s + x*c, r)
            g = f[:]
            for j in range(x, s+1):
                g[j] += g[j-x]
                if j>=(c+1)*x:
                    g[j] -= f[j-(c+1)*x]
                g[j]%=mod
            f = g
        
        return sum(f[l:r+1])%mod
```

## LCS

### [712. 两个字符串的最小ASCII删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/)

**题目大意**：给定两个字符串`s1`和`s2`，返回使两个字符串相等所需删除字符的`ASCII`值的最小和。

**实现思路**：使用动态规划解决。首先创建一个二维数组f，其中f[i][j]表示`s1`的前i个字符和`s2`的前j个字符组成的子串的最小`ASCII`删除和。初始化`f[0][j]`和`f[i][0]`表示空字符串到s1或s2的转换的ASCII值累加和。然后通过遍历`s1`和`s2`的每个字符，如果当前字符相等，则`f[i][j]`等于`f[i-1][j-1]`，否则取`f[i-1][j]`加上s1当前字符的`ASCII`值和f[i][j-1]加上s2当前字符的ASCII值的较小值。最终返回`f[m][n]`，即`s1`和`s2`组成的子串的最小ASCII删除和。

```py
class Solution:
    def minimumDeleteSum(self, s1: str, s2: str) -> int:
        m, n = len(s1), len(s2)
        f = [[0]*(n+1) for _ in range(m+1)]
        for i in range(1, m+1):
            f[i][0] = f[i-1][0] + ord(s1[i-1])
        for j in range(1, n+1):
            f[0][j] = f[0][j-1] + ord(s2[j-1])

        for i in range(1, m+1):
            for j in range(1, n+1):
                if s1[i-1]==s2[j-1]:
                    f[i][j] = f[i-1][j-1]
                else:
                    f[i][j] = min(f[i-1][j] + ord(s1[i-1]), f[i][j-1] + ord(s2[j-1]))
        
        return f[m][n]
```

### [1458. 两个子序列的最大点积](https://leetcode.cn/problems/max-dot-product-of-two-subsequences/)

**题目大意**

给定两个数组 `nums1` 和 `nums2`，要求返回 `nums1` 和 `nums2` 中两个长度相同的非空子序列的最大点积。数组的非空子序列是通过删除原数组中某些元素（可以不删除）后剩余数字组成的序列，但不能改变数字间相对顺序。点积定义为两个对应位置元素的乘积的和。

**实现思路**

1. **动态规划定义**：
    - 设 `f[i][j]` 表示 `nums1` 前 `i` 个元素和 `nums2` 前 `j` 个元素的最大点积。
    - 初始化 `f` 矩阵为 `-inf`，表示还没有计算过。
    - 特别的，`f[0][0]` 初始化为 `0`。

2. **状态转移方程**：
    - `dotP` 表示 `nums1[i-1]` 和 `nums2[j-1]` 的乘积。
    - 对于每个位置 `f[i][j]`，有以下四种情况：
        1. `f[i-1][j-1] + dotP`：表示选择了 `nums1[i-1]` 和 `nums2[j-1]` 作为子序列的当前元素，并加上之前的最大点积。
        2. `f[i-1][j]`：表示跳过 `nums2[j-1]`。
        3. `f[i][j-1]`：表示跳过 `nums1[i-1]`。
        4. `dotP`：表示仅考虑当前 `nums1[i-1]` 和 `nums2[j-1]` 作为单独子序列的点积。
    - 因此，状态转移方程为：`f[i][j] = max(f[i-1][j-1] + dotP, f[i-1][j], f[i][j-1], dotP)`。

3. **计算结果**：
    - 通过填充 `f` 矩阵，最终 `f[m][n]` 即为所求的两个数组最大点积。

```py
class Solution:
    def maxDotProduct(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        f = [[-inf]*(n+1) for _ in range(m+1)]
        f[0][0] = 0
        
        for i in range(1, m+1):
            for j in range(1, n+1):
                dotP = nums1[i-1] * nums2[j-1]
                f[i][j] = max(f[i-1][j-1] + dotP, f[i-1][j], f[i][j-1], dotP)
        
        return f[m][n]
```

