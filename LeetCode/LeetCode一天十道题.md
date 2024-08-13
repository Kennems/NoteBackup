# LeetCode一天十道题

# 做十道题，复习十道题

## 20240606

[274. H 指数](https://leetcode.cn/problems/h-index/)

[45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

### [135. 分发糖果](https://leetcode.cn/problems/candy/)

[14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

[13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

[26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

[12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/)

[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/) （ 单调栈解法）

[58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

[151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)



## 20240607

### [68. 文本左右对齐](https://leetcode.cn/problems/text-justification/)

```py
class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        n = len(words)
        res = []
        cur, curl, cnt = [], 0, 0
        i = 0
        
        while i<n:
            if curl + len(words[i]) + cnt <= maxWidth:
                curl += len(words[i])
                cur.append(words[i])
                cnt += 1
                i += 1
            else:
                if cnt==1:
                    row = cur[0] + " "*(maxWidth - len(cur[0]))
                    res.append(row)
                else:
                    wordlen = sum(len(wd) for wd in cur)
                    spaceNum, rest = (maxWidth-wordlen)//(cnt-1), (maxWidth-wordlen)%(cnt-1)
                    row = ""
                    for j in range(cnt-1):
                        row = row + cur[j] + " "*spaceNum + (" " if rest>0 else "")
                        rest -= 1
                    row += cur[-1]
                    res.append(row)

                cur, curl, cnt = [], 0, 0

        if cnt==1:
            row = cur[0] + " "*(maxWidth - len(cur[0]))
            res.append(row)
        else:
            row = ""
            for j in range(cnt-1):
                row = row + cur[j] + " "
            row += cur[-1]

            wordlen = sum(len(wd) for wd in cur)
            wordlen += (cnt-1)
            row += " "*(maxWidth-wordlen)

            res.append(row)

        return res
```

[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

[122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

[55. 跳跃游](https://leetcode.cn/problems/jump-game/)

[380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/)

[238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

[28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

[30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)

```py
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        n, m, l = len(words), len(words[0]), len(s)
        t = n*m
        res = []

        for i in range(m):
            c = defaultdict(int)
            for j in range(i, i+t, m):
                wd = s[j:j+m]
                c[wd] += 1
            for wd in words:
                c[wd] -= 1
                if c[wd]==0:
                    del c[wd]
            
            if len(c)==0:
                res.append(i) 
            for st in range(i + m, l-t+1, m):
                wd = s[st + (n-1)*m: st + n*m]
                c[wd] += 1
                if c[wd]==0:
                    del c[wd]
                
                wd = s[st-m: st]
                c[wd] -= 1
                if c[wd] == 0:
                    del c[wd]
                
                if len(c) == 0:
                    res.append(st)
        return res
```

[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/) （ 双指针解法）





## 20240608

[66. 加一](https://leetcode.cn/problems/plus-one/)

[172. 阶乘后的零](https://leetcode.cn/problems/factorial-trailing-zeroes/)

[201. 数字范围按位与](https://leetcode.cn/problems/bitwise-and-of-numbers-range/)

[191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)

[190. 颠倒二进制位](https://leetcode.cn/problems/reverse-bits/)

[67. 二进制求和](https://leetcode.cn/problems/add-binary/)

[373. 查找和最小的 K 对数字](https://leetcode.cn/problems/find-k-pairs-with-smallest-sums/)

### [502. IPO](https://leetcode.cn/problems/ipo/)

[153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

[162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)



## 20240609 周赛





## 20240610





## 20240611

[392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

[15. 三数之和](https://leetcode.cn/problems/3sum/)

[209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

[30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)

[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

[36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)

[73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

[289. 生命游戏](https://leetcode.cn/problems/game-of-life/)

[383. 赎金信](https://leetcode.cn/problems/ransom-note/)

[205. 同构字符串](https://leetcode.cn/problems/isomorphic-strings/)

[202. 快乐数](https://leetcode.cn/problems/happy-number/)

### [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

```py
class DLinkedNode:
    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None


class LRUCache:

    def __init__(self, capacity: int):
        self.cache = dict()

        # 使用伪头部和伪尾部节点    
        self.head = DLinkedNode()
        self.tail = DLinkedNode()

        self.head.next = self.tail
        self.tail.prev = self.head

        self.capacity = capacity
        self.size = 0

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        # 如果 key 存在，先通过哈希表定位，再移到头部
        node = self.cache[key]
        self.moveToHead(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            # 如果 key 不存在，创建一个新的节点
            node = DLinkedNode(key, value)
            # 添加进哈希表
            self.cache[key] = node
            # 添加至双向链表的头部
            self.addToHead(node)
            self.size += 1
            if self.size > self.capacity:
                # 如果超出容量，删除双向链表的尾部节点
                removed = self.removeTail()
                # 删除哈希表中对应的项
                self.cache.pop(removed.key)
                self.size -= 1
        else:
            # 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            node = self.cache[key]
            node.value = value
            self.moveToHead(node)

    def removeNode(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev

    def moveToHead(self, node):
        self.removeNode(node)
        self.addToHead(node)

    def addToHead(self, node):
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    def removeTail(self):
        node = self.tail.prev
        self.removeNode(node)
        return node

```

## 20240612

[290. 单词规律](https://leetcode.cn/problems/word-pattern/)

[242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

[219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

[57. 插入区间](https://leetcode.cn/problems/insert-interval/)

[452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

[150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

[224. 基本计算器](https://leetcode.cn/problems/basic-calculator/)

[138. 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/)

```py
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        d = dict()
        d[None] = None
        cur = dummy = Node(-1)

        while head:
            if head not in d:
                d[head] = Node(head.val)
            if head.random not in d:
                d[head.random] = Node(head.random.val)
            
            cur.next = d[head]
            cur.next.random = d[head.random]
            cur = cur.next

            head = head.next
        
        return dummy.next
```

[61. 旋转链表](https://leetcode.cn/problems/rotate-list/)

[86. 分隔链表](https://leetcode.cn/problems/partition-list/)

[25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        def reverse(head, tail):
            cur = tail.next
            dummy = head
            while cur != tail:
                nxt = dummy.next
                dummy.next = cur
                cur = dummy
                dummy = nxt
            return tail, head
            
        hair = ListNode(-1)
        hair.next = head
        pre = hair

        while head:
            tail = pre

            for i in range(k):
                tail = tail.next
                if not tail:
                    return hair.next
                    
            head, tail = reverse(head, tail)
            pre.next = head

            pre = tail
            head = tail.next

        return hair.next
            
            
```



## 20240613

[4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

[153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

[33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

[162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)

[23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)
