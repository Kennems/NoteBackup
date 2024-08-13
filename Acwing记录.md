# Acwing记录

易错点：

1. 变量名，符号写错（已知算法的情况下重新写一遍）
2. 函数写了没有调用

保分

根据数据量用不同的算法





DP， 数学，哈希，分块，树状数组线段树

```py
def check(x):
    if x<2:
        return False
    for i in range(2, int(x**0.5) + 1):
        if x%i==0:
            return False
    return True

n = int(input())
for i in range(n):
    x = int(input())
    if check(x):
        print("Yes")
    else:
        print("No")
```



```py
def get(x):
    for i in range(2, int(x**0.5) + 1):
        cnt = 0
        if x%i==0:
            while x%i==0:
                x//=i
                cnt+=1
        if cnt:
            print(i, cnt)
    if x!=1:
        print(x, 1)
            

n = int(input())
for i in range(n):
    x = int(input())
    get(x)
    print()
```

### [阶乘分解](https://www.acwing.com/problem/content/description/199/)

```py
def get(n):
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p]=True
            if i%p==0:
                break
    

n = int(input())
primes = []
st = [False]*(n+1)

get(n)

for p in primes:
    s, t = 0, n
    
    while t:
        s += t//p
        t//=p
    
    print(p, s)
```



### [\196. 质数距离](https://www.acwing.com/problem/content/description/198/)

```py
import math
inf = math.inf

def get(n):
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p] = True
            if i%p==0:
                break
    
N = 50000
primes = []
st = [False]*(N + 5)
get(N)

while True:
    try:
        l, r = map(int, input().split())
    except:
        exit()
    
    isprimes = [False]*(r-l+1)
    inprimes = []
    for p in primes:
        for i in range(max(2*p, (l+p-1)//p*p), r+1, p):
            isprimes[i-l] = True
    
    for i in range(l, r+1):
        if i>1 and not isprimes[i-l]:
            inprimes.append(i)
    
    if len(inprimes)<2:
        print("There are no adjacent primes.")
        continue
    
    mx, mxl, mxr = -inf, 0, 0
    mn, mnl, mnr = inf, 0, 0
    for i in range(len(inprimes)-1):
        p1, p2 = inprimes[i], inprimes[i+1]
        
        d = p2 - p1
        if d<mn:
            mn, mnl, mnr = d, p1, p2
        if d>mx:
            mx, mxl, mxr = d, p1, p2
    
    print("{},{} are closest, {},{} are most distant.".format(mnl, mnr, mxl, mxr))
            


```



### AcWing 1293. 夏洛克和他的女朋友

```py
def get(n):
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p] = True
            if i%p==0:
                break

N = 100010
primes = []
st = [False]*(N+10)
get(N)

n = int(input())

if n<=2:
    print(1)
else:
    print(2)

for i in range(2, n+2):
    if not st[i]:
        print(1, end=' ')
    else:
        print(2, end=' ')
    
```

分解约数

```py
def get(x):
    res = []
    for i in range(1, int(x**0.5) + 1):
        if x%i==0:
            res.append(i)
            if i!=x//i:
                res.append(x//i)
    return res
            

n = int(input())

for i in range(n):
    x = int(input())
    res = sorted(get(x))
    print(*res, sep=' ')
```



```py
from collections import defaultdict
mod = int(1e9) + 7

n = int(input())    
res = 1 
d = defaultdict(int)


for i in range(n):
    x = int(input())

    for i in range(2, int(x**0.5) + 1):    
        cnt = 0
        if x%i==0:
            while x%i==0:
                x//=i
                cnt += 1
        d[i] += cnt
    if x>1:
        d[x] += 1

for x in d:  
    res = res * (d[x]+1) % mod
        
print(res)
                
```



### 约数之和

```py
from collections import defaultdict
d = defaultdict(int)
mod = 9901

def qpow(a, k):
    res = 1
    while k:
        if k&1:
            res = res*a%mod
        a = a*a%mod
        k>>=1
    return res
    
def calc(a, k):
    if k==0:
        return 1
    if k&1:
        return (1 + qpow(a, k//2 + 1)) * calc(a, k//2) % mod
    
    return (1 + qpow(a, (k-1)//2 + 1)) * calc( a, (k-1)//2 ) + qpow(a, k)


a, b = map(int, input().split())
x = a
for i in range(2, int(x**0.5) + 1):
    if x%i==0:
        cnt = 0
        while x%i==0:
            x//=i
            cnt += 1
        d[i] += cnt

if x>1:
    d[x] += 1
    
res = 1
for x in d:
    res = res * calc(x, d[x]*b) % mod

print(res if a else 0)
```



