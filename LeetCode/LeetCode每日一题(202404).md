---
title : 'LeetCode每日一题（202404）'
date : 2024-04-17T15:37:01+08:00
lastmod: 2024-04-22T15:37:01+08:00
description : "每日一题（202404）" 
image : img/cat.jpg
draft : false    
categories : ["LeetCode"]
tags : ["LeetCode笔记"]
# password : leetcode
---


# 每日一题（202404）

###  [2009. 使数组连续的最少操作数](https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-continuous/)

- **题意**：给定一个可能有重复元素的数组，可以修改数组中值为任意其他值，问使数组连续的最小操作数。

- **思路**：由于只能改动元素，所以最后的元素个数不变，去重后，枚举每个值作为左端点，则右端点为`nums[i]+n-1`, 用双指针计算在区间内的元素个数即为可以保留的数字，其他数字修改元素值填满空隙即可。

```py
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        n = len(nums)
        nums = sorted(set(nums))
        j, ans = 0, 0
        for i in range(len(nums)):
            right = nums[i]+ n-1
            if len(nums)-1 - i + 1 <= ans:
                break
            while j<len(nums) and nums[j]<=right:
                j+=1
            ans = max(ans, j-i)
        return n-ans
```

### [1600. 王位继承顺序](https://leetcode.cn/problems/throne-inheritance/)

- **题意**：国王继承次序按嫡长子次序，实现`ThroneInheritance(string kingName)`初始化,`void birth(string parentName, string childName)` 出生，`void death(string name)`人死亡，`string[] getInheritanceOrder()` 返回 **除去** 死亡人员的当前继承顺序列表。

- **思路**：类似树形结构，国王先继承，之后是国王的长子，之后是长子的长子， 如果不存在则为国王的次子以此类推， 所以可以用`defaultdict()`存储每个的儿子情况，然后查询时，先将自己入结果列表，然后是第一个儿子...；并记录每个人的存活情况，排除掉已经死掉的人。

```py
class ThroneInheritance:

    def __init__(self, kingName: str):
        self.son = defaultdict(list)
        self.die = set()
        self.king = kingName

    def birth(self, parentName: str, childName: str) -> None:
        self.son[parentName].append(childName)

    def death(self, name: str) -> None:
        self.die.add(name)

    def getInheritanceOrder(self) -> List[str]:
        ret = []
        def dfs(root):
            if root not in self.die:
                ret.append(root)
            for child in self.son[root]:
                dfs(child)
        dfs(self.king)
        return ret
```

### [1483. 树节点的第 K 个祖先](https://leetcode.cn/problems/kth-ancestor-of-a-tree-node/)

- **题意**：给定n个点(`0~n-1`)每个点的父亲节点，快速查询每个节点的第k个祖宗节点。

- **思路**：LCA倍增原理，$ancestor[j][i] = ancestor[ ancestor[j][i-1] ][i-1]$ 节点j的第$2^i$个祖宗节点为节点第$2^{i-1}$个祖宗的第$2^{i-1}$个祖宗，即$2^{i-1}*2$个祖宗节点， 所以先预处理好每个节点的各个二进制位上的祖宗节点是谁，之后分解二进制位据可以得知任意第k个祖宗节点是谁。

```py
class TreeAncestor:
    def __init__(self, n: int, parent: List[int]):
        self.level = 16
        self.ancestor = [[-1]*self.level for _ in range(n)]
        for i in range(1, n):
            self.ancestor[i][0] = parent[i]
        for i in range(1, self.level):
            for j in range(1, n):
                if self.ancestor[j][i-1]!=-1:
                    self.ancestor[j][i] = self.ancestor[ self.ancestor[j][i-1] ][i-1]
    def getKthAncestor(self, node: int, k: int) -> int:
        while k:
            t = k&-k
            k -= t
            t = t.bit_length()-1
            node = self.ancestor[node][t]
            if node==-1:
                return -1
        return node
```

### [面试题 08.12. 八皇后](https://leetcode.cn/problems/eight-queens-lcci/)

**题目大意**：

这道题目是经典的八皇后问题的扩展，需要设计一个算法来打印 N 皇后在 N × N 棋盘上的各种摆法，其中每个皇后都不在同一行、同一列，也不在任何对角线上。

**实现思路：**

