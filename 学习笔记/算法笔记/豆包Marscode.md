---
title : '豆包Marscode'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "total_sum = sum(sums)"
image : img/cat.jpg
draft : false
categories : ["算法笔记"]
tags : ["学习笔记", "算法笔记"]
---
# 豆包Marscode

## 数字分组求偶数和





## [和的逆运算问题](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(n, sums):
    # 计算所有和的总和
    total_sum = sum(sums)

    # 如果总和不能被 (n - 1) 整除，说明无解
    if total_sum % (n - 1) != 0:
        return "Impossible"

    # 初始化结果数组
    ans = [0] * n
    # 对和数组进行排序
    sums.sort()

    # 尝试从 sums[0] 的绝对值范围内选择一个数作为 ans[0]
    for i in range(-abs(sums[0]), abs(sums[0]) + 1):
        ans[0] = i
        # 创建一个字典来记录每个和的出现次数
        p = {s: sums.count(s) for s in sums}  
        idx = 1

        # 遍历字典中的每个和
        for k in p.keys():
            # 当某个和的出现次数大于0时
            while p[k] > 0:
                # 假设 ans[idx] 是当前和减去 ans[0]
                ans[idx] = k - ans[0]

                t = 0
                # 检查当前假设的 ans[idx] 是否与其他已确定的数相加后在 sums 中存在
                while t < idx:
                    if p.get(ans[idx] + ans[t], 0):
                        p[ans[idx] + ans[t]] -= 1
                    else:
                        break
                    t += 1

                # 如果所有已确定的数都满足条件，继续下一个数
                if t == idx:
                    idx += 1
                else:
                    break

        # 如果成功找到了 n 个数，跳出循环
        if idx == n:
            break

    # 返回结果
    return " ".join(map(str, ans[:]))

```

## [二叉树供暖问题](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(nodes):
    # 给定的节点列表为空时，直接返回0
    if not nodes or nodes[0] == 0:
        return 0
    
    # 将层次遍历的列表转化为二叉树
    from collections import deque
    
    class TreeNode:
        def __init__(self, val=0):
            self.val = val
            self.left = None
            self.right = None
    
    def build_tree(nodes):
        if not nodes:
            return None
        root = TreeNode(nodes[0])
        queue = deque([root])
        i = 1
        while i < len(nodes):
            node = queue.popleft()
            if i < len(nodes) and nodes[i] == 1:
                node.left = TreeNode(1)
                queue.append(node.left)
            i += 1
            if i < len(nodes) and nodes[i] == 1:
                node.right = TreeNode(1)
                queue.append(node.right)
            i += 1
        return root
    
    root = build_tree(nodes)

    # 用一个全局变量来记录所需的供暖器数量
    heaters = 0

    # 后序遍历 + 贪心策略
    def postorder(node):
        if not node:
            return 1  # 空节点视为已被覆盖
        
        left = postorder(node.left)
        right = postorder(node.right)
        
        # 如果左右子节点有一个需要供暖，则当前节点必须安装供暖器
        if left == -1 or right == -1:
            nonlocal heaters
            heaters += 1
            return 0  # 当前节点已安装供暖器
        
        # 如果左右子节点有一个安装了供暖器，则当前节点已被覆盖
        if left == 0 or right == 0:
            return 1  # 当前节点已被覆盖，但没有安装供暖器
        
        # 如果左右子节点都已被覆盖且没有安装供暖器，则当前节点需要供暖
        return -1
    
    # 如果根节点需要供暖，必须在根节点上安装供暖器
    if postorder(root) == -1:
        heaters += 1
    
    return heaters

# 测试用例
if __name__ == "__main__":
    print(solution([1, 1, 0, 1, 1]) == 1)  # 输出1
    print(solution([1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1]) == 3)  # 输出3
    print(solution([1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0, 0, 1]) == 3)  # 输出3
```

