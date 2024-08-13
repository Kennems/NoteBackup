## 问题 J: Brute Force Sorting

链表

```c++
#include<bits/stdc++.h>
using namespace std;
#define endl "\n"
const int maxn=1e7+10;
int a[maxn],nxt[maxn],last[maxn];
int flag[150000];
int t,n,ans,cnt,tmp;
signed main()
{
	ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	cin>>t;
	while(t--){
		cnt=0; 
		memset(flag,0,sizeof(flag));
		queue<int> s,q;
		cin>>n;
		for(int i=1;i<=n;i++) cin>>a[i];
		for(int i=1;i<=n;i++){
			q.push(i); //第一次从头开始遍历 
			last[i]=i-1; //指向前一个 
			nxt[i]=i+1; //指向后一个 
		}
		while(1){
			tmp=0;
			while(!q.empty()){
				int u=q.front(); q.pop(); //
				if(u>=1&&u<=n) s.push(u);
			}
			while(!s.empty()){
				int u=s.front(); s.pop();
				if(u>=1&&u<=n&&nxt[u]>=1&&nxt[u]<=n&&a[u]>a[nxt[u]]){ //判断不是第一个或最后一个而且前一个大于后一个 
					if(flag[u]==1||flag[nxt[u]]==1) continue; //如果已经标记 或后面已经标记 
					int cur=u; //当前点为cur即u, 而且u这个点大于之后的点 
					int v=nxt[u]; //v表示u后面的点 
					while(1){
						if(u>=1&&u<=n&&v>=1&&v<=n){ //如果在范围之内 
							tmp=1;//此时还有逆序 
							if(a[u]>a[v]){
								flag[u]=flag[v]=1;
								nxt[last[cur]]=nxt[v];  //当前点前面的点指向下一个点后面的点，即跳过这的逆序段 
								last[nxt[v]]=last[cur];  //同上 
							}
							else break;
						}
						else break; //不在范围之内 
						u=nxt[u]; //往后移一位 ，确认下一位不是逆序 
						v=nxt[v]; 
					}
					q.push(last[cur]); //此时这一小段逆序已经找完，只用确认这个点的上一个点，确认这个点会不会和后面形成逆序 
				}
			}
			if(tmp==0) break;
		}
		for(int i=1;i<=n;i++){
			if(!flag[i]) cnt++; 
		} 
		cout<<cnt<<endl;
		for(int i=1;i<=n;i++){
			if(!flag[i]) cout<<a[i]<<" ";
		}
		cout<<endl;
	}
	return 0;
}
```

## ACM ICPC 2017 HongKong I Count the Even Integers (JAVA 大数 杨辉三角)



## B - Black and White

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int n;
int sum[40000];
int mark[40000];
void nodeupdate(int root,int l,int r,ll num)
{
	mark[root]^=1;
	sum[root]=(r-l+1)-sum[root];
}
void pushdown(int root,int l,int r)// 传递给两孩子 
{
	if(mark[root]==0)return;
	int mid=(l+r)/2;
	nodeupdate(root*2,l,mid,mark[root]);
	nodeupdate(root*2+1,mid+1,r,mark[root]);
	mark[root]=0;
}
void update(int kl,int kr,ll num, int root=1,int l=1,int r=n)//区间[kl,kr]修改
{
	if(kl<=l&&r<=kr){
	nodeupdate(root,l,r,num);
	return;
}
	pushdown(root,l,r);
	int mid=(l+r)/2;
	if(kl<=mid)
	update(kl,kr,num,root*2,l,mid);
	if(kr>mid)
	update(kl,kr,num,root*2+1,mid+1,r);
	sum[root]=sum[root*2]+sum[root*2+1];
}
struct node{
	int h,a,b,flag;
}e[404040];
int cnt=0;
bool cmp(node a,node b){
	if(a.h==b.h)return a.flag>b.flag;
	return a.h<b.h;
}
int main()
{
	int T,m,x1,x2,y1,y2;
	cin>>T;
	while(T--)
	{
		cin>>n>>m;
		memset(sum,0,sizeof(sum));
		memset(mark,0,sizeof(mark));
		cnt=0;
		for(int i=0;i<m;i++)
		{
			scanf("%d%d%d%d",&x1,&x2,&y1,&y2);
			e[cnt++]=node{y1,x1,x2,1};
			e[cnt++]=node{y2,x1,x2,-1};
		}
		sort(e,e+cnt,cmp);
		int ans=0;
		for(int i=1,j=0;i<=n;i++)
		{
			while(j<cnt&&e[j].h<=i&&e[j].flag==1){
				update(e[j].a,e[j].b,1);
				j++;
			}
			ans+=sum[1];
			while(j<cnt&&e[j].h<=i){
				update(e[j].a,e[j].b,1);
			j++;
			}
		}
		cout<<ans<<endl;
	}
	return 0; 
}
```