1. **回溯算法**：
   - 使用回溯算法来尝试所有可能的皇后摆放方式。
   - 从第一行开始，依次尝试每一列放置皇后，然后继续递归地尝试下一行的放置，直到所有行都放置完毕。
   - 在递归过程中，通过检查每个位置的列、主对角线和副对角线是否已经存在皇后来确定是否可以放置皇后。

2. **生成棋盘**：
   - 当确定了一种合法的皇后摆放方式时，将其转换为棋盘的形式，并将其添加到结果列表中。
   - 对于每行中皇后的位置，用 'Q' 表示，其他位置用 '.' 表示。

3. **辅助函数**：
   - `solve`: 递归函数，尝试在每行放置皇后。
   - `generate`: 将合法的皇后摆放方式转换为棋盘形式并添加到结果列表中。

4. **剪枝优化**：
   - 使用位运算来表示列、主对角线和副对角线的占用情况，可以快速判断某个位置是否可以放置皇后，从而提高效率。
   - 在每次递归时，利用位运算进行剪枝，排除不可能的位置。

```py
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        def generate():
            board = []
            for r in range(n):
                c = pos[r]
                row[c] = 'Q'
                board.append("".join(row))
                row[c] = '.'
            ret.append(board)
        
        def solve(row, column, diag1, diag2):
            if row==n:
                generate()
            else:
                avail = ( (1<<n)-1) & ( ~(column|diag1|diag2) )
                while avail:
                    colPos = avail&(-avail)
                    avail -= colPos
                    colNum = bin(colPos-1).count("1")
                    pos[row]=colNum
                    solve(row+1, column|colPos, (diag1|colPos)<<1, (diag2|colPos)>>1)
        
        pos = [0]*n 
        ret = []
        row = ['.']*n
        solve(0, 0, 0, 0)
        return ret
```

### [1702. 修改后的最大二进制字符串](https://leetcode.cn/problems/maximum-binary-string-after-change/)

- **题意**：给定一个二进制字符串`00->10, 10->01`问任意次操作后能达到的最大二进制字符串。

- **思路**：构造，根据条件推过程，`10->01`可以看作后面的0浮到前面，所有的0聚到一起可以变成`10`串，所以最后至多只有一个`0`。根据**构造**来做，统计第一个0的位置和0的个数，最后构造出最后的0的位置；**双指针**来做：遇到0时找到后面第一个0的位置，将其浮到旁边，并换成10。

**构造**：

```py
class Solution:
    def maximumBinaryString(self, binary: str) -> str:
        cnt = binary.count('0')
        if cnt<=1:
            return binary

        n = len(binary)
        ans = ['1']*n
        index = binary.index('0')
        ans[index+cnt-1]='0'
        return ''.join(ans)
```

**双指针**：

```py
class Solution:
    def maximumBinaryString(self, binary: str) -> str:
        n = len(binary)
        s = list(binary)
        j = 0
        for i in range(n):
            if s[i]=='0':
                while j<=i or (j<n and s[j]=='1'):
                    j+=1
                if j<n:
                    s[i], s[j], s[i+1] = '1', '1', '0'
        return ''.join(s)
```

### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

- **题意**：给定一个链表，分别两个一组交换相对位置。

- **思路**：链表交换next指针操作，`prev->A->B->C`变成`prev->B->A->C`，一共要变三条边。需要记录前面的点，后面的点。


```py
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = head
        prev = None
        while head and head.next:
            tmp = head.next.next
            #A->B 变成 B->A
            nxt = head.next #nxt是B, head是A
            
            nxt.next = head 
            head.next = tmp
            if prev:
                prev.next = nxt
                prev = nxt.next #是头节点(即没有前面的点)的时候不需要这一步
            if dummy==head:
                dummy = nxt
                prev = nxt.next
            
            head = tmp
        return dummy
```

### [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

- **题意**：每隔k个数字翻转链表。

- **思路**：链表交换指针操作。

```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        def reverse(head, tail): # A->B->C 变成 C->B->A
            cur = tail.next
            dummy = head
            while cur != tail:
                nxt = dummy.next

                dummy.next = cur
                cur = dummy

                dummy = nxt
            return tail, head # 原来的tail和head位置没变，但中间点的指向关系变了
        hair = ListNode(-1) # 伪头节点
        hair.next = head
        pre = hair

        while head:
            tail = pre
            for i in range(k): # 走过k个点
                tail = tail.next
                if not tail:
                    return hair.next
            head, tail = reverse(head, tail)
            pre.next = head # pre->C

            pre = tail # pre变成尾节点
            head = tail.next # 头节点变成当前尾节点的下个一个节点

        return hair.next
```