### 约数之和

```py
from collections import defaultdict

def qpow(a, k):
    res = 1
    while k:
        if k&1:
            res = res*a%mod
        a = a*a%mod
        k>>=1
    return res

def calc(a, k):
    if k==0:
        return 1
    
    if k&1:
        return (1 + qpow(a, k//2+1)) * calc(a, k//2) % mod
        
    return ( qpow(a, k) + (1 + qpow(a, (k-1)//2+1)) * calc(a, (k-1)//2) ) % mod

n = int(input())
mod = int(1e9) + 7
d = defaultdict(int)

for i in range(n):
    x = int(input())
    
    for j in range(2, int(x**0.5) + 1):
        if x%j==0:
            cnt = 0
            while x%j==0:
                cnt += 1
                x//=j
            d[j] += cnt
    
    if x>1:
        d[x] += 1
        
res = 1
for x in d:
    res = res * calc(x, d[x])%mod

print(res)
    
```



### 分解质因数法求组合数

```py
def get_primes(n):
    primes = []
    st = [False]*(n+1)
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p] = True
            if i%p==0:
                break
    return primes

def get(n, p):
    res = 0
    while n:
        res += n//p
        n//=p
    return res

def qpow(a, k):
    res = 1
    while k:
        if k&1:
            res = res * a
        a = a*a
        k>>=1
    return res

def calc(a, b):
    primes = get_primes(a)
    s = [0]*len(primes)
    for i, p in enumerate(primes):
        s[i] = get(a, p) - get(b, p) - get(a-b, p)
    res = 1
    for i in range(len(primes)):
        res = res * qpow(primes[i], s[i])
    return res

a, b = map(int, input().split())
print(calc(a, b))

```

### Kruskal 

```py
import sys
input = lambda:sys.stdin.readline().strip()

def find(x):
    while p[x] != x:
        p[x] = p[p[x]]
        x = p[x]
    return x

def kruskal():
    res, cnt = 0, 0
    ans = []
    for u, v, w in g:
        fu, fv = find(u), find(v)
        if fu != fv:
            res += w
            cnt += 1
            p[fu] = fv
            ans.append((u, v, w))
    
    ans.sort()
    if cnt == n-1:
        print(res)
        # print(ans)
    else:
        print("impossible")
    
n, m = map(int, input().split())
g = []
p = [i for i in range(n+1)]
for i in range(m):
    u, v, w = map(int, input().split())
    g.append((u, v, w))

g.sort(key = lambda e:e[2])

kruskal()
```



### prim

```py
from math import inf
from sys import stdin

def prim():
    res = 0
    dis[1] = 0
    for i in range(n):
        t = -1
        for j in range(1, n+1):
            if not st[j] and (t==-1 or dis[t] > dis[j]):
                t = j
        if i and dis[t]==inf:
            return inf
        if i:
            res += dis[t]
        st[t] = True

        for j in range(1, n+1):
            dis[j] = min(dis[j], g[t][j])
    return res
    

n, m = map(int, input().split())
g = [[inf]*(n+1) for _ in range(n+1)]
dis = [inf]*(n+1)
st = [False]*(n+1)

for i in range(m):
    u, v, w = map(int, input().split())
    g[u][v] = g[v][u] = min(w, g[u][v])

res = prim()
if res==inf:
    print("impossible")
else:
    print(res)
```

### floyd

```py
from sys import stdin
from math import inf

def floyd():
    for k in range(1, n+1):
        for i in range(1, n+1):
            for j in range(1, n+1):
                d[i][j] = min(d[i][j], d[i][k] + d[k][j])

n, m, k = map(int, input().split())
d = [[0 if i==j else inf for i in range(n+1)] for j in range(n+1)]

for i in range(m):
    x, y, z = map(int, input().split())
    d[x][y] = min(d[x][y], z)
    
floyd()
    
for i in range(k):
    x, y = map(int, input().split())
    print(d[x][y] if d[x][y]!=inf else "impossible")
```

### spfa

