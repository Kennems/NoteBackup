## python程序答案

### 1、编写程序，判断今天是今年的第几天。

```py
import time
date = time.localtime()
year,month,day = date[:3]
day_month = [31,28,31,30,31,30,31,31,30,31,30,31]
if year%400 == 0 or (year % 4 == 0 and year%100 != 0):
    day_month[1] = 29
if month == 1:
    print('今天是今年的第:%d' % day)
else:
    print('今天是今年的第:%d天'% (sum(day_month[:month-1])+day))
```

### 2、编写代码，模拟比赛评委打分系统。要求评委人数由键盘输入，打分规则为删除最高分和最低分之后计算剩余分数的平均分（不考虑分值输入错误情况）。

```python
list=[]
n=int(input("请输入评委人数："))
for i in range(n):
    t=eval(input(f"请输入第{i+1}位评委的打分："))
    list.append(t)
if n<=3:
    cj=sum(list)/n
else:
    list.sort() #对打分进行排序
    list.pop() #删除最后一项
    list.pop(0) #删除第一项
    cj=sum(list)/(n-2)
print(f"选手的成绩为：{round(cj,4)}")
```

### 3、编写程序，计算小于100的最大素数。

```python
for n in range(100,1,-1):
    for i in range(2,n):
        if n%i==0:
            print(i)
            break
    else:
        print(n,end=' ')
        break
#python循环内if循环外else
```

### 4、输入若干个成绩，求所有成绩的平均分。每输入一个成绩后询问是否继续输入下一个成绩，回答“yes”就继续输入下一个成绩，回答“no”就停止输入成绩，对输入的不合法成绩引发的异常给出提示：'不是合法成绩'。

```python
numbers = [] #使用列表存放临时数据
while True:
    x = input('请输入一个成绩: ')
    #检查是不是浮点数
    try:
        numbers.append(float(x))
    except:
        print('不是合法成绩')

    while True:
        flag = input('继续输入吗?(yes / no) ')
        if flag.lower() not in ('yes', 'no'):
            print('只能输入yes 或 no')
        else:
            break

    if flag.lower() == 'no':
        break

print(sum(numbers) / len(numbers))
```

### 5、编写函数，接收字符串参数，返回一个元组，其中第一个元素为大写字母个数，第二个元素为小写字母个数；从键盘输入字符串作实参，输出函数调用的结果。

```python
def gs(st):
    dx = 0
    xx = 0
    for i in st:
        if i.islower():
            xx += 1
        elif i.isupper():
            dx += 1
    return (dx,xx)
print(gs(input("请输入一串字符串：")))
def num(str1):
    big = 0
    small = 0
    for i in str1:
        if i.isupper():
            big += 1
        elif i.islower():
            small += 1
    return big, small
if __name__ == '__main__':
    s = 'ehlla WROLD'
    n = num(s)
    print(n)
```

### 6、编写函数实现凯撒加密算法：即把原始字符串中的每个英文字母都使用后面第k个字母进行替换，其中k表示算法秘钥。例如：若k=3，’abc’加密后为：’def’；若k=5，’Abc’加密后为：’Fgh’。

```python
import string
def kaisa(s, k): #定义函数 接受一个字符串s 和 一个偏移量k
    lower = string.ascii_lowercase #小写字母
    upper = string.ascii_uppercase #大写字母
    before = string.ascii_letters #无偏移的字母顺序 小写+大写
    after = lower[k:] + lower[:k] + upper[k:] + upper[:k]#偏移后的字母顺序 还是小写+大写
    #分别把小写字母和大写字母偏移后再加到一起 
    table = ''.maketrans(before, after)  #创建映射表
    return s.translate(table) #对s进行偏移 即加密
s = input('请输入一个字符串：')
k = int(input('请输入一个整数密钥：'))
print(kaisa(s, k))
```

### 7、统计文本文件sample.txt中最长一行的长度和该行的内容，并输出

```python
result=[0,'']
with open('data.txt','r') as fp:	
	for line in fp:
		t=len(line)
		if t>result[0]:
			result=[t,line]
print(result)
```