### [1766. 互质树](https://leetcode.cn/problems/tree-of-coprimes/)

- **题意**：给定一棵树`0~n-1`共n个节点， 每个节点有一个值，返回每个节点的 最近的 与自己互质的`(gcd(a,b)=1)` 祖宗节点的编号。值的范围为`[1,50]`
- **思路**：`DFS`， 遍历一遍树，维护一个祖宗节点的列表，判断当前节点与祖宗节点的互质关系。但朴素的将所有的祖宗节点都加入列表中，到深层的子节点时会遍历整个列表，会超时。由于值的范围最大为`50`，所以可以先预处理`50`范围以内的互质关系，对于每个节点，只需要判断祖宗节点里面有没有这些互质的数即可，但是需要的是最近的祖宗节点，所以还要添加一个深度信息，这样才能保证在相同值的祖宗节点中选择离自己最近的祖宗节点。

```py
class Solution:
    def getCoprimes(self, nums: List[int], edges: List[List[int]]) -> List[int]:
        n = len(nums)
        g = [[]*n for _ in range(n)]
        ret = [-1]*n
        store = [[] for _ in range(51)]
        mem = [(-1, -1)]*(51) 

        for i in range(1, 51):
            for j in range(1, 51):
                if gcd(i, j)==1:
                    store[i].append(j)

        for u, v in edges:
            g[u].append(v)
            g[v].append(u)

        def dfs(u, last, level):
            ret[u] = max(mem[i] for i in store[nums[u]])[1]
            tmp = mem[nums[u]]
            mem[nums[u]] = (level, u)
            for son in g[u]:
                if son==last:
                    continue
                dfs(son, u, level+1)
            mem[nums[u]] = tmp

        dfs(0, -1, 0)

        return ret
```

### [924. 尽量减少恶意软件的传播](https://leetcode.cn/problems/minimize-malware-spread/)

**题目大意**：给定一个由n个节点组成的网络，用n x n个邻接矩阵图graph表示。节点之间存在直接连接当且仅当graph[i][j] = 1。一些节点initial最初被恶意软件感染，如果两个节点直接连接且至少一个节点被感染，则两个节点都将被感染。移除一个节点后，返回使得整个网络中感染恶意软件的最终节点数最小的节点，若有多个节点满足条件，则返回索引最小的节点。

**实现思路**：首先使用并查集将节点分组，然后计算每个分组的大小和其中感染节点的数量。遍历initial列表，找到使得其所在分组中只有一个感染节点且分组大小最大的节点，返回其索引。

```py
class Solution:
    def minMalwareSpread(self, g: List[List[int]], initial: List[int]) -> int:
        n, m = len(g), len(initial)
        f = [i for i in range(n)]
        size = [1 for _ in range(n)] # 连通块内的点数
        num = [0 for _ in range(n)] # 连通块内的感染点数

        for x in initial:
            num[x]=1
        
        def find(x):
            if f[x] != x:
                f[x] = find(f[x])
            return f[x]

        for i in range(n):
            for j in range(i+1, n):
                if g[i][j]:
                    fi, fj = find(i), find(j)
                    if fi!=fj:
                        f[fi] = fj
                        num[fj] += num[fi]
                        size[fj] += size[fi]

        ma = -1
        initial.sort()
        idx = initial[0]
        for x in initial:
            if num[find(x)]==1 and size[find(x)]>ma:
                ma = size[find(x)]
                idx = x
        return idx
```

### [928. 尽量减少恶意软件的传播 II](https://leetcode.cn/problems/minimize-malware-spread-ii/)

- **题目大意**：给定一个由n个节点组成的网络，用n x n个邻接矩阵graph表示。节点之间存在直接连接当且仅当graph[i][j] = 1。一些节点initial最初被恶意软件感染，如果两个节点直接连接且至少一个节点被感染，则两个节点都将被感染。移除一个节点及其连接后，返回移除后能使整个网络中感染恶意软件的最终节点数最小的节点，若有多个节点满足条件，则返回索引最小的节点。