```PY
from sys import stdin
from collections import defaultdict, deque
from math import inf

def spfa():
    q = deque([1])
    dis[1] = 0
    st[1] = True
    while q:
        x = q.popleft()
        st[x] = False
        for y, z in g[x]:
            if dis[y] > dis[x] + z:
                dis[y] = dis[x] + z
                if not st[y]:
                    q.append(y)
                    st[y] = True
    return dis[n]


n, m = map(int, input().split())
dis = [inf]*(n+1)
st = [False]*(n+1)
g = defaultdict(list)

for i in range(m):
    x, y, z = map(int, input().split())
    g[x].append((y, z))
    
res = spfa()
print(res if res != inf else "impossible")
```



### SPFA判断负环

```py
from sys import stdin
from collections import deque, defaultdict
from math import inf

input = lambda:stdin.readline().strip()

def spfa():
    q = deque()
    for i in range(1, n+1):
        q.append(i)
        st[i] = True
    while q:
        x = q.popleft()
        st[x] = False
        for y, z in g[x]:
            if dis[y] > dis[x] + z:
                dis[y] = dis[x] + z
                cnt[y] = cnt[x] + 1
                if cnt[y] >= n:
                    return True
                if not st[y]:
                    q.append(y)
                    st[y] = True
    return False

n, m = map(int, input().split())

g = defaultdict(list)
cnt = [0]*(n+1)
st = [False]*(n+1)
dis = [0]*(n+1)

for i in range(m):
    x, y, z = map(int, input().split())
    g[x].append((y, z))

res = spfa()

print("Yes" if res else "No")
```



### 二分图

```py
from sys import stdin
from collections import defaultdict, deque

def bfs(u, c):
    color[u] = c
    q = deque([(u, c)])
    while q:
        cur, col = q.popleft()
        for nx in g[cur]:
            if color[nx]==-1:
                color[nx] = col^1
                q.append((nx, col^1))
            elif color[nx] == col:
                return False
    return True

n, m = map(int, input().split())
g = defaultdict(list)
color = [-1]*(n+1)

for i in range(m):
    u, v = map(int, input().split())
    if u not in g[v]:
        g[v].append(u)
    if v not in g[u]:
        g[u].append(v)
    
for i in range(1, n+1):
    if color[i] == -1:
        if not bfs(i, 0):
            print("No")
            exit()

print("Yes")
```

### 匈牙利算法

```py
from sys import stdin
from collections import defaultdict

def find(u):
    for v in g[u]:
        if not st[v]:
            st[v] = True
            if mat[v]==0 or find(mat[v]):
                mat[v] = u
                return True
    return False
            

n1, n2, m = map(int, input().split())
mat = [0]*(n2 + 1)
g = defaultdict(list)

for i in range(m):
    u, v = map(int, input().split())
    g[u].append(v)
    
    
res = 0
for i in range(1, n1+1):
    st = [False]*(n2 + 1)
    if find(i):
        res += 1
print(res)
```

### TopSort

```py
from sys import stdin
from collections import deque

input = lambda:stdin.readline().strip()

def topsort():
    q = deque()
    res = []
    for i in range(1, n+1):
        if ind[i]==0:
            res.append(i)
            q.append(i)
    
    while q:
        u = q.popleft()
        for v in g[u]:
            ind[v] -= 1
            if ind[v]==0:
                q.append(v)
                res.append(v)
    
    return res
    
    
    
n = int(input())
ind = [0]*(n+1)
g = [[] for _ in range(n+1)]


for i in range(1, n+1):
    a = list(map(int, input().split()))
    for j in range(len(a)-1):
        g[i].append(a[j])
        ind[a[j]]+=1
    
res = topsort()
print(*res, sep=' ')
```

### Dijkstra

```py
from math import inf
from sys import stdin
from collections import deque

def dijkstra():
    dis[1] = 0
    # st[1] = True
    for i in range(n-1):
        t = -1
        for j in range(1, n+1):
            if not st[j] and (t==-1 or dis[j] < dis[t]):
                t = j  
        st[t] = True
        for j in range(1, n+1):
            dis[j] = min(dis[j], dis[t] + g[t][j])
        
    return dis[n]
    
    
    
n, m = map(int, input().split())
g = [[inf]*(n+1) for _ in range(n+1)]
st = [False]*(n+1)
dis = [inf]*(n+1)

for i in range(m):
    x, y, z = map(int, input().split())
    g[x][y] = min(g[x][y], z)

res =dijkstra()

print(res if res != inf else -1)
```





