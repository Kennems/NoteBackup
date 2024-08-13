# Python 性能优化

[python 高性能编程](https://kevinng77.github.io/posts/notes/articles/%E7%AC%94%E8%AE%B0python_advanced.html)

[Python代码性能优化方法总结](https://hanfeng.ink/post/python_performance/)

```py
standard_input, packages, output_together = 1, 1, 0  # 设置标志来控制输入输出方式和是否导入所需的包
dfs, hashing, read_from_file = 0, 0, 0  # 设置标志来决定是否启用深度优先搜索和哈希等功能
de = 1  # 设置调试标志

if 1:  # 这是一个占位的 if 语句，无实际作用

    if standard_input:  # 如果标准输入为真，则使用标准输入方式
        import io, os, sys
        input = lambda: sys.stdin.readline().strip()  # 重定向输入函数为标准输入
        import math
        inf = math.inf  # 设置无穷大值

        def I():  # 读取字符串输入
            return input()

        def II():  # 读取整数输入
            return int(input())

        def MII():  # 读取多个整数输入并以元组形式返回
            return map(int, input().split())

        def LI():  # 读取以空格分隔的字符串列表输入
            return list(input().split())

        def LII():  # 读取以空格分隔的整数列表输入
            return list(map(int, input().split()))

        def LFI():  # 读取以空格分隔的浮点数列表输入
            return list(map(float, input().split()))

        def GMI():  # 读取以空格分隔的整数列表输入，并将每个值减1
            return map(lambda x: int(x) - 1, input().split())

        def LGMI():  # 读取以空格分隔的整数列表输入，并将每个值减1并返回为列表
            return list(map(lambda x: int(x) - 1, input().split()))

    if packages:  # 如果导入包的标志为真，则导入所需的包
        from io import BytesIO, IOBase
        import random
        import os
        import bisect
        import typing
        from collections import Counter, defaultdict, deque
        from copy import deepcopy
        from functools import cmp_to_key, lru_cache, reduce
        from heapq import merge, heapify, heappop, heappush, heappushpop, nlargest, nsmallest
        from itertools import accumulate, combinations, permutations, count, product
        from operator import add, iand, ior, itemgetter, mul, xor
        from string import ascii_lowercase, ascii_uppercase, ascii_letters
        from typing import *
        BUFSIZE = 4096  # 设置缓冲区大小

    if output_together:  # 如果输出合并标志为真，则设置快速输入输出函数
        class FastIO(IOBase):
            newlines = 0

            def __init__(self, file):
                self._fd = file.fileno()
                self.buffer = BytesIO()
                self.writable = "x" in file.mode or "r" not in file.mode
                self.write = self.buffer.write if self.writable else None

            def read(self):
                while True:
                    b = os.read(self._fd, max(os.fstat(self._fd).st_size, BUFSIZE))
                    if not b:
                        break
                    ptr = self.buffer.tell()
                    self.buffer.seek(0, 2), self.buffer.write(b), self.buffer.seek(ptr)
                self.newlines = 0
                return self.buffer.read()

            def readline(self):
                while self.newlines == 0:
                    b = os.read(self._fd, max(os.fstat(self._fd).st_size, BUFSIZE))
                    self.newlines = b.count(b"\n") + (not b)
                    ptr = self.buffer.tell()
                    self.buffer.seek(0, 2), self.buffer.write(b), self.buffer.seek(ptr)
                self.newlines -= 1
                return self.buffer.readline()

            def flush(self):
                if self.writable:
                    os.write(self._fd, self.buffer.getvalue())
                    self.buffer.truncate(0), self.buffer.seek(0)

        class IOWrapper(IOBase):
            def __init__(self, file):
                self.buffer = FastIO(file)
                self.flush = self.buffer.flush
                self.writable = self.buffer.writable
                self.write = lambda s: self.buffer.write(s.encode("ascii"))
                self.read = lambda: self.buffer.read().decode("ascii")
                self.readline = lambda: self.buffer.readline().decode("ascii")

        sys.stdout = IOWrapper(sys.stdout)

    if dfs:  # 如果深度优先搜索标志为真，则定义一个装饰器来实现递归优化
        from types import GeneratorType

        def bootstrap(f, stack=[]):
            def wrappedfunc(*args, **kwargs):
                if stack:
                    return f(*args, **kwargs)
                else:
                    to = f(*args, **kwargs)
                    while True:
                        if type(to) is GeneratorType:
                            stack.append(to)
                            to = next(to)
                        else:
                            stack.pop()
                            if not stack:
                                break
                            to = stack[-1].send(to)
                    return to
            return wrappedfunc

    if hashing:  # 如果哈希标志为真，则生成一个随机数作为哈希种子
        RANDOM = random.getrandbits(20)
        class Wrapper(int):
            def __init__(self, x):
                int.__init__(x)

            def __hash__(self):
                return super(Wrapper, self).__hash__() ^ RANDOM

    if read_from_file:  # 如果从文件读取标志为真，则设置输入和输出文件
        file = open("input.txt", "r").readline().strip()[1:-1]
        fin = open(file, 'r')
        input = lambda: fin.readline().strip()
        output_file = open("output.txt", "w")
        def fprint(*args, **kwargs):
            print(*args, **kwargs, file=output_file)

    if de:  # 如果调试标志为真，则定义调试函数
        def debug(*args, **kwargs):
            print('\033[92m', end='')  # 设置输出颜色为绿色
            print(*args, **kwargs)  # 打印调试信息
            print('\033[0m', end='')  # 恢复输出颜色


```