- **实现思路**：首先通过并查集将不在initial中的节点进行合并，然后计算每个initial节点的直接感染节点数。最后选择使得感染节点数最小且索引最小的initial节点返回。

```py
class Solution:
    def minMalwareSpread(self, g: List[List[int]], initial: List[int]) -> int:
        n = len(g)
        def find(x):
            if f[x] != x:
                f[x] = find(f[x])
            return f[x]

        f = [i for i in range(n)]
        source = [[] for _ in range(n)]
        cnt = [0]*n
        init = [0]*n
        for i in initial:
            init[i] = 1
        for i in range(n):
            if init[i]:
                continue
            for j in range(n):
                if init[j] or not g[i][j]:
                    continue
                fi, fj = find(i), find(j)
                if fi!=fj:
                    f[fi] = fj
        
        for x in initial:
            infected = [0]*n 
            for j in range(n):
                if not g[x][j] or init[j] or infected[find(j)]:
                    continue
                infected[find(j)] = 1
            for j in range(n):
                if infected[j]:
                    source[j].append(x)
        for i in range(n):
            if len(source[i])==1:
                root = source[i][0]
                for j in range(n):
                    if find(i)==find(j): # 是一个连通块
                        cnt[root]+=1
        idx = initial[0]
        for x in initial:
            if cnt[x]>cnt[idx] or (cnt[x]==cnt[idx] and x<idx):
                idx = x
        return idx
```

### [2007. 从双倍数组中还原原数组](https://leetcode.cn/problems/find-original-array-from-doubled-array/)

**题目大意**：给定一个数组 `changed`，通过以下方式构造原始数组 `original`：将 `changed` 中的每个元素乘以2，并将结果随机打乱，然后返回原始数组 `original`。如果无法构造原始数组，则返回空数组。

**实现思路**：首先判断数组 `changed` 的长度是否为偶数，如果不是偶数，则无法构造原始数组，直接返回空数组。接着使用一个字典记录数组 `changed` 中各个元素的出现次数。然后遍历数组 `changed`，对于每个元素 i，判断是否存在 i 的两倍的元素，若存在，则将 i 和其两倍的元素都从字典中减去一个，并将 i 加入结果数组中。最后判断结果数组的长度是否达到了原始数组的一半，如果达到了则返回结果数组的前一半，否则返回空数组。

```py
class Solution:
    def findOriginalArray(self, changed: List[int]) -> List[int]:
        n = len(changed)
        if n&1:
            return []

        d = defaultdict(int)
        res = []
        cnt = 0
        changed.sort()
        for i in changed:
            d[i]+=1
        for i in changed:
            if not d[i]:
                continue
            x = i*2

            if i==x: # 0*2 = 0
                d[x]-=1
                if d[x]:
                    d[x]-=1 # 匹配成功
                    cnt+=1
                    res.append(i)
            else:
                if d[x]:
                    d[x]-=1 # 匹配成功
                    d[i]-=1
                    cnt+=1
                    res.append(i)
        if cnt>=n//2:
            return res[:n//2]
        return []
```

**实现思路**：首先使用 `Counter` 统计数组 `changed` 中各个元素的出现次数。然后将字典中键为 `0` 的元素（如果存在）弹出，因为原始数组中不可能有 `0`。接着判断字典中键为 `0` 的元素出现次数是否为偶数，如果不是偶数则无法构造原始数组，直接返回空数组。然后初始化一个由 `0` 组成的数组 `ans`，长度为 `cnt_0` 的一半。接下来遍历字典中的键值对，对于每个键 `x`，判断是否存在其一半的键值对，并将 `x` 加入结果数组 `ans` 中相应次数。最后返回结果数组 `ans`。

```py
class Solution:
    def findOriginalArray(self, changed: List[int]) -> List[int]:
        cnt = Counter(changed)
        cnt_0 = cnt.pop(0, 0)
        if cnt_0&1:
            return []
        ans = [0]*(cnt_0//2)

        for x in cnt:
            if x%2==0 and cnt[x//2]:
                continue
            while x in cnt:
                if cnt[2*x]<cnt[x]:
                    return []
                ans.extend([x]*(cnt[x]))
                if cnt[2*x]>cnt[x]:
                    cnt[2*x]-=cnt[x]
                    x*=2
                else:
                    x*=4
        return ans
```