```py
from sys import stdin
from math import inf 
from collections import defaultdict, deque

def spfa():
    q = deque([1])
    dis[1] = 0
    st[1] = True
    while q:
        u = q.popleft()
        st[u] = False
        for v, z in g[u]:
            if dis[v] > dis[u] + z:
                dis[v] = dis[u] + z
                if not st[v]:
                    st[v] = True
                    q.append(v)
    return dis[n]

n, m = map(int, input().split())
g = defaultdict(list)
dis = [inf]*(n+1)
st = [False]*(n+1)

for i in range(m):
    x, y, z = map(int, input().split())
    g[x].append((y, z))


res = spfa()

print(res if res != inf else "impossible")
```

### 热浪

```py
from sys import stdin
from collections import deque, defaultdict
from math import inf
input = lambda:stdin.readline().strip()

def spfa():
    q = deque([st])
    dis[st] = 0
    vis[st] = True
    while q:
        x = q.popleft()
        vis[x] = False
        for y, z in g[x]:
            if dis[y] > dis[x] + z:
                dis[y] = dis[x] + z
                if not vis[y]:
                    vis[y] = True
                    q.append(y)
    
    return dis[ed]

n, m, st, ed = map(int, input().split())
g = defaultdict(list)
dis = [inf]*(n+1)
vis = [False]*(n+1)

for i in range(m):
    x, y, z = map(int, input().split())
    g[x].append((y, z))
    g[y].append((x, z))

res = spfa()

print(res)
```

### 堆优化版Dijkstra()

```py
from sys import stdin
from math import inf
from collections import deque, defaultdict
from heapq import heappush, heappop
input = lambda:stdin.readline().strip()

def dijkstra():
    h = [(0, 1)]
    dis[1] = 0
    
    while h:
        dist, ver = heappop(h)
        if vis[ver]:
            continue
        vis[ver] = True
        for nx, z in g[ver]:
            if dis[nx] > dis[ver] + z:
                dis[nx] = dis[ver] + z
                heappush(h, (dis[nx], nx))
                
    return dis[n]
    

n, m = map(int, input().split())
dis = [inf]*(n+1)
vis = [False]*(n+1)
g = defaultdict(list)

for i in range(m):
    x, y, z = map(int, input().split())
    g[x].append((y, z))

res = dijkstra()
print(res if res != inf else -1)

```

### 1275 最大数

```py
from sys import stdin
input = lambda:stdin.readline().strip()

def pushup(u):
    tr[u] = max(tr[u<<1], tr[u<<1|1])

def build(u, l, r):
    if l==r:
        tr[u] = 0
    else:
        mid = l+r>>1
        build(u<<1, l, mid)
        build(u<<1|1, mid+1, r)
        pushup(u)

def modify(u, l, r, x, v):
    if l==r:
        tr[u] = v
        return 
    mid = l+r>>1
    if x<=mid:
        modify(u<<1, l, mid, x, v)
    else:
        modify(u<<1|1, mid+1, r, x, v)
    pushup(u)
    
    
def query(u, l, r, ql, qr):
    if ql<=l and r<=qr:
        return tr[u]
    mid = l+r>>1
    res = 0
    if ql<=mid:
        res = query(u<<1, l, mid, ql, qr)
    if qr>mid:
        res = max(res, query(u<<1|1, mid+1, r, ql, qr))
    
    return res

m, p = map(int, input().split())
tr = [0]*(1<<m.bit_length() + 1)
pre = 0
cnt = 0

for i in range(m):
    op, x = input().split()
    x = int(x)

    if op=='Q':    
        l, r = cnt-x+1, cnt
        pre = query(1, 1, m, l, r)
        print(pre)
    else:
        cnt += 1
        val = (x + pre)%p
        modify(1, 1, m, cnt, val)


        
```

### 反素数

```py
from sys import stdin
input = lambda:stdin.readline().strip()

def get(n):
    primes = []
    st = [False]*(n+1)
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p] = True
            if i%p==0:
                break
    return primes
    
    
def dfs(i, od, p, cnt):
    global mxp, mxn
    if cnt>mxn or (cnt==mxn and p<mxp):
        mxp, mxn = p, cnt
    if i>10:
        return 
    for j in range(1, od+1):
        if p*primes[i]>n:
            return 
        p *= primes[i]
        dfs(i+1, j, p, cnt*(j+1))

primes = get(50)
# print(primes)
n = int(input())
mxp, mxn = 0, 0
dfs(0, 30, 1, 1)

print(mxp)

```



