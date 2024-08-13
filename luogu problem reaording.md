# luogu problem reaording 

## P1434 [SHOI2002] 滑雪

大致题意为从高到低，找出最长路径，运用动态规划/记忆化搜索，dfs,注意最后结果为距离，边界的距离为一,dfs结果需要加一

```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e3+10;
int a[maxn][maxn],m[maxn][maxn];
int r,c,ma=-1;
int dx[]={1,0,-1,0};
int dy[]={0,1,0,-1};
int dfs(int x,int y){
	if(m[x][y]) return m[x][y]; //memory search 
	m[x][y]=1;
	for(int i=0;i<4;i++){
		int xx=x+dx[i];
		int yy=y+dy[i];
		if(xx>=0&&xx<r&&yy>=0&&yy<c&&a[xx][yy]<a[x][y]){
			dfs(xx,yy);
			m[x][y]=max(m[x][y],m[xx][yy]+1);
		}
	}
	return m[x][y];
}
int main()
{
	cin>>r>>c;
	for(int i=0;i<r;i++){
		for(int j=0;j<c;j++){
			cin>>a[i][j];
		}
	}
	for(int i=0;i<r;i++){
		for(int j=0;j<c;j++){
			ma=max(ma,dfs(i,j));
		}
	}
	cout<<ma<<endl;
	return 0;
 } 
```

## P1127 词链

欧拉回路



## P1572 计算分数

浪费了n多时间，一定要好好反思，吸取教训。

题目大意：给一串分式字符串，求最后的和。

开始想的太简单了，只考虑了一位数的情况，没有考虑到a=0或b=1的情况。看输入数据才发现问题，之后又改变策略，字符串表达式总是处理不好数组越界的问题，昨天晚上的cf也是这样。之后应该是运行时错误（除以零），要好好总结一下运行时错误RE，程序自己暂停，调了好久才发现。

代码也写的很长，较复杂。

```c++
//really not hard, if you can keep doing that and can quickly do the problem that you have done
#include<bits/stdc++.h>
using namespace std;
#define int long long 
#define endl "\n"
const int maxn=1e7+10;
char s[maxn];
int k=1,a,b,ta,tb,l,in,num;
int gcd(int a1,int b1){
	a1=abs(a1); b1=abs(b1); 
	return b1>0 ? gcd(b1,a1%b1) : a1;
}
int lcm(int a1,int b1){
	return a1/gcd(a1,b1)*b1;
} 
void add(int x,int y){
	if(a==0&&b==0){
		a=x; b=y;
		return ;
	}
	if(x%y==0){
		int t=x/y;
		a+=b*t;
		return ;
	} 
	int lc=lcm(b,y);
	int t1=lc/b,t2=lc/y;
	a=a*t1+x*t2;
	b=lc;
}
signed main()
{
	ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	scanf("%s",s+1);
	l=strlen(s+1); in=0;
	if(s[1]=='-'){
		in=1; k=-1;
	}
	while(1){
		in++;
		if(in==l+1){
			break;
		}
		if(s[in]>='0'&&s[in]<='9'){
			num=num*10+s[in]-'0';
		}
		if(s[in]=='-'){
			add(k*ta,num);
			k=-1;
			num=0;
		}
		if(s[in]=='+'){
			add(k*ta,num);
			k=1;
			num=0;
		}
		if(s[in]=='/'){
			ta=num; num=0;
		}
	}
	add(k*ta,num);
	int g=gcd(a,b);
	a/=g; b/=g;
	if(a==0){
		cout<<0<<endl;
		return 0; 
	} 
	if(b==1){
		cout<<a<<endl;
		return 0;
	} 
	cout<<a<<"/"<<b<<endl;
	return 0;
}
```

## P1403 [AHOI2005]约数研究

题目技巧性很强，也是规律性。一个从1正整数n的约数为i个数为 n/i 向下取整。

遍历一遍就能过，

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define endl "\n"
int n,ans;

signed main()
{
	ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin>>n;
	for(int i=1,j;i<=n;i=j+1){
		j=n/(n/i);
		ans+=(n/i)*(j-i+1);
	}
	cout<<ans<<endl;
}
```