### [1883. 准时抵达会议现场的最小跳过休息次数](https://leetcode.cn/problems/minimum-skips-to-arrive-at-meeting-on-time/)


- **题目大意**：题目给出了一组道路的长度和行驶速度，以及剩余可用时间。要求计算在给定的时间内，最少跳过休息次数，以准时抵达会议现场。

- **实现思路**：动态规划。使用二维数组dp[i][j]表示到达第i条路，跳过j次休息的最小距离。初始化dp数组，然后遍历每一条路，在遍历的过程中更新dp数组。最后检查是否有方案可以在规定时间内到达会议现场，如果有，返回跳过休息的最小次数，否则返回-1。

```py
class Solution:
    def minSkips(self, dist: List[int], speed: int, hoursBefore: int) -> int:
        n = len(dist)
        if sum(dist) > hoursBefore * speed:
            return -1

        dp = [[inf] * (n + 1) for _ in range(n + 1)]
        dp[0][0] = 0

        for i in range(1, n):
            for j in range(i+1): # 到第i条路，跳过次数为j的最小距离
                # 不跳过第 i 条路
                dp[i][j] = min(dp[i][j], (dp[i-1][j] + dist[i-1] + speed -1)//speed*speed)
                # 跳过第 i 条路
                if j:
                    dp[i][j] = min(dp[i][j], dp[i - 1][j -1] + dist[i-1])

        for j in range(n): # 可以跳0~n-1次(最后一条边不用跳)
            if dp[n-1][j]+dist[-1] <= hoursBefore * speed:
                return j
        return -1
```

### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

- **题目大意** ：给定一个无重复元素的整数数组 `candidates` 和一个目标整数 `target`，找出 `candidates` 中可以使数字和为目标数 `target` 的所有不同组合，并以列表形式返回。可以无限制重复选择 `candidates` 中的同一个数字。

- **实现思路** ：

1. 使用深度优先搜索`（DFS）`进行组合搜索。
2. 定义一个辅助函数 `dfs(i, s)`，其中 i 表示当前遍历到的 `candidates` 数组的索引，`s` 表示当前已经选取的数字的和。
3. 在 `dfs` 函数中，如果 s 大于等于 `target` 或者已经遍历到数组末尾，则进行以下判断：
   - 如果 s 等于 `target`，则将当前的组合 `ls` 加入结果列表 `ret` 中。
   - 如果 s 大于 `target` 或者已经遍历到数组末尾，则直接返回。
4. 在 `dfs` 函数中，分别尝试两种情况：
   - 不选择当前数字，继续向后搜索：`dfs(i+1, s)`。
   - 选择当前数字，继续向后搜索：将当前数字加入组合 ls 中，更新 s，并递归调用 `dfs(i, s+c[i])`。
5. 在递归调用之后，需要将当前选择的数字从组合 `ls` 中弹出，保证不影响后续搜索。
6. 最终返回结果列表 `ret`。

```py
class Solution:
    def combinationSum(self, c: List[int], t: int) -> List[List[int]]:
        n = len(c)
        ret, ls = [], []
        def dfs(i, s):
            if s>=t or i==n:
                if s==t:
                    ret.append(ls[:])
                return
            dfs(i+1, s)
            ls.append(c[i])
            dfs(i, s+c[i])
            ls.pop()
        dfs(0, 0)
        return ret
```

### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

- **题目大意**：找出所有相加之和为n的k个数的组合，要求使用数字1到9，每个数字最多使用一次，返回所有可能的有效组合的列表。列表不能包含相同的组合两次，组合可以以任何顺序返回。

- **实现思路**：使用深度优先搜索（DFS）的方法，从1到9的数字中进行遍历，每次选择一个数字或不选择，直到满足条件为止。递归过程中，维护一个列表ls，记录当前已选数字的集合，如果当前集合的数字个数等于k且它们的和等于n，则将其加入结果列表ret中。

```py
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        ret, ls = [], []
        def dfs(i, s, cnt):
            if i==10 or s>=n or cnt>=k:
                if s==n and cnt==k:
                    ret.append(ls[:])
                return
            dfs(i+1, s, cnt)
            ls.append(i)
            dfs(i+1, s+i, cnt+1)
            ls.pop()
        dfs(1, 0, 0)
        return ret
```

### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