```py
from sys import stdin

def get(n):
    primes = []
    st = [False]*(n+1)
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p] = True
            if i%p==0:
                break
    return primes

def dfs(i, od, p, cnt):
    global mxn, mxp
    if cnt>mxn or (cnt==mxn and p<mxp):
        mxp, mxn = p, cnt
    if i>10:
        return 
    for j in range(1, od+1):
        if p * primes[i] > n:
            break
        p *= primes[i]
        dfs(i+1, j, p, cnt*(j+1))
    
primes = get(50)
n = int(input())
mxp, mxn = 0, 0

dfs(0, 30, 1, 1)
print(mxp)
```

### \200. Hankson的趣味题

```py
from sys import stdin
from math import gcd
input = lambda:stdin.readline().strip()

def lcm(a, b):
    return a*b//gcd(a, b)


def get(n):
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
        for p in primes:
            if i*p>n:
                break
            st[i*p] = True
            if i%p==0:
                break
    
def dfs(i, p):
    if i==len(pd):
        if p<=b1:
            div.append(p)
        return 
    
    dfs(i+1, p)
    for j in range(1, pd[i][1] + 1):
        p *= pd[i][0]
        dfs(i+1, p)
        


n = int(input())

N = int(2e9) + 10
N = int(N**0.5)
st = [False]*(N+10)
primes = []

get(N)

for i in range(n):
    a0, a1, b0, b1 = map(int, input().split())
    
    t = b1
    pd = []
    for p in primes:
        if t%p==0:
            cnt = 0
            while t%p==0:
                cnt += 1
                t//=p
            pd.append((p, cnt))
    if t>1:
        pd.append((t, 1))
    
    div = []
    dfs(0, 1)
    # print(div)
    
    res = 0
    for x in div:
        if gcd(x, a0)==a1 and lcm(x, b0)==b1:
            res += 1
            
    print(res)
```

### 楼兰图腾

```py
from sys import stdin

M = int(2e5) + 10
tr = [0]*M

def lowbit(x):
    return x&-x

def add(x, v):
    while x<M:
        tr[x] += v
        x += lowbit(x)
    
def query(x):
    res = 0
    while x:
        res += tr[x]
        x -= lowbit(x)
    return res

n = int(input())
a = list(map(int, input().split()))
suml, sumu = 0, 0

for x in a:
    lwl = query(x)
    lwr = (x-1) - lwl
    
    upr = query(n) - query(x)
    upl = n-x - upr
    
    suml += (lwl * lwr)
    sumu += (upl * upr)
    
    add(x, 1)

print(sumu, suml)
```

### 多重背包

```py
from sys import stdin
input = lambda:stdin.readline().strip()

N, V = map(int, input().split())
f = [0]*(V+1)
v, w = [], []

for i in range(N):
    vi, wi, si = map(int, input().split())
    c = 1
    while si>=c:
        v.append(c*vi)
        w.append(c*wi)
        si -= c
        c *= 2
    if si:
        v.append(si*vi)
        w.append(si*wi)
    
l = len(v)

for i in range(l):
    for j in range(V, v[i]-1, -1):
        f[j] = max(f[j], f[j-v[i]] + w[i])

print(f[V])

```

### 欧拉函数

```py

def get(x):
    res = x
    for i in range(2, int(x**0.5) + 1):
        if x%i==0:
            res = res//i*(i-1)
            while x%i==0:
                x//=i
    if x>1:
        res = res//x*(x-1)
    return res
            

n = int(input())
for i in range(n):
    x = int(input())
    print(get(x))
```

### 线性筛法求欧拉函数

```py
def get(n):
    euler[1] = 1
    for i in range(2, n+1):
        if not st[i]:
            primes.append(i)
            euler[i] = i-1
        for p in primes:
            t = i*p
            if t>n:
                break
            st[t] = True
            if i%p==0:
                euler[t] = euler[i] * p
                break
            euler[t] = euler[i] * (p-1)
    
n = int(input())

st = [False] * (n+1)
primes = []
euler = [0]*(n+1)

get(n)

res = 0
for i in range(1, n+1):
    res += euler[i]
    
print(res)
```