## [简单四则运算解析器](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(expression):
    def evaluate(tokens):
        # 初始化操作数栈和运算符栈
        operands = []
        operators = []
        
        for token in tokens:
            if token.isdigit():
                # 处理数字
                operands.append(int(token))
            elif token in "+-*/":
                # 处理运算符
                while operators and precedence(operators[-1]) >= precedence(token):
                    apply_operator(operands, operators)
                operators.append(token)
            elif token == '(':
                # 处理左括号
                operators.append(token)
            elif token == ')':
                # 处理右括号
                while operators and operators[-1] != '(':
                    apply_operator(operands, operators)
                operators.pop()  # 弹出左括号
        
        # 处理剩余的运算符
        while operators:
            apply_operator(operands, operators)
        
        return operands[0]
    
    def precedence(op):
        # 定义运算符优先级
        if op in "+-":
            return 1
        if op in "*/":
            return 2
        return 0
    
    def apply_operator(operands, operators):
        # 应用运算符
        op = operators.pop()
        b = operands.pop()
        a = operands.pop()
        if op == '+':
            operands.append(a + b)
        elif op == '-':
            operands.append(a - b)
        elif op == '*':
            operands.append(a * b)
        elif op == '/':
            operands.append(a // b)  # 整数除法
    
    # 将表达式拆分为tokens
    tokens = []
    i = 0
    while i < len(expression):
        if expression[i].isdigit():
            num = 0
            while i < len(expression) and expression[i].isdigit():
                num = num * 10 + int(expression[i])
                i += 1
            tokens.append(str(num))
        else:
            tokens.append(expression[i])
            i += 1
    
    return evaluate(tokens)
```

## [最大乘积问题](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(n, array):
    # 初始化 L 和 R 数组
    L = [0] * n
    R = [0] * n
    
    # 使用栈计算 L(i)
    stack = []
    for i in range(n):
        while stack and array[stack[-1]] <= array[i]:
            stack.pop()
        if stack:
            L[i] = stack[-1] + 1  # 因为题目中 i 从 1 开始
        stack.append(i)
    
    # 使用栈计算 R(i)
    stack = []
    for i in range(n-1, -1, -1):
        while stack and array[stack[-1]] <= array[i]:
            stack.pop()
        if stack:
            R[i] = stack[-1] + 1  # 因为题目中 i 从 1 开始
        stack.append(i)
    
    # 计算 MAX(i) 并找到最大值
    max_product = 0
    for i in range(n):
        max_product = max(max_product, L[i] * R[i])
    
    return max_product
```

## [最优硬币组合问题](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(coins, amount):
    # 动态规划数组，初始值为正无穷，f[0] = 0 表示金额为 0 时不需要硬币
    f = [float('inf')] * (amount + 1)
    f[0] = 0
    
    # 用每个硬币面值去更新f数组
    for coin in coins:
        for i in range(coin, amount + 1):
            f[i] = min(f[i], f[i - coin] + 1)
    
    # 如果 f[amount] 仍然是无穷大，表示无法凑出该金额
    if f[amount] == float('inf'):
        return []
    
    # 回溯找出使用的硬币组合
    result = []
    remaining = amount
    while remaining > 0:
        for coin in coins:
            if remaining >= coin and f[remaining] == f[remaining - coin] + 1:
                result.append(coin)
                remaining -= coin
                break
    
    result.sort(reverse = True)
    return result
```

## [价格优惠计算问题](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(N: int, p: list) -> int:
    # 初始化总优惠
    total_discount = 0
    # 初始化栈
    stack = []
    
    # 遍历价格数组
    for price in p:
        # 当栈不为空且栈顶元素大于当前价格时，弹出栈顶元素
        while stack and stack[-1] > price:
            stack.pop()
        
        # 如果栈不为空，说明栈顶元素是最近的满足条件的商品
        if stack:
            total_discount += stack[-1]
        
        # 将当前价格压入栈中
        stack.append(price)
    
    return total_discount
```

## [小A的子数组权值](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
def solution(n: int, a: list) -> list:
    c = [0 for _ in range(n + 1)]
    
    # 遍历所有可能的子数组
    for i in range(n):
        # 使用字典记录当前子数组中每个元素的出现次数
        count_dict = {}
        distinct_count = 0
        
        for j in range(i, n):
            # 更新当前元素的出现次数
            if a[j] in count_dict:
                count_dict[a[j]] += 1
            else:
                count_dict[a[j]] = 1
                distinct_count += 1
            
            # 更新权值为 distinct_count 的子数组数量
            if distinct_count <= n:
                c[distinct_count] += 1
    
    return c[1:]
```

## [组队](https://www.marscode.cn/practice/65621ew1pe38dj)

```py
from collections import defaultdict
from itertools import combinations

def solution(n: int, k: int, a: list, b: list) -> int:
    # 如果 n == 1，直接判断是否能形成 k 个组
    if n == 1:
        return 0

    def calculate_diff(i, j):
        return abs(a[i] - a[j]) + abs(b[i] - b[j])

    def can_divide_with_L(L):
        # 构建图：差别值小于等于 L 的人连接在同一组
        adj = defaultdict(list)
        for i, j in combinations(range(n), 2):
            if calculate_diff(i, j) <= L:
                adj[i].append(j)
                adj[j].append(i)
        
        # 深度优先搜索找连通分量
        visited = [False] * n
        def dfs(node):
            stack = [node]
            while stack:
                u = stack.pop()
                for v in adj[u]:
                    if not visited[v]:
                        visited[v] = True
                        stack.append(v)
        
        # 统计连通分量
        components = 0
        for i in range(n):
            if not visited[i]:
                visited[i] = True
                dfs(i)
                components += 1
        return components >= k

    # 二分查找最大化 L
    left, right = 0, max(calculate_diff(i, j) for i in range(n) for j in range(i+1, n))
    answer = 0

    while left <= right:
        mid = (left + right) // 2
        if can_divide_with_L(mid):
            answer = mid  # 更新可行解
            left = mid + 1  # 尝试更大的 L
        else:
            right = mid - 1  # 尝试更小的 L

    return answer
```