- **题目大意**：给定一个由不同整数组成的数组nums和一个目标整数target，要求从nums中找出总和为target的元素组合的个数。

- **实现思路**：这是一个典型的动态规划问题。使用递归+记忆化搜索（Memoization）的方法来解决。定义dfs函数，参数为当前的和s，函数返回当前和s能够组成的组合数目。递归的终止条件是当前和s大于等于目标target，则返回1（如果s等于target，则表示找到一种组合）。否则，遍历数组nums中的每个数，对每个数进行递归调用dfs(s+i)，累加得到的组合数目。使用cache装饰器对dfs函数进行记忆化搜索，避免重复计算。最终返回dfs(0)，即从初始和为0开始的组合数目。

```py
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        @cache
        def dfs(s):
            if s>=target:
                if s==target:
                    return 1
                return 0
            cnt = 0
            for i in nums:
                cnt += dfs(s+i)
            return cnt

        return dfs(0)
```

### [1146. 快照数组](https://leetcode.cn/problems/snapshot-array/)

**题目大意**：实现一个支持快照的数组数据结构`SnapshotArray`，具体包括以下接口：

1. `SnapshotArray(length)`：初始化一个与指定长度相等的类数组的数据结构，初始时每个元素都为0。
2. `set(index, val)`：将指定索引index处的元素设置为`val`。
3. `snap()`：获取该数组的快照，并返回快照的编号snap_id（快照号是调用snap()的总次数减去1）。
4. `get(index, snap_id)`：根据指定的snap_id选择快照，并返回该快照指定索引index的值。

**实现思路**：使用一个字典存储每个索引处的快照历史记录，键为索引，值为一个列表，列表中每个元素是一个元组，元组第一个值是快照的编号，第二个值是该快照对应索引的值。对于set操作，直接将快照编号和值添加到对应索引的列表中；对于snap操作，记录快照编号并返回；对于get操作，利用二分查找找到snap_id对应的快照，返回快照中该索引的值。

```py
class SnapshotArray:

    def __init__(self, length: int):
        self.id = 0
        self.arr = defaultdict(list)

    def set(self, index: int, val: int) -> None:
        self.arr[index].append((self.id, val))

    def snap(self) -> int:
        self.id+=1
        return self.id-1

    def get(self, index: int, snap_id: int) -> int:
        # if not len(self.arr[index]):
        #     return 0
        # l, r = 0, len(self.arr[index])-1
        # while l<r:
        #     mid = (l+r+1)>>1
        #     if self.arr[index][mid][0] <= snap_id:
        #         l = mid
        #     else:
        #         r = mid-1
        # return self.arr[index][r][1] if self.arr[index][r][0] <= snap_id else 0
        l = bisect.bisect_right(self.arr[index], (snap_id, inf)) - 1
        return self.arr[index][l][1] if l>=0 else 0

```

### [857. 雇佣 K 名工人的最低成本](https://leetcode.cn/problems/minimum-cost-to-hire-k-workers/)

**题目大意**：给定一组工人，每个工人有工作质量和最低期望工资，现在要雇佣 k 名工人组成一个工资组。雇佣工人时，按照每名工人的工作质量与同组其他工人的工作质量的比例来支付工资，每名工人至少应得到其最低期望工资。求组成满足条件的付费群体所需的最小金额。

**实现思路**：首先按照工资与工作质量的比例对工人进行排序，然后使用最小堆来维护一个大小为 k 的堆，堆中存放的是负的工作质量。初始化堆时，将前 k 名工人的质量放入堆中。计算当前堆中所有工人的总质量，并计算以当前工资标准下的总支付金额。然后遍历剩余的工人，每次将堆顶的质量弹出，加上新的工人的质量，计算新的总质量，并更新总支付金额。找出所有情况下的最小总支付金额即可。

```py
class Solution:
    def mincostToHireWorkers(self, quality: List[int], wage: List[int], k: int) -> float:
        emp = sorted(zip(wage, quality), key = lambda e:e[0]/e[1])
        h = [-q for _, q in emp[:k]]
        heapify(h)
        sum_q = -sum(h)
        ans = sum_q*(emp[k-1][0]/emp[k-1][1])

        for w, q in emp[k:]:
            sum_q += heapreplace(h, -q) + q 
            ans = min(ans, sum_q*w/q)
        
        return ans 
```

