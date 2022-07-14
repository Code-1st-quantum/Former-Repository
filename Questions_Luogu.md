**$\mathbf{\color{Purple}{Welcome\ to\ The\ OI\ -\ Space\ of\ Code}} \mathbf{\color{Purple}{\underline{\ \ }}} \mathbf{\color{Purple}{quantum\ on\ Github}}$**

PS.在输入算法名称寻找相应题目时，若要输入多个算法，请按照拼音字典序依次输入，两个算法间添加 '/' 符号。
## P1002 [NOIP2002 普及组] 过河卒(递推)

**题目描述**

棋盘上 $A$ 点有一个过河卒，需要走到目标 $B$ 点。卒行走的规则：可以向下、或者向右。同时在棋盘上 $C$ 点有一个对方的马，该马所在的点和所有跳跃一步可达的点称为对方马的控制点。因此称之为“马拦过河卒”。

棋盘用坐标表示，$A$ 点 $(0, 0)$、$B$ 点 $(n, m)$，同样马的位置坐标是需要给出的。

![](https://cdn.luogu.com.cn/upload/image_hosting/vg6k477j.png)

现在要求你计算出卒从 $A$ 点能够到达 $B$ 点的路径的条数，假设马的位置是固定不动的，并不是卒走一步马走一步。

 输入格式

一行四个正整数，分别表示 $B$ 点坐标和马的坐标。

**输出格式**

一个整数，表示所有的路径条数。

**样例**

**样例输入**

```
6 6 3 3
```

**样例输出**

```
6
```

**提示**

对于 $100 \%$ 的数据，$1 \le n, m \le 20$，$0 \le$ 马的坐标 $\le 20$。

**【题目来源】**

NOIP 2002 普及组第四题

```c++
#include<bits/stdc++.h>
using namespace std;
int cotx[22]={0,1,2,2,1,-1,-2,-2,-1},coty[22]={0,2,1,-1,-2,-2,-1,1,2};
long long n,m,hx,hy;
int hc[22][22]={0};
long long mapp[22][22]={0};
int main(){
	cin>>n>>m>>hx>>hy;
	for(int i=1;i<=9;i++){
		int cx=hx+cotx[i];
		int cy=hy+coty[i];
		if(cx>=0&&cx<=n&&cy>=0&&cy<=m) hc[cx][cy]=1;
	}
	if(hc[0][0]==1){
      cout<<0;
      return 0;
	}mapp[0][0]=1;
	for(int i=0;i<=n;i++){
		for(int j=0;j<=m;j++){
			if(hc[i][j]==1) continue;
			if(i!=0) mapp[i][j]+=mapp[i-1][j];
			if(j!=0) mapp[i][j]+=mapp[i][j-1];
		}
	}
	cout<<mapp[n][m];
	return 0;
}

```
---
## P1003 [NOIP2011 提高组] 铺地毯(模拟)

**题目描述**

为了准备一个独特的颁奖典礼，组织者在会场的一片矩形区域（可看做是平面直角坐标系的第一象限）铺上一些矩形地毯。一共有 $n$ 张地毯，编号从 $1$ 到 $n$。现在将这些地毯按照编号从小到大的顺序平行于坐标轴先后铺设，后铺的地毯覆盖在前面已经铺好的地毯之上。

地毯铺设完成后，组织者想知道覆盖地面某个点的最上面的那张地毯的编号。注意：在矩形地毯边界和四个顶点上的点也算被地毯覆盖。

**输入格式**

输入共 $n + 2$ 行。

第一行，一个整数 $n$，表示总共有 $n$ 张地毯。

接下来的 $n$ 行中，第 $i+1$ 行表示编号 $i$ 的地毯的信息，包含四个整数 $a ,b ,g ,k$，每两个整数之间用一个空格隔开，分别表示铺设地毯的左下角的坐标 $(a, b)$ 以及地毯在 $x$ 轴和 $y$ 轴方向的长度。

第 $n + 2$ 行包含两个整数 $x$ 和 $y$，表示所求的地面的点的坐标 $(x, y)$。

**输出格式**

输出共 $1$ 行，一个整数，表示所求的地毯的编号；若此处没有被地毯覆盖则输出 `-1`。

**样例 #1**

**样例输入 #1**

```
3
1 0 2 3
0 2 3 3
2 1 3 3
2 2
```

**样例输出 #1**

```
3
```

**样例 #2**

**样例输入 #2**

```
3
1 0 2 3
0 2 3 3
2 1 3 3
4 5
```

**样例输出 #2**

```
-1
```

**提示**

【样例解释 1】

如下图，$1$ 号地毯用实线表示，$2$ 号地毯用虚线表示，$3$ 号用双实线表示，覆盖点 $(2,2)$ 的最上面一张地毯是 $3$ 号地毯。

 ![](https://cdn.luogu.com.cn/upload/pic/100.png) 

【数据范围】

对于 $30\%$ 的数据，有 $n \le 2$。  
对于 $50\%$ 的数据，$0 \le a, b, g, k \le 100$。  
对于 $100\%$ 的数据，有 $0 \le n \le 10^4$, $0 \le a, b, g, k \le {10}^5$。   

noip2011 提高组 day1 第 $1$ 题。
```c++
#include<iostream>
using namespace std;
struct node{
	int a,b,c,d;
}s[10005];
int t,x,y,ans;
bool flag=false;
int main(){
	ios::sync_with_stdio(false);
	cin>>t;
	for(int i=1;i<=t;i++){
	    cin>>s[i].a>>s[i].b>>s[i].c>>s[i].d;	
	}
	cin>>x>>y;
	for(int i=1;i<=t;i++){
		if(x>=s[i].a&&x<=s[i].a+s[i].c-1&&y>=s[i].b&&y<=s[i].b+s[i].d-1){
			ans=i;
			flag=true;
		}
	}
	if(flag){
		cout<<ans<<endl;
		return 0;
	}
	cout<<-1<<endl;
	return 0;
}
```
---
## P1253 [yLOI2018] 扶苏的问题(线段树)

**题目描述**

给定一个长度为 $n$ 的序列 $a$，要求支持如下三个操作：

1. 给定区间 $[l, r]$，将区间内每个数都修改为 $x$。
2. 给定区间 $[l, r]$，将区间内每个数都加上 $x$。
3. 给定区间 $[l, r]$，求区间内的最大值。

**输入格式**

第一行是两个整数，依次表示序列的长度 $n$ 和操作的个数 $q$。  
第二行有 $n$ 个整数，第 $i$ 个整数表示序列中的第 $i$ 个数 $a_i$。  
接下来 $q$ 行，每行表示一个操作。每行首先有一个整数 $op$，表示操作的类型。
- 若 $op = 1$，则接下来有三个整数 $l, r, x$，表示将区间 $[l, r]$ 内的每个数都修改为 $x$。
- 若 $op = 2$，则接下来有三个整数 $l, r, x$，表示将区间 $[l, r]$ 内的每个数都加上 $x$。
- 若 $op = 3$，则接下来有两个整数 $l, r$，表示查询区间 $[l, r]$ 内的最大值。

**输出格式**

对于每个 $op = 3$ 的操作，输出一行一个整数表示答案。

**样例 #1**

**样例输入 #1**

```
6 6
1 1 4 5 1 4
1 1 2 6
2 3 4 2
3 1 4
3 2 3
1 1 6 -1
3 1 6
```

**样例输出 #1**

```
7
6
-1
```

**样例 #2**

**样例输入 #2**

```
4 4
10 4 -3 -7
1 1 3 0
2 3 4 -4
1 2 4 -9
3 1 4
```

**样例输出 #2**

```
0
```

**提示**

**数据规模与约定**

- 对于 $10\%$ 的数据，$n = q = 1$。
- 对于 $40\%$ 的数据，$n, q \leq 10^3$。
- 对于 $50\%$ 的数据，$0 \leq a_i, x \leq 10^4$。
- 对于 $60\%$ 的数据，$op \neq 1$。
- 对于 $90\%$ 的数据，$n, q \leq 10^5$。
- 对于 $100\%$ 的数据，$1 \leq n, q \leq 10^6$，$1 \leq l, r \leq n$，$op \in \{1, 2, 3\}$，$|a_i|, |x| \leq 10^9$。

**提示**

请注意大量数据读入对程序效率造成的影响。
```c++
#include<cstdio>
#include<algorithm>
#define MAXN 1000005
using namespace std;
typedef long long ll;
const ll INF=1e18;
struct sgtree{
	ll l,r;
	ll d1,d2,maxn;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define d1(x) tree[x].d1
	#define d2(x) tree[x].d2
	#define maxn(x) tree[x].maxn
}tree[4*MAXN];
ll n,m;
void build(ll x,ll l,ll r){
	l(x)=l,r(x)=r,d1(x)=INF,d2(x)=0,maxn(x)=-INF;
	if(l==r) return (void)(scanf("%lld",&maxn(x)));
	ll mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	maxn(x)=max(maxn(x<<1),maxn(x<<1|1));
}
inline void change(ll x,ll k){
	d2(x)=0; d1(x)=k;
	maxn(x)=k;
}
inline void add(ll x,ll k){
	d2(x)+=k;
	maxn(x)+=k;
}
inline void pushdown(ll x){
	if(d1(x)!=INF){
		change(x<<1,d1(x));
		change(x<<1|1,d1(x));
		d1(x)=INF;
	}
	if(d2(x)){
		add(x<<1,d2(x));
		add(x<<1|1,d2(x));
		d2(x)=0;
	}
}
inline ll ask(ll x,ll l,ll r){
	if(l<=l(x)&&r(x)<=r) return maxn(x);
	pushdown(x); ll ans=-INF;
	ll mid=(l(x)+r(x))>>1;
	if(l<=mid) ans=max(ans,ask(x<<1,l,r));
	if(r>mid) ans=max(ans,ask(x<<1|1,l,r));
	return ans;
} 
inline void modify(ll x,ll l,ll r,ll op,ll k){
	if(l<=l(x)&&r>=r(x)){
		if(op==1) change(x,k);
		else add(x,k);
		return;
	}
	pushdown(x);
	ll mid=(l(x)+r(x))>>1;
	if(l<=mid) modify(x<<1,l,r,op,k);
	if(r>mid) modify(x<<1|1,l,r,op,k);
	maxn(x)=max(maxn(x<<1),maxn(x<<1|1)); 
}
int main(){
	scanf("%lld%lld",&n,&m);
	build(1,1,n);
	while(m--){
		ll op,l,r; ll k;
		scanf("%lld%lld%lld",&op,&l,&r);
		if(op==1){
			scanf("%lld",&k);
			modify(1,l,r,1,k);
		}else if(op==2){
			scanf("%lld",&k);
			modify(1,l,r,2,k);
		}else printf("%lld\n",ask(1,l,r));
	}
	return 0;
}
```
---

## P1314 [NOIP2011 提高组] 聪明的质监员(二分答案/前缀和)

**题目描述**

`小T` 是一名质量监督员，最近负责检验一批矿产的质量。这批矿产共有 $n$ 个矿石，从 $1$ 到 $n$ 逐一编号，每个矿石都有自己的重量 $w_i$ 以及价值 $v_i$ 。检验矿产的流程是：

1 、给定$ m$ 个区间 $[l_i,r_i]$；

2 、选出一个参数 $W$；

3 、对于一个区间 $[l_i,r_i]$，计算矿石在这个区间上的检验值 $y_i$：

$$y_i=\sum\limits_{j=l_i}^{r_i}[w_j \ge W] \times \sum\limits_{j=l_i}^{r_i}[w_j \ge W]v_j$$  

其中 $j$ 为矿石编号。

这批矿产的检验结果 $y$ 为各个区间的检验值之和。即：$\sum\limits_{i=1}^m y_i$  

若这批矿产的检验结果与所给标准值 $s$ 相差太多，就需要再去检验另一批矿产。`小T` 不想费时间去检验另一批矿产，所以他想通过调整参数 $W$ 的值，让检验结果尽可能的靠近标准值 $s$，即使得 $|s-y|$ 最小。请你帮忙求出这个最小值。

**输入格式**

第一行包含三个整数 $n,m,s$，分别表示矿石的个数、区间的个数和标准值。

接下来的 $n$ 行，每行两个整数，中间用空格隔开，第 $i+1$ 行表示 $i$ 号矿石的重量 $w_i$ 和价值 $v_i$。

接下来的 $m$ 行，表示区间，每行两个整数，中间用空格隔开，第 $i+n+1$ 行表示区间 $[l_i,r_i]$ 的两个端点 $l_i$ 和 $r_i$。注意：不同区间可能重合或相互重叠。

**输出格式**

一个整数，表示所求的最小值。

**样例 #1**

**样例输入 #1**

```
5 3 15 
1 5 
2 5 
3 5 
4 5 
5 5 
1 5 
2 4 
3 3
```

**样例输出 #1**

```
10
```

**提示**

【输入输出样例说明】

当 $W$ 选 $4$ 的时候，三个区间上检验值分别为 $20,5 ,0$ ，这批矿产的检验结果为 $25$，此时与标准值 $S$ 相差最小为 $10$。

【数据范围】

对于 $10\% $ 的数据，有 $1 ≤n ,m≤10$；

对于 $30\% $的数据，有 $1 ≤n ,m≤500$ ；

对于 $50\% $ 的数据，有 $ 1 ≤n ,m≤5,000$；
 
对于 $70\%$ 的数据，有 $1 ≤n ,m≤10,000$ ；

对于 $100\%$ 的数据，有 $ 1 ≤n ,m≤200,000$，$0 < w_i,v_i≤10^6$，$0 < s≤10^{12}$，$1 ≤l_i ≤r_i ≤n$ 。
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cmath>
#define MAXN 200005
using namespace std;
typedef long long ll;
ll n,m,s,w[MAXN],v[MAXN],l[MAXN],r[MAXN],sum1[MAXN],sum2[MAXN],ans=1e18,maxn=-1,minn=1e18,now;
bool chck(ll x){
	ll y=0;
	for(int i=0;i<=max(m,n);i++) sum1[i]=sum2[i]=0;
	for(int i=1;i<=n;i++)
		if(w[i]>x) sum1[i]=sum1[i-1]+1,sum2[i]=sum2[i-1]+v[i];
		else sum1[i]=sum1[i-1],sum2[i]=sum2[i-1];
	for(int i=1;i<=m;i++) y+=(sum1[r[i]]-sum1[l[i]-1])*(sum2[r[i]]-sum2[l[i]-1]);
	now=abs(y-s); //记录当前与 s 的差值 
	if(y>s) return true; //如果 y>s ,则要将二分的 w 变大,从而调小 y ,使其更接近 s 
	else return false;
}
int main(){
	scanf("%lld%lld%lld",&n,&m,&s);
	for(int i=1;i<=n;i++) scanf("%lld%lld",&w[i],&v[i]),maxn=max(maxn,w[i]),minn=min(minn,w[i]);
	for(int i=1;i<=m;i++) scanf("%lld%lld",&l[i],&r[i]);
	int l=minn-1,r=maxn+1;
	while(l<=r){
		int mid=(l+r)>>1;
		if(chck(mid)) l=mid+1;
		else r=mid-1; 
		if(now<ans) ans=now; //将差值最小化 
	}
	printf("%lld",ans);
	return 0;
}
```
---
## P1542 包裹快递(二分答案)

**题目描述**

小K成功地破解了密文。但是乘车到X国的时候，发现钱包被偷了，于是无奈之下只好作快递员来攒足路费去Orz教主……

一个快递公司要将n个包裹分别送到n个地方，并分配给邮递员小K一个事先设定好的路线，小K需要开车按照路线给的地点顺序相继送达，且不能遗漏一个地点。小K得到每个地方可以签收的时间段，并且也知道路线中一个地方到下一个地方的距离。若到达某一个地方的时间早于可以签收的时间段，则必须在这个地方停留至可以签收，但不能晚于签收的时间段，可以认为签收的过程是瞬间完成的。

为了节省燃料，小K希望在全部送达的情况下，车的最大速度越小越好，就找到了你给他设计一种方案，并求出车的最大速度最小是多少。

**输入格式**

第1行为一个正整数n，表示需要运送包裹的地点数。

下面n行，第i+1行有3个正整数xi，yi，si，表示按路线顺序给出第i个地点签收包裹的时间段为[xi, yi]，即最早为距出发时刻xi，最晚为距出发时刻yi，从前一个地点到达第i个地点距离为si，且保证路线中xi递增。

可以认为s1为出发的地方到第1个地点的距离，且出发时刻为0。

**输出格式**

仅包括一个正数，为车的最大速度最小值，结果保留两位小数。

**样例 #1**

**样例输入 #1**

```
3
1 2 2
6 6 2
7 8 4
```

**样例输出 #1**

```
2.00
```
**样例输入 #2**

```
9
4 6 3
12 19 12
18 26 6
20 29 18
27 32 4
28 36 6
29 33 13
37 40 2
49 50 3
```

**样例输出 #2**

```
3.17
```

**提示**

**数据范围**
- 对于 $20\%$ 的数据，$0 < n \le 10$。   
- 对于 $30\%$ 的数据，$0<x_i,y_i,s_i \le 1000$。   
- 对于 $50\%$ 的数据，$0<n \le 1000$。   
- 对于 $100\%$ 的数据，$0<n \le 2\times10^5$，$x_i \le y_i \le 10^8$，$s_i \le10^7$。

**样例解释**
第一段用$1$的速度在时间$2$到达第$1$个地点，第二段用$0.5$的速度在时间$6$到达第$2$个地点，第三段用$2$的速度在时间$8$到达第$3$个地点。
```c++
#include<cstdio>
#include<algorithm>
#include<cmath>
#define MAXN 200005
using namespace std;
typedef double db;
typedef long double ldb;
db ans=1e9;
int n,x[MAXN],y[MAXN],s[MAXN],maxn=-1;
bool chck(db xx){
	ldb t=0;
	for(int i=1;i<=n;i++){
		t+=s[i]/xx;
		if(t>y[i]) return false;
		if(t<x[i]) t=x[i];
	}
	return true;
}
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++) scanf("%d%d%d",&x[i],&y[i],&s[i]),maxn=max(maxn,s[i]); 
	ldb l=0,r=(db)maxn;
	while(r-l>=0.00001){  //控制精度，二分答案常用
		db mid=(l+r)/2;
		if(chck(mid)) r=mid,ans=mid;
		else l=mid;
	}
	printf("%.2lf",ans);
	return 0;
}
```
---
## P1836 数页码(动态规划/数位DP)

**题目描述**

一本书的页码是从 $1\sim n$ 编号的连续整数：$1,2,3,\cdots,n$。请你求出全部页码中所有单个数字的和，例如第 $123$ 页，它的和就是 $ 1+2+3=6$。

**输入格式**

一行一个整数 $n$。

**输出格式**

一行，代表所有单个数字的和。

**样例 #1**

**样例输入 #1**

```
3456789
```

**样例输出 #1**

```
96342015
```

**提示**

$1\le n\le 10^9$
```c++
#include<cstdio>
#include<cstring>
using namespace std;
typedef long long ll;
ll x,len=0,a[12],dp[12][12];
ll dfs(int pos,ll sum,int num,int limit,int lead){
	if(!pos) return sum;
	if(!limit&&!lead&&dp[pos][sum]!=-1) return dp[pos][sum];
	ll ans=0,ret=limit?a[pos]:9;
	for(int i=0;i<=ret;i++)
	  ans+=dfs(pos-1,sum+(i==num),num,limit&&(i==ret),lead&&(!i));
	if(!limit&&!lead) dp[pos][sum]=ans;
	return ans;
}
ll work(){
	ll x2=x; ll ans=0;
	while(x2){
		a[++len]=x2%10;
		x2/=10;
	}
	for(int i=1;i<=9;i++){
		memset(dp,-1,sizeof dp);
		ans+=i*dfs(len,0,i,1,1);
	}
	return ans;
}
int main(){
	scanf("%lld",&x);
	printf("%lld",work());
	return 0;
}
```
---

## P1902 刺杀大使(二分答案/搜索)

**题目描述**

某组织正在策划一起对某大使的刺杀行动。他们来到了使馆，准备完成此次刺杀，要进入使馆首先必须通过使馆前的防御迷阵。

迷阵由 $n\times m$ 个相同的小房间组成，每个房间与相邻四个房间之间有门可通行。在第 $n$ 行的 $m$ 个房间里有 $m$ 个机关，这些机关必须全部打开才可以进入大使馆。而第 $1$ 行的 $m$ 个房间有 $m$ 扇向外打开的门，是迷阵的入口。除了第 $1$ 行和第 $n$ 行的房间外，每个房间都被使馆的安保人员安装了激光杀伤装置，将会对进入房间的人造成一定的伤害。第 $i$ 行第 $j$ 列 造成的伤害值为 $p_{i,j}$（第 $1$ 行和第 $n$ 行的 $p$ 值全部为 $0$）。

现在某组织打算以最小伤害代价进入迷阵，打开全部机关，显然，他们可以选 择任意多的人从任意的门进入，但必须到达第 $n$ 行的每个房间。一个士兵受到的伤害值为他到达某个机关的路径上所有房间的伤害值中的最大值，整个部队受到的伤害值为所有士兵的伤害值中的最大值。现在，这个恐怖组织掌握了迷阵的情况，他们需要提前知道怎么安排士兵的行进路线可以使得整个部队的伤害值最小。

**输入格式**

第一行有两个整数 $n,m$，表示迷阵的大小。

接下来 $n$ 行，每行 $m$ 个数，第 $i$ 行第 $j$ 列的数表示 $p_{i,j}$。

**输出格式**

输出一个数，表示最小伤害代价。

**样例 #1**

**样例输入 #1**

```
4 2
0 0 
3 5 
2 4 
0 0
```

**样例输出 #1**

```
3
```

**提示**

- $50\%$ 的数据，$n,m \leq 100$；
- $100\%$ 的数据，$n,m \leq 1000$，$p_{i,j} \leq 1000$。
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
#include<utility>
using namespace std;
int m,n,map[1005][1005],l=1e9,r=-1e9,ans=1e9;
bool vis[1005][1005]={false};
int dx[4]={1,0,-1,0};
int dy[4]={0,-1,0,1};
bool chck(int k){
	queue<pair<int,int> >q;
	memset(vis,false,sizeof vis);
	q.push(make_pair(1,1));
	vis[1][1]=true;
	while(!q.empty()){
		int x=q.front().first,y=q.front().second; 
		q.pop();
		for(int i=0;i<4;i++){
			int xx=x+dx[i],yy=y+dy[i];
			if(!(xx<=n&&yy<=m&&xx>=1&&yy>=1&&map[xx][yy]<=k&&!vis[xx][yy])) continue;
			vis[xx][yy]=true;
			q.push(make_pair(xx,yy));
			if(xx==n) return true;
		}
	}
	return false;
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)
	  for(int j=1;j<=m;j++)
	    scanf("%d",&map[i][j]),l=min(l,map[i][j]),r=max(r,map[i][j]);
	while(l<=r){
		int mid=(l+r)>>1;
		if(chck(mid)) ans=min(ans,mid),r=mid-1;
		else l=mid+1;
	}
	printf("%d",ans);
	return 0;
}
```
---
## P2061 [USACO07OPEN]City Horizon S(离散化/线段树)

**题面翻译**

约翰带着奶牛去都市观光。在落日的余晖里，他们看到了一幢接一幢的摩天高楼的轮廓在地平线 上形成美丽的图案。以地平线为 X 轴，每幢高楼的轮廓是一个位于地平线上的矩形，彼此间可能有 重叠的部分。奶牛一共看到了 N 幢高楼，第 i 幢楼的高度是 Hi，两条边界轮廓在地平线上的坐标是 Ai 到 Bi。请帮助奶牛们计算一下，所有摩天高楼的轮廓覆盖的总面积是多少。


**题目描述**

Farmer John has taken his cows on a trip to the city! As the sun sets, the cows gaze at the city horizon and observe the beautiful silhouettes formed by the rectangular buildings.

The entire horizon is represented by a number line with N (1 ≤ N ≤ 40,000) buildings. Building i's silhouette has a base that spans locations Ai through Bi along the horizon (1 ≤ Ai < Bi ≤ 1,000,000,000) and has height Hi (1 ≤ Hi ≤ 1,000,000,000). Determine the area, in square units, of the aggregate silhouette formed by all N buildings.

有一个数列，初始值均为0，他进行N次操作，每次将数列[ai,bi)这个区间中所有比Hi小的数改为Hi，他想知道N次操作后数列中所有元素的和。

**输入格式**

第一行一个整数N，然后有N行，每行三个正整数ai、bi、Hi。

**输出格式**

一个数，数列中所有元素的和。

**样例 #1**

**样例输入 #1**

```
4
2 5 1
9 10 4
6 8 2
4 6 3
```

**样例输出 #1**

```
16
```

**提示**

N<=40000 , a、b、k<=10^9 。
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
typedef long long ll;
struct house{
	int h,l,r;
	bool operator < (const house x)const {return h<x.h;}
}a[40005];
struct sgtree{
	int l,r,d,num;
	#define num(x) tree[x].num
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define d(x) tree[x].d
}tree[360005];
ll ans=0;
int n,all[120005],sum=0,tot;
void build(int x,int l,int r){
	l(x)=l,r(x)=r,d(x)=0;
	if(l==r) return;
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
}
void pushdown(int x){
	d(x<<1)=d(x);
	d(x<<1|1)=d(x);
	num(x<<1)=d(x);
	num(x<<1|1)=d(x);
	d(x)=0;
}
void modify(int x,int l,int r,int k){
	if(l<=l(x)&&r>=r(x)){d(x)=k; num(x)=k; return;}
	if(d(x)) pushdown(x);
	int mid=(l(x)+r(x))>>1;
	if(l<=mid) modify(x<<1,l,r,k);
	if(r>mid) modify(x<<1|1,l,r,k);
}
int query(int x,int l,int r){
	if(l<=l(x)&&r>=r(x)) return num(x);
	if(d(x)) pushdown(x);
	int mid=(l(x)+r(x))>>1;
	if(l<=mid) return query(x<<1,l,r);
	if(r>mid) return query(x<<1|1,l,r);
}
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++){
		scanf("%d%d%d",&a[i].l,&a[i].r,&a[i].h); a[i].r--;
		all[++sum]=a[i].l,all[++sum]=a[i].r,all[++sum]=a[i].r+1;
	}
	sort(all+1,all+sum+1);
	sort(a+1,a+n+1);
	tot=unique(all+1,all+sum+1)-(all+1);
	build(1,1,tot);
	for(int i=1;i<=n;i++){
		int l=lower_bound(all+1,all+tot+1,a[i].l)-all;
		int r=lower_bound(all+1,all+tot+1,a[i].r)-all;
		modify(1,l,r,a[i].h);
	}
	int lasthigh=query(1,1,1),p=1;
	for(int i=2;i<=tot;i++){
		int now=query(1,i,i);
		if(now!=lasthigh) ans+=1ll*(all[i]-all[p])*lasthigh,p=i,lasthigh=now;
	}
	printf("%lld",ans);
	return 0;
}
```
---
## P2569 [SCOI2010]股票交易(单调队列/动态规划)

**题目描述**

最近 $\text{lxhgww}$ 又迷上了投资股票，通过一段时间的观察和学习，他总结出了股票行情的一些规律。

通过一段时间的观察，$\text{lxhgww}$ 预测到了未来 $T$ 天内某只股票的走势，第 $i$ 天的股票买入价为每股 $AP_i$，第 $i$ 天的股票卖出价为每股 $BP_i$（数据保证对于每个 $i$，都有 $AP_i \geq BP_i$），但是每天不能无限制地交易，于是股票交易所规定第 $i$ 天的一次买入至多只能购买 $AS_i$ 股，一次卖出至多只能卖出 $BS_i$ 股。

另外，股票交易所还制定了两个规定。为了避免大家疯狂交易，股票交易所规定在两次交易（某一天的买入或者卖出均算是一次交易）之间，至少要间隔 $W$ 天，也就是说如果在第 $i$ 天发生了交易，那么从第 $i+1$ 天到第 $i+W$ 天，均不能发生交易。同时，为了避免垄断，股票交易所还规定在任何时间，一个人的手里的股票数不能超过 $\text{MaxP}$。

在第 $1$ 天之前，$\text{lxhgww}$ 手里有一大笔钱（可以认为钱的数目无限），但是没有任何股票，当然，$T$ 天以后，$\text{lxhgww}$ 想要赚到最多的钱，聪明的程序员们，你们能帮助他吗？

**输入格式**

输入数据第一行包括 $3$ 个整数，分别是 $T$，$\text{MaxP}$，$W$。

接下来 $T$ 行，第 $i$ 行代表第 $i-1$ 天的股票走势，每行 $4$ 个整数，分别表示 $AP_i,\ BP_i,\ AS_i,\ BS_i$。

**输出格式**

输出数据为一行，包括 $1$ 个数字，表示 $\text{lxhgww}$ 能赚到的最多的钱数。

**样例 #1**

**样例输入 #1**

```
5 2 0
2 1 1 1
2 1 1 1
3 2 1 1
4 3 1 1
5 4 1 1
```

**样例输出 #1**

```
3
```

**提示**

对于 $30\%$ 的数据，$0\leq W<T\leq 50,1\leq\text{MaxP}\leq50$

对于 $50\%$ 的数据，$0\leq W<T\leq 2000,1\leq\text{MaxP}\leq50$

对于 $100\%$ 的数据，$0\leq W<T\leq 2000,1\leq\text{MaxP}\leq2000$

对于所有的数据，$1\leq BP_i\leq AP_i\leq 1000,1\leq AS_i,BS_i\leq\text{MaxP}$
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int n,m,w,dp[2005][2005],l,r,q[2005],ans;
int main(){
	scanf("%d%d%d",&n,&m,&w);
	memset(dp,-0x3f,sizeof dp);
	for(int i=1;i<=n;i++){
		int ap,bp,as,bs;
		scanf("%d%d%d%d",&ap,&bp,&as,&bs);
		for(int j=0;j<=as;j++) dp[i][j]=(-1)*ap*j;
		for(int j=0;j<=m;j++) dp[i][j]=max(dp[i][j],dp[i-1][j]);
		if(i<=w) continue;
		l=1,r=0;
		for(int j=0;j<=m;j++){
			while(l<=r&&q[l]<j-as) l++;
			while(l<=r&&dp[i-w-1][j]+ap*j>=dp[i-w-1][q[r]]+ap*q[r]) r--;
			q[++r]=j;
			if(l<=r) dp[i][j]=max(dp[i][j],dp[i-w-1][q[l]]+ap*q[l]-ap*j);
		}
		l=1,r=0;
		for(int j=m;j>=0;j--){
			while(l<=r&&q[l]>j+bs) l++;
			while(l<=r&&dp[i-w-1][q[r]]+bp*q[r]<=dp[i-w-1][j]+bp*j) r--;
			q[++r]=j;
			if(l<=r) dp[i][j]=max(dp[i][j],dp[i-w-1][q[l]]+bp*q[l]-bp*j);
		}
	}
	for(int i=0;i<=m;i++) ans=max(ans,dp[n][i]);
	printf("%d",ans);
	return 0;
}
```

---
## P2846 [USACO08NOV]Light Switching G(线段树)

**题目描述**

Farmer John tries to keep the cows sharp by letting them play with intellectual toys. One of the larger toys is the lights in the barn. Each of the N (2 <= N <= 100,000) cow stalls conveniently numbered 1..N has a colorful light above it.

At the beginning of the evening, all the lights are off. The cows control the lights with a set of N pushbutton switches that toggle the lights; pushing switch i changes the state of light i from off to on or from on to off.

The cows read and execute a list of M (1 <= M <= 100,000) operations expressed as one of two integers (0 <= operation <= 1).

The first kind of operation (denoted by a 0 command) includes two subsequent integers $S_i$ and $E_i (1 <= S_i <= E_i <= N)$ that indicate a starting switch and ending switch. They execute the operation by pushing each pushbutton from $S_i$ through $E_i$ inclusive exactly once.

The second kind of operation (denoted by a 1 command) asks the cows to count how many lights are on in the range given by two integers $S_i$ and $E_i (1 <= S_i <= E_i <= N)$ which specify the inclusive range in which the cows should count the number of lights that are on.

Help FJ ensure the cows are getting the correct answer by processing the list and producing the proper counts.

灯是由高科技——外星人鼠标操控的。你只要左击两个灯所连的鼠标，

这两个灯，以及之间的灯都会由暗变亮，或由亮变暗。右击两个灯所连的鼠

标，你就可以知道这两个灯，以及之间的灯有多少灯是亮的。起初所有灯都是暗的，你的任务是在LZ之前算出灯的亮灭。

**输入格式**

第1 行: 用空格隔开的两个整数N 和M，n 是灯数

第2..M+1 行: 每行表示一个操作, 有三个用空格分开的整数: 指令号, $S_i$ 和$E_i$

第1 种指令(用0 表示)包含两个数字$S_i$ 和 $E_i (1 <= S_i <= E_i <= N) $, 它们表示起

始开关和终止开关. 表示左击

第2 种指令(用1 表示)同样包含两个数字$S_i$ 和$E_i (1 <= S_i <= E_i <= N)$, 不过这

种指令是询问从$S_i$ 到$E_i$ 之间的灯有多少是亮着的.

**输出格式**

**样例 #1**

**样例输入 #1**

```
4 5
0 1 2
0 2 4
1 2 3
0 2 4
1 1 4
```

**样例输出 #1**

```
1
2
```

**提示**

![](https://cdn.luogu.com.cn/upload/pic/2448.png)
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
struct sgtree{
	int l,r,num,d;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define num(x) tree[x].num
	#define d(x) tree[x].d
}tree[400005];
int n,m;
void build(int x,int l,int r){
	tree[x].l=l,tree[x].r=r;
	if(l==r){num(x)=0; return;}
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
}
void pushdown(int x){
	d(x<<1)^=1;
	d(x<<1|1)^=1;
	num(x<<1)=(r(x<<1)-l(x<<1)+1-num(x<<1));
	num(x<<1|1)=(r(x<<1|1)-l(x<<1|1)+1-num(x<<1|1));
	d(x)=0;
}
void modify(int x,int l,int r){
	if(l<=l(x)&&r>=r(x)){
		d(x)^=1;
		num(x)=(r(x)-l(x)+1)-num(x);
		return;
	}
	if(d(x)) pushdown(x);
	int mid=(l(x)+r(x))>>1;
	if(l<=mid) modify(x<<1,l,r);
	if(r>mid) modify(x<<1|1,l,r);
	num(x)=num(x<<1)+num(x<<1|1);
}
int ask(int x,int l,int r){
	if(l<=l(x)&&r>=r(x)) return num(x);
	if(d(x)) pushdown(x);
	int ans=0,mid=(l(x)+r(x))>>1;
	if(l<=mid) ans+=ask(x<<1,l,r);
	if(r>mid) ans+=ask(x<<1|1,l,r);
	return ans;
}
int main(){
	scanf("%d%d",&n,&m);
	build(1,1,n);
	while(m--){
		int op,l,r;
		scanf("%d%d%d",&op,&l,&r);
		if(op==0) modify(1,l,r);
		else printf("%d\n",ask(1,l,r));
	}
	return 0;
}
```
---

## P2901 [USACO08MAR]Cow Jogging G(A*/k短路)

**题目描述**

贝西终于尝到了懒惰的后果，决定每周从谷仓到池塘慢跑几次来健身。当然，她不想跑得太累，所以她只打算从谷仓慢跑下山到池塘，然后悠闲地散步回谷仓。

同时，贝西不想跑得太远，所以她只想沿着通向池塘的最短路径跑步。一共有 $M$ 条道路，其中每一条都连接了两个牧场。这些牧场从 $1$ 到 $N$ 编号，如果 $X>Y$，则说明牧场 $X$ 的地势高于牧场 $Y$，即下坡的道路是从 $X$ 通向 $Y$ 的，$N$ 为贝西所在的牛棚（最高点），$1$ 为池塘（最低点）。

然而，一周之后，贝西开始对单调的路线感到厌烦，她希望可以跑不同的路线。比如说，她希望能有 $K$ 种不同的路线。同时，为了避免跑得太累，她希望这 $K$ 条路线是从牛棚到池塘的路线中最短的 $K$ 条。如果两条路线包含的道路组成的序列不同，则这两条路线被认为是不同的。

请帮助贝西算算她的训练强度，即将牧场网络里最短的 $K$ 条路径的长度分别算出来。你将会被提供一份牧场间路线的列表，每条道路用 $(X_i, Y_i, D_i)$ 表示，意为从 $X_i$ 到 $Y_i$ 有一条长度为 $D_i$ 的下坡道路。

**输入格式**

第一行三个用空格分开的整数 $N,M,K$，其中 。

第二行到第 $M+1$ 行每行有三个用空格分开的整数 $X_i,Y_i,D_i$，描述一条下坡的道路。

**输出格式**

共 $K$ 行，在第 $i$ 行输出第 $i$ 短的路线长度，如果不存在则输出 $-1$。如果出现多种有相同长度的路线，务必将其全部输出。

**样例 #1**

**样例输入 #1**

```
5 8 7 
5 4 1 
5 3 1 
5 2 1 
5 1 1 
4 3 4 
3 1 1 
3 2 1 
2 1 1
```

**样例输出 #1**

```
1 
2 
2 
3 
6 
7 
-1
```

**提示**

**样例 1 解释**

这些路线分别为 (5-1)、(5-3-1)、(5-2-1)、(5-3-2-1)、(5-4-3-1) 和 (5-4-3-2-1)。

**数据规模与约定**

对于全部的测试点，保证 $1 \le N \le 1,000$，$1 \le M \le 1\times10^4$，$1 \le K \le 100$，$1 \le Y_i < X_i\le N$，$1 \le D_i \le 1\times 10^6$，
```c++

```
---

## P3174 [HAOI2009] 毛毛虫(树的直径)

**题目描述**

对于一棵树，我们可以将某条链和与该链相连的边抽出来，看上去就象成一个毛毛虫，点数越多，毛毛虫就越大。例如下图左边的树（图 $1$）抽出一部分就变成了右边的一个毛毛虫了（图 $2$）。

![](https://cdn.luogu.com.cn/upload/pic/7967.png)

**输入格式**

输入中第一行两个整数 $N, M$，分别表示树中结点个数和树的边数。

接下来 $M$ 行，每行两个整数 $a, b$ 表示点 $a$ 和点 $b$ 有边连接（$a, b \le N$）。你可以假定没有一对相同的 $(a, b)$ 会出现一次以上。

**输出格式**

输出一行一个整数 , 表示最大的毛毛虫的大小。

**样例 #1**

**样例输入 #1**

```
13 12 
1 2 
1 5 
1 6 
3 2 
4 2 
5 7 
5 8 
7 9 
7 10 
7 11 
8 12 
8 13
```

**样例输出 #1**

```
11
```

**提示**

对于 $40\%$ 的数据，$1\leq N \le 50000$。

对于 $100\%$ 的数据，$1\leq N \le 300000$。
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
struct e{
	int to,next;
}edge[600005];
int ans=-1,root,n,m,cnt=0,calc[300005],head[300005];
void add_edge(int u,int v){
	edge[++cnt].to=v;
	edge[cnt].next=head[u];
	head[u]=cnt;
}
void predfs(int u,int fa,int sum){
	if(sum>ans){ans=sum; root=u;}
	for(int i=head[u];i;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		predfs(v,u,sum-2+calc[v]);
	}
}
void dfs(int u,int fa,int sum){
	if(sum>ans){ans=sum;}
	for(int i=head[u];i;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		dfs(v,u,sum-2+calc[v]);
	}
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) calc[i]=1;
	for(int i=1;i<=m;i++){
		int u,v;
		scanf("%d%d",&u,&v);
		calc[v]++,calc[u]++;
		add_edge(u,v); add_edge(v,u);
	}
	predfs(1,0,calc[1]);
	dfs(root,0,calc[root]);
	printf("%d",ans);
	return 0;
}
```

---
## P3554 [POI2013]LUK-Triumphal arch(动态规划/二分答案/树形DP)

**题目描述**

The king of Byteotia, Byteasar, is returning to his country after a victorious battle.

In Byteotia, there are ![](http://main.edu.pl/images/OI20/luk-en-tex.1.png) towns connected with only ![](http://main.edu.pl/images/OI20/luk-en-tex.2.png) roads.

It is known that every town can be reached from every other town by a unique route,    consisting of one or more (direct) roads.

    (In other words, the road network forms a tree).

The king has just entered the capital.

Therein a triumphal arch, i.e., a gate a victorious king rides through, has been erected.

Byteasar, delighted by a warm welcome by his subjects, has planned a    triumphal procession to visit all the towns of Byteotia, starting with the capital he is currently in.

The other towns are not ready to greet their king just yet -    the constructions of the triumphal arches in those towns did not even begin!

But Byteasar's trusted advisor is seeing to the issue.

    He desires to hire a number of construction crews.

    Every crew can construct a single arch each day, in any town.

    Unfortunately, no one knows the order in which the king will visit the towns.

The only thing that is clear is that every day the king will travel from the city he is currently in to a neighboring one.

The king may visit any town an arbitrary number of times    (but as he is not vain, one arch in each town will suffice).

Byteasar's advisor has to pay each crew the same flat fee, regardless of how many arches this crew builds.

Thus, while he needs to ensure that every town has an arch when it is visited by the king, he wants to hire as few crews as possible.

Help him out by writing a program that will determine the minimum number    of crews that allow a timely delivery of the arches.


给一颗 $n$ 个节点的树（$n \le 3 \times 10^5$），初始时 $1$ 号节点被染黑，其余是白的。两个人轮流操作，一开始 B 在 $1$ 号节点。每一轮，A 选择 $k$ 个点染黑，然后 B 走到一个相邻节点，如果 B 当前处于白点则 B 胜，否则当 A 将所有点染为黑点时 A 胜。求能让 A 获胜的最小的 $k$ 。

**输入格式**

The first line of the standard input contains a single integer ![](http://main.edu.pl/images/OI20/luk-en-tex.3.png)    (![](http://main.edu.pl/images/OI20/luk-en-tex.4.png)), the number of towns in Byteotia.

The towns are numbered from 1 to ![](http://main.edu.pl/images/OI20/luk-en-tex.5.png), where the number 1 corresponds to the capital.

The road network is described in ![](http://main.edu.pl/images/OI20/luk-en-tex.6.png) lines that then follow.

Each of those lines contains two integers, ![](http://main.edu.pl/images/OI20/luk-en-tex.7.png)    (![](http://main.edu.pl/images/OI20/luk-en-tex.8.png)), separated by a single space,    indicating that towns ![](http://main.edu.pl/images/OI20/luk-en-tex.9.png) and ![](http://main.edu.pl/images/OI20/luk-en-tex.10.png) are directly connected with a two way road.

In tests worth 50% of the total points, an additional condition ![](http://main.edu.pl/images/OI20/luk-en-tex.11.png) holds.

**输出格式**

The first and only line of the standard output is to hold a single integer,    the minimum number of crews that Byteasar's advisor needs to hire.

**样例 #1**

**样例输入 #1**

```
7
1 2
1 3
2 5
2 6
7 2
4 1
```

**样例输出 #1**

```
3
```

**解析**

首先看出 $k$ 具有单调性，因此采用二分答案将最优性问题转化为判定性问题。

其次，B 一定不会走回头路。因为走回头路就说明 B 的某一次选择不是所有选择中最优的，那 B 为啥不在那次选择时就采取最优策略呢？走回头路只会多给 A 染色的机会，那 B 的胜算就更小了。

既然 B 不会走回头路，那么当 B 走到结点 $i$ 时，A 必须在 B 做下一次选择之前把结点 $i$ 的所有儿子染成黑色，不然 B 下一次走的时候走到结点 $i$ 没来得及染成黑色的儿子就胜利了。但是只用一次染色可能无法让结点 $i$ 的所有儿子都变成黑色，这时候就要未雨绸缪，在之前几次染色时提前把这些没法染成黑色的儿子染成黑色。换句话说，我们需要向“上一级”申请支援，因为以 $i$ 为根的子树内部已经无法“消化”掉需要染黑的结点了。

那未雨绸缪的时机又该如何确定呢？假设在 B 走到结点 $j$ 时我们发现结点 $j$ 的儿子数小于 $k$，也就是说染色的次数会产生剩余，那我们就需要看一下结点 $j$ 的儿子里有没有需要支援的，把剩余的次数分配给它们。如果剩余的次数仍然够，那就又要向 $j$ 的“上一级”求援了。

令 $f_{i}$表示以 $i$ 为根的子树（不包括 i）需要多少次数的支援，$f_{i}=cnt_{i}-k+\sum \max(f_{p},0)$。其中 $cnt_{i}$为 $i$ 的儿子数量，$p$ 为 $i$ 的儿子结点。如果 $f_{1}\le 0$，那么当前二分到的 $k$ 就是合法的，否则就是不合法的。

二分的复杂度为 $O(\log v)$，v 为值域。一次 dp 复杂度为 $O(n)$，总复杂度为 $O(n\log v)$。这里我又对值域做了个小优化，因为结点 1 的所有儿子在第一次染色中一定要全被染成黑色而且不可能有支援，因此二分答案的下界可以取 $cnt_{1}$。同时，如果 $a$ 不小于任一结点的儿子数量，那么 $k=a$ 一定是一个合法的答案，二分答案的上界只需要取到 $a$。
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
struct e{
	int to,next;
}edge[600005];
int cnt=0,n,maxn=-1,head[300005],dp[300005],son[300005],ans=1e9;
void add_edge(int u,int v){
	edge[++cnt].next=head[u];
	edge[cnt].to=v;
	head[u]=cnt;
}
void predfs(int u,int fa){
	for(int i=head[u];i;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		son[u]++;
		predfs(v,u);
	}
}
void dfs(int u,int fa,int k){
	dp[u]=son[u]-k;
	for(int i=head[u];i;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		dfs(v,u,k);
		if(dp[v]>0) dp[u]+=dp[v];  //dp[v]<0 的话就不能加，因为下面并不需要支援，而这里能提供的支援又会被扩大，就不正确了
	}
}
int main(){
	scanf("%d",&n);
	for(int i=1;i<n;i++){
		int u,v; scanf("%d%d",&u,&v);
		add_edge(u,v); add_edge(v,u);
	}
	predfs(1,0);
	for(int i=1;i<=n;i++) maxn=max(maxn,son[i]);
	int l=son[1],r=maxn;
	while(l<=r){
		int mid=(l+r)>>1;
		dfs(1,0,mid);
		if(dp[1]<=0) r=mid-1,ans=min(ans,mid);
		else l=mid+1;
	} 
	printf("%d",ans);
	return 0;
}
```
---
## P3592 [POI2015] MYJ(动态规划/离散化/区间DP)

**题目描述**

有 $n$ 家洗车店从左往右排成一排，每家店都有一个正整数价格 $p_i$。有 $m$ 个人要来消费，第 $i$ 个人会驶过第 $a_i$ 个开始一直到第 $b_i$ 个洗车店，且会选择这些店中最便宜的一个进行一次消费。但是如果这个最便宜的价格大于 $c_i$，那么这个人就不洗车了。请给每家店指定一个价格，使得所有人花的钱的总和最大。

**输入格式**

第一行包含两个正整数 $n,m$（$1 \le n \le 50$，$1 \le m \le 4000$）。接下来 $m$ 行，每行包含三个正整数 $a_i,b_i,c_i$（$1 \le a_i \le b_i \le n$，$1 \le c_i \le 5\times 10^5$）。

**输出格式**

第一行输出一个正整数，即消费总额的最大值。第二行输出 $n$ 个正整数，依次表示每家洗车店的价格 $p_i$，要求 $1 \le p_i \le 5\times 10^5$。若有多组最优解，输出任意一组。

**样例 #1**

**样例输入 #1**

```
7 5
1 4 7
3 7 13
5 6 20
6 7 1
1 2 5
```

**样例输出 #1**

```
43
5 5 13 13 20 20 13
```

**提示**

原题名称：Myjnie。

**解析**

设 $dp[i][j][k]$ 为在区间 $[i,j]$ 内，最低价格为 $k$ 时的最大收益 

则转移方程：

$dp[i][j][k]=\begin{cases} dp[i][j][k+1] \\\ max(dp[i][l-1][k]+dp[l+1][j][k]+val(i,j,l,k)(i<=l<=j))\end{cases}$

第一种转移：价格从大到小逐层传递，虽然人可能少一些，但价格更高

第二种转移： $val(i,j,k,l)$ 表示在 $[i,j]$ 的区间内，最小值在 $l$ 处，最小值为 $k$ 时的收益，
枚举断点 $val(i,j,k,l)=cnt(i,j,k,l)\times k$ , $cnt(i,j,k,l)$ 表示在 $[i,j]$ 的区间内 ， 最小值在 $l$ 处 ，最小值为 $k$ 时 ，在 $l$ 处消费的顾客数

所以最终答案为 $dp[1][n][1]$ ,因为最后一维的 $k$ (现在是 1) ，是在 dp 过程中将上面的 $(k+1,k+2...)$ 已经传递了
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
int n,m,a[4005],b[4005],c[4005],d[4005],tot;
int g[55][4005],dp[55][55][4005],val[55][55][4005],num[55][55][4005],ans[55]; 
/* 
1.g[i][j]表示在第 i 个洗车店 ,价格为 j ,会有多少人来  
2.dp[i][j][k] 表示在 [i,j] 这个区间内 ，最低价格为 k 时的最大收益 
3.val[i][j][k] 表示当 dp[i][j][k] 取到最优值时，价格为多少
  (取到最优值时价格不一定为 k ，因为最优值可能从 dp[i][j][k+1] 转移而来，也就是从更高的价格转移而来)
4.num[i][j][k] 表示当 dp[i][j][k] 取到最优值时，断点是哪里 
*/ 
void print(int l,int r,int k){
	if(l>r) return;
	int p=num[l][r][k=val[l][r][k]];
	ans[p]=d[k]; //是 d[k] ,而不是 k ,k是离散化后的价格
	print(l,p-1,k),print(p+1,r,k); 
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++) scanf("%d%d%d",&a[i],&b[i],&c[i]),d[i]=c[i];
	sort(d+1,d+m+1);
	tot=unique(d+1,d+m+1)-d-1;
	for(int i=1;i<=m;i++) c[i]=lower_bound(d+1,d+tot+1,c[i])-d; //离散化
	for(int len=1;len<=n;len++)
	  for(int i=1;i+len-1<=n;i++){
	  	int j=i+len-1;
	  	for(int k=i;k<=j;k++)
	  	  for(int t=1;t<=tot;t++)
	  	    g[k][t]=0;  //将数组清空 
	  	for(int p=1;p<=m;p++) //枚举第 p 个人
		  if(i<=a[p]&&b[p]<=j) //判断这个人的区间是否只在 [i,j] 里
		    for(int k=a[p];k<=b[p];k++)
			  g[k][c[p]]++;    
		for(int k=i;k<=j;k++)
		  for(int t=tot-1;t;t--) //从大到小枚举 
		    g[k][t]+=g[k][t+1];  //若 t+1 的价格都可以，那么 t 也可以 
		for(int k=tot;k;k--){  //枚举最低价格，从大往小，因为小的价格可能从更高的价格转移而来 
		  int ans=0,maxn=0;
		  for(int l=i;l<=j;l++) //枚举断点
		    if((ans=dp[i][l-1][k]+dp[l+1][j][k]+g[l][k]*d[k])>=maxn) maxn=ans,num[i][j][k]=l;
		  if(maxn<dp[i][j][k+1]) //和上一次那个比较，也就是转移方程里的第一种情况
		    dp[i][j][k]=dp[i][j][k+1],val[i][j][k]=val[i][j][k+1];
		  else dp[i][j][k]=maxn,val[i][j][k]=k;
		}
	  } 
	  printf("%d\n",dp[1][n][1]);
	  print(1,n,1);
	  for(int i=1;i<=n;i++) printf("%d ",ans[i]);
	  return 0;
} 
```

---
## P3594 [POI2015] WIL(单调队列/双指针)

**题目描述**

给定一个长度为 $n$ 的序列，你有一次机会选中一段连续的长度不超过 $d$ 的区间，将里面所有数字全部修改为 $0$。请找到最长的一段连续区间，使得该区间内所有数字之和不超过 $p$。

**输入格式**

输入的第一行包含三个整数，分别代表 $n,p,d$。

第二行包含 $n$ 个整数，第 $i$ 个整数代表序列中第 $i$ 个数 $w_i$。

**输出格式**

包含一行一个整数，即修改后能找到的最长的符合条件的区间的长度。

**样例 #1**

**样例输入 #1**

```
9 7 2
3 4 1 9 4 1 7 1 3
```

**样例输出 #1**

```
5
```

**提示**

**【数据范围】**

对于 $100\%$ 的数据，$1 \le d \le n \le 2 \times 10^6$，$0 \le p \le 10^{16}$，$1 \leq w_i \leq 10^9$。

原题名称：Wilcze doły。

```c++
#include<cstdio>
#include<algorithm>
using namespace std;
typedef long long ll;
ll n,p,d,a[2000005],sum[2000005],ans,q[2000005],h,t,l;
int main(){
	scanf("%lld%lld%lld",&n,&p,&d);
	for(int i=1;i<=n;i++) scanf("%lld",&a[i]),sum[i]=sum[i-1]+a[i];
	ans=d,l=1,q[0]=d;
	for(int i=d+1;i<=n;i++){
		while(h<=t&&sum[i]-sum[i-d]>=sum[q[t]]-sum[q[t]-d]) t--;
		q[++t]=i;
		while(h<=t&&sum[i]-sum[l-1]-(sum[q[h]]-sum[q[h]-d])>p){
			l++;
			while(h<=t&&q[h]-d+1<l) h++;
		}
		ans=max(ans,i-l+1);
	}
	printf("%lld",ans);
	return 0;
}
```

---

## P3718 [AHOI2017初中组]alter(二分答案)

**题目描述**

有N盏灯排成一列，其中有些灯开着，有些灯关着。小可可希望灯是错落有致的，他定义一列灯的状态的不优美度为这些灯中最长的连续的开着或关着的灯的个数。小可可最多可以按开关k次，每次操作可以使该盏灯的状态取反：原来开着的就关着，反之开着。现在给出这些灯的状态，求操作后最小的不优美度。

**输入格式**

第一行两个整数n,k

第二行是一个长度为n的字符串，其中有两种字符：N和F。其中N表示该灯开着，F表示该灯关着

**输出格式**

最小的不优美度

**样例 #1**

**样例输入 #1**

```
8 1
NNNFFNNN
```

**样例输出 #1**

```
3
```

**提示**

30%的数据：1 ≤ K ≤ N ≤ 20；

50%的数据：1 ≤ K ≤ N ≤ 300；

另有15%的数据：1 ≤ N ≤ 100000，字符串为全N或全F；

100%的数据：1 ≤ K ≤ N ≤ 100000。

```c++
#include<cstdio>
#include<algorithm>
using namespace std;
int n,k,cnt=0,s1=0,s2=0,a[100005]={0},ans=1e9;
//a[i] 表示原 str 序列里第 i 组连续的字母内的连续的字母数目 
//cnt 表示一共有多少组连续字母 
char str[100005];
bool chck(int x){
	int ans=0;
	for(int i=1;i<=cnt;i++) ans+=a[i]/(x+1);
	if(ans<=k) return true;
	else return false;
}
int main(){
	scanf("%d%d",&n,&k);
	scanf("%s",str+1);
	for(int i=1;i<=n;i++) if((i%2==0&&str[i]=='N')||(i%2==1&&str[i]=='F')) s1++;
	for(int i=1;i<=n;i++) if((i%2==1&&str[i]=='N')||(i%2==0&&str[i]=='F')) s2++;
	if(s1<=k||s2<=k){printf("%d",1); return 0;}
	//以上特判表示在 k 步以内可以将序列变为不优美度为 1 的序列 
	for(int i=1;i<=n;i++){
		a[++cnt]++;
		while(str[i]==str[i+1]){a[cnt]++;i++;}
	}
	int l=2,r=n; //left 端点是 2 ,因为 1的情况上面已经特判过了 
	while(l<=r){
		int mid=(l+r)>>1;
		if(chck(mid)) r=mid-1,ans=min(ans,mid);
		else l=mid+1;
	}
	printf("%d",ans);
	return 0;
} 
```
---
## P3743 kotori的设备(二分答案)

**题目背景**

kotori 有 n 个可同时使用的设备。

**题目描述**

第 i 个设备每秒消耗ai个单位能量。能量的使用是连续的，也就是说能量不是某时刻突然消耗的，而是匀速消耗。也就是说，对于任意实数 ,在 k 秒内消耗的能量均为k\*ai 单位。在开始的时候第 i 个设备里存储着bi个单位能量。

同时 kotori 又有一个可以给任意一个设备充电的充电宝，每秒可以给接通的设备充能p 个单位，充能也是连续的，不再赘述。你可以在任意时间给任意一个设备充能，从一个设备切换到另一个设备的时间忽略不计。

kotori 想把这些设备一起使用，直到其中有设备能量降为 0。所以 kotori 想知道，

在充电器的作用下，她最多能将这些设备一起使用多久。

**输入格式**

第一行给出两个整数 n,p。

接下来 n 行，每行表示一个设备，给出两个整数，分别是这个设备的ai 和 bi。

**输出格式**

如果 kotori 可以无限使用这些设备，输出-1。

否则输出 kotori 在其中一个设备能量降为 0 之前最多能使用多久。

设你的答案为 a,标准答案为 b,只有当 a,b 满足 ![](https://cdn.luogu.com.cn/upload/pic/5170.png)  的时候，你能得到本测

试点的满分。

**样例 #1**

**样例输入 #1**

```
2 1
2 2
2 1000
```

**样例输出 #1**

```
2.0000000000
```

**样例 #2**

**样例输入 #2**

```
1 100
1 1
```

**样例输出 #2**

```
-1
```

**样例 #3**

**样例输入 #3**

```
3 5
4 3
5 2
6 1
```

**样例输出 #3**

```
0.5000000000
```
**提示**

对于 100%的数据， 1<=n<=100000，1<=p<=100000，1<=ai,bi<=100000。
```c++
#include<cstdio>
#include<algorithm>
#include<iostream>
using namespace std;
int n;
double a[100005],b[100005],sum=0,p;
bool chck(double x){
	double s=x*p,s2=0;
	for(int i=1;i<=n;i++){
		if(a[i]*x<=b[i]) continue;
		s2+=a[i]*x-b[i];
	}
    return s2<s;
}
int main(){
	scanf("%d%lf",&n,&p);
	for(int i=1;i<=n;i++) scanf("%lf%lf",&a[i],&b[i]),sum+=a[i];
	if(sum<=p){printf("-1"); return 0;}
	double l=0,r=1e10;
	while(r-l>1e-6){
		double mid=(l+r)/2;
		if(chck(mid)) l=mid;
		else r=mid;
	}
	printf("%.6lf",l);
	return 0;
}
```
---
## P3853 [TJOI2007]路标设置(二分答案)

**题目背景**

B 市和 T 市之间有一条长长的高速公路，这条公路的某些地方设有路标，但是大家都感觉路标设得太少了，相邻两个路标之间往往隔着相当长的一段距离。为了便于研究这个问题，我们把公路上相邻路标的最大距离定义为该公路的 “空旷指数”。

**题目描述**

现在政府决定在公路上增设一些路标，使得公路的“空旷指数”最小。他们请求你设计一个程序计算能达到的最小值是多少。请注意，公路的起点和终点保证已设有路标，公路的长度为整数，并且原有路标和新设路标都必须距起点整数个单位距离。

**输入格式**

第 1 行包括三个数 L、N、K，分别表示公路的长度，原有路标的数量，以及最多可增设的路标数量。


第 2 行包括递增排列的 N 个整数，分别表示原有的 N 个路标的位置。路标的位置用距起点的距离表示，且一定位于区间 [0,L] 内。

**输出格式**

输出1行，包含一个整数，表示增设路标后能达到的最小“空旷指数”值。

**样例 #1**

**样例输入 #1**

```
101 2 1
0 101
```

**样例输出 #1**

```
51
```

**提示**

公路原来只在起点和终点处有两个路标，现在允许新增一个路标，应该把新路标设在距起点50或51个单位距离处，这样能达到最小的空旷指数51。


50%的数据中，2 ≤ N ≤100，0 ≤K ≤100

100%的数据中，2 ≤N ≤100000, 0 ≤K ≤100000

100%的数据中，0 ＜ L ≤10000000
```c++
/*
此类二分答案一般都是二分题目所想要最小化 / 最大化的那个值，
然后判断其可行性，从而调整二分左右端点 
*/
#include<cstdio>
#include<algorithm>
#include<cmath>
using namespace std;
typedef long long ll;
ll l,n,k,pos[100005],maxn=-1e18,ans=1e18;
bool chck(ll x){
	ll sum=0;
	for(int i=2;i<=n;i++)
	  if(pos[i]-pos[i-1]>=x){
		sum+=(pos[i]-pos[i-1])/x;
		if((pos[i]-pos[i-1])%x==0) sum--; //很重要 
	  }
	if(sum<=k) return true;
	else return false;
}
int main(){
	scanf("%lld%lld%lld",&l,&n,&k);
	for(int i=1;i<=n;i++) scanf("%lld",&pos[i]),maxn=max(maxn,pos[i]-pos[i-1]);
	if(k==0){
		printf("%lld",maxn);
		return 0;
	}
	ll l=0,r=maxn;
	while(l<=r){
		ll mid=(l+r)>>1;
		if(chck(mid)) r=mid-1,ans=min(ans,mid);
		else l=mid+1;
	}
	printf("%lld",ans);
	return 0;
}
```
---
## P3959 [NOIP2017 提高组] 宝藏(动态规划/状态压缩DP)

**题目描述**

参与考古挖掘的小明得到了一份藏宝图，藏宝图上标出了 $n$ 个深埋在地下的宝藏屋， 也给出了这 $n$ 个宝藏屋之间可供开发的 $m$ 条道路和它们的长度。

小明决心亲自前往挖掘所有宝藏屋中的宝藏。但是，每个宝藏屋距离地面都很远，也就是说，从地面打通一条到某个宝藏屋的道路是很困难的，而开发宝藏屋之间的道路则相对容易很多。

小明的决心感动了考古挖掘的赞助商，赞助商决定免费赞助他打通一条从地面到某个宝藏屋的通道，通往哪个宝藏屋则由小明来决定。

在此基础上，小明还需要考虑如何开凿宝藏屋之间的道路。已经开凿出的道路可以 任意通行不消耗代价。每开凿出一条新道路，小明就会与考古队一起挖掘出由该条道路所能到达的宝藏屋的宝藏。另外，小明不想开发无用道路，即两个已经被挖掘过的宝藏屋之间的道路无需再开发。

新开发一条道路的代价是 $\mathrm{L} \times \mathrm{K}$。其中 $L$ 代表这条道路的长度，$K$ 代表从赞助商帮你打通的宝藏屋到这条道路起点的宝藏屋所经过的宝藏屋的数量（包括赞助商帮你打通的宝藏屋和这条道路起点的宝藏屋） 。

请你编写程序为小明选定由赞助商打通的宝藏屋和之后开凿的道路，使得工程总代价最小，并输出这个最小值。

**输入格式**

第一行两个用空格分离的正整数 $n,m$，代表宝藏屋的个数和道路数。

接下来 $m$ 行，每行三个用空格分离的正整数，分别是由一条道路连接的两个宝藏屋的编号（编号为 $1-n$），和这条道路的长度 $v$。

**输出格式**

一个正整数，表示最小的总代价。

**样例 #1**

**样例输入 #1**

```
4 5 
1 2 1 
1 3 3 
1 4 1 
2 3 4 
3 4 1
```

**样例输出 #1**

```
4
```

**样例 #2**

**样例输入 #2**

```
4 5 
1 2 1 
1 3 3 
1 4 1 
2 3 4 
3 4 2
```

**样例输出 #2**

```
5
```

**提示**

![](https://cdn.luogu.com.cn/upload/pic/10868.png) 

【样例解释 $1$】

小明选定让赞助商打通了 $1$ 号宝藏屋。小明开发了道路 $1 \to 2$，挖掘了 $2$ 号宝藏。开发了道路 $1 \to 4$，挖掘了 $4$ 号宝藏。还开发了道路 $4 \to 3$，挖掘了 $3$ 号宝藏。

工程总代价为 $1 \times 1 + 1 \times 1 + 1 \times 2  = 4 $。

【样例解释 $2$】

小明选定让赞助商打通了 $1$ 号宝藏屋。小明开发了道路 $1 \to 2$，挖掘了 $2$ 号宝藏。开发了道路 $1 \to 3$，挖掘了 $3$ 号宝藏。还开发了道路 $1 \to 4$，挖掘了 $4$ 号宝藏。

工程总代价为 $1 \times 1 + 3 \times 1 + 1 \times 1  = 5$。


【数据规模与约定】

对于 $ 20\%$ 的数据： 保证输入是一棵树，$1 \le n \le 8$，$v \le 5\times 10^3$ 且所有的 $v$ 都相等。

对于 $40\%$ 的数据： $1 \le n \le 8$，$0 \le m \le 10^3$，$v \le 5\times 10^3$ 且所有的 $v$ 都相等。

对于 $ 70\%$ 的数据： $1 \le n \le 8$，$0 \le m \le 10^3$，$v \le  5\times 10^3$。

对于 $ 100\%$ 的数据： $1 \le n \le 12$，$0 \le m \le 10^3$，$v \le  5\times 10^5$。
```c++
#include<cstdio>
#include<algorithm>
#include<cstring>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=(1<<13);
int n,m,dis[13][13],extend[N],dp[N][13],ans=INF;  
//extend[i] 表示从集合 i 中每一个节点出发，所能遍历到的所有节点 
//dp[i][j] 表示当前选取的节点状态集合为 i ,已经打通的节点的最大深度为 j 时的最小花费 
int main(){
	scanf("%d%d",&n,&m);
	memset(dis,0x3f,sizeof dis);
	memset(dp,0x3f,sizeof dp);
	for(int i=1;i<=m;i++){
		int a,b,v; scanf("%d%d%d",&a,&b,&v);
		a--,b--;  //方便后面处理 
		dis[b][a]=dis[a][b]=min(dis[a][b],v);
	}
	int maxn=(1<<n)-1;
	for(int i=1;i<=maxn;i++)
	  for(int j=0;j<n;j++)
		if(((1<<j)|i)==i){  //枚举集合 i 中每一个节点 
		  dis[j][j]=0;  //这样在下面第二排判断时才会把 j 号节点算上 
		  for(int k=0;k<n;k++)
		  	if(dis[j][k]!=INF) extend[i]|=(1<<k);
		}
	for(int i=0;i<n;i++) dp[(1<<i)][0]=0; //初始化：将 i 号点作为根节点
	for(int i=2;i<=maxn;i++)
	  for(int s=i-1;s;s=(s-1)&i) 
	  	if((extend[s]|i)==extend[s]){
	  	  int ss=i^s; //新扩展的节点集合 
	  	  int sum=0;
		  for(int k=0;k<n;k++)
		  	if((1<<k)&ss){ //枚举新扩展的节点 
		  	  int now=INF;
		  	  for(int t=0;t<n;t++) //枚举已挖通的节点 
		  	  	if((1<<t)&s) now=min(now,dis[t][k]);
		  	  sum+=now;  //打通这个新扩展的节点至少要多走的长度 
	        }
		  for(int j=1;j<n;j++)  if(dp[s][j-1]!=INF)
		      dp[i][j]=min(dp[i][j],dp[s][j-1]+j*sum); //不要忘记乘上 d 
	  	}
	for(int i=0;i<n;i++) ans=min(ans,dp[maxn][i]);
	printf("%lld",ans);
	return 0;
}
```
---
## P4317 花神的数论题(动态规划/进制/数位DP)

**题目背景**

众所周知，花神多年来凭借无边的神力狂虐各大 OJ、OI、CF、TC …… 当然也包括 CH 啦。

**题目描述**

话说花神这天又来讲课了。课后照例有超级难的神题啦…… 我等蒟蒻又遭殃了。 花神的题目是这样的：设  $\text{sum}(i)$  表示  $i$  的二进制表示中  $1$  的个数。给出一个正整数  $N$  ，花神要问你  $\prod_{i=1}^{N}\text{sum}(i)$ ，也就是  $\text{sum}(1)\sim\text{sum}(N)$  的乘积。

**输入格式**

一个正整数 $N$。

**输出格式**

一个数，答案模 $10000007$ 的值。

**样例 #1**

**样例输入 #1**

```
3
```

**样例输出 #1**

```
2
```

**提示**

对于 $100\%$ 的数据，$1\le N\le 10^{15}$。
```c++
#include<cstdio>
#include<algorithm>
#include<cstring>
using namespace std;
typedef long long ll;
const int mod=1e7+7;
ll n,len=0,a[55],dp[55][55];  
//数组要开大一点，因为是存二进制，要开到log2(1e15)
ll dfs(int pos,int sum,int limit){
	if(pos>len) return max(1,sum); //乘上 0的话就全盘覆没了
	if(dp[pos][sum]!=-1&&!limit) return dp[pos][sum];
	int ret=limit?a[len-pos+1]:1; ll ans=1;
	for(int i=0;i<=ret;i++)
	  ans=(ans*dfs(pos+1,sum+(i==1),limit&(i==ret)))%mod;
	if(!limit) dp[pos][sum]=ans;
	return ans;
}
int main(){
	scanf("%lld",&n);
	while(n){
		a[++len]=n&1;
		n>>=1;
	}
	memset(dp,-1,sizeof dp); 
	printf("%lld",dfs(1,0,1));
	return 0;
}
```
---

## P4392 [BOI2007]Sound 静音问题(单调队列/RMQ/线段树)

**题目描述**

数字录音中，声音是用表示空气压力的数字序列描述的，序列中的每个值称为一个采样，每个采样之间间隔一定的时间。 

很多声音处理任务都需要将录到的声音分成由静音隔开的几段非静音段。为了避免分成过多或者过少的非静音段，静音通常是这样定义的：m个采样的序列，该序列中采样的最大值和最小值之差不超过一个特定的阈值c。 

请你写一个程序，检测n个采样中的静音。

**输入格式**

第一行有三个整数n，m，c（ 1<= n<=1000000，1<=m<=10000， 0<=c<=10000），分别表示总的采样数、静音的长度和静音中允许的最大噪音程度。

第2行n个整数ai (0 <= ai <= 1,000,000)，表示声音的每个采样值，每两个整数之间用空格隔开。

**输出格式**

列出了所有静音的起始位置i（i满足max(a[i, . . . , i+m−1]) − min(a[i, . . . , i+m−1]) <= c），每行表示一段静音的起始位置，按照出现的先后顺序输出。如果没有静音则输出NONE。

**样例 #1**

**样例输入 #1**

```
7 2 0
0 1 1 2 3 2 2
```

**样例输出 #1**

```
2
6
```
```c++
#include<cstdio>
#include<algorithm>
#define MAXN 1000005
using namespace std;
struct sgtree{
	int l,r,maxn,minn;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define maxn(x) tree[x].maxn
	#define minn(x) tree[x].minn
}tree[4*MAXN];
int n,m,c;
bool flag=false;
void build(int x,int l,int r){
	l(x)=l,r(x)=r;
	if(l==r){scanf("%d",&maxn(x)); minn(x)=maxn(x); return;}
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	maxn(x)=max(maxn(x<<1),maxn(x<<1|1));
	minn(x)=min(minn(x<<1),minn(x<<1|1));
}
int ask(int x,int l,int r,int op){
	if(l<=l(x)&&r>=r(x)){
		if(op==1) return maxn(x);
		else return minn(x);
	}
	int mx=-1e9,mn=1e9;
	int mid=(l(x)+r(x))>>1;
	if(l<=mid) mx=max(ask(x<<1,l,r,op),mx),
	           mn=min(ask(x<<1,l,r,op),mn);
	if(r>mid) mx=max(mx,ask(x<<1|1,l,r,op)),
	          mn=min(mn,ask(x<<1|1,l,r,op));
	if(op==1) return mx;
	else return mn;
}
int main(){
	scanf("%d%d%d",&n,&m,&c);
	build(1,1,n);
	for(int i=1;i+m-1<=n;i++)
	  if(ask(1,i,i+m-1,1)-ask(1,i,i+m-1,2)<=c){
		flag=true;
		printf("%d\n",i);
	  }
	if(!flag) printf("NONE\n");
	return 0;
}
```
---
## P4408 [NOI2003] 逃学的小孩(树的直径/贪心)

**题目描述**

Chris 家的电话铃响起了，里面传出了 Chris 的老师焦急的声音：“喂，是 Chris 的家长吗？你们的孩子又没来上课，不想参加考试了吗？”一听说要考试，Chris 的父母就心急如焚，他们决定在尽量短的时间内找到 Chris。他们告诉 Chris 的老师：“根据以往的经验，Chris 现在必然躲在朋友 Shermie 或 Yashiro 家里偷玩《拳皇》游戏。现在，我们就从家出发去找 Chris，一但找到，我们立刻给您打电话。”说完砰的一声把电话挂了。

Chris 居住的城市由 $N$ 个居住点和若干条连接居住点的双向街道组成，经过街道 $x$ 需花费 $T_{x}$ 分钟。可以保证，任两个居住点间有且仅有一条通路。Chris 家在点 $C$，Shermie 和 Yashiro 分别住在点 $A$ 和点 $B$。Chris 的老师和 Chris 的父母都有城市地图，但 Chris 的父母知道点 $A$、$B$、$C$ 的具体位置而 Chris 的老师不知。

为了尽快找到 Chris，Chris 的父母会遵守以下两条规则：

1. 如果 $A$ 距离 $C$ 比 $B$ 距离 $C$ 近，那么 Chris 的父母先去 Shermie 家寻找 Chris，如果找不到，Chris 的父母再去 Yashiro 家；反之亦然。
2. Chris 的父母总沿着两点间唯一的通路行走。

显然，Chris 的老师知道 Chris 的父母在寻找 Chris 的过程中会遵守以上两条规则，但由于他并不知道 $A$、$B$、$C$ 的具体位置，所以现在他希望你告诉他，最坏情况下 Chris的父母要耗费多长时间才能找到 Chris？

**输入格式**

输入文件第一行是两个整数 $N$ 和 $M$，分别表示居住点总数和街道总数。

以下 $M$ 行，每行给出一条街道的信息。第 $i+1$ 行包含整数 $U_{i}$、$V_{i}$、$T_{i}$，表示街道 $i$ 连接居住点 $U_{i}$ 和 $V_{i}$，并且经过街道 $i$ 需花费 $T_{i}$ 分钟。街道信息不会重复给出。

**输出格式**

输出文件仅包含整数 $T$，即最坏情况下 Chris 的父母需要花费 $T$ 分钟才能找到 Chris。

**样例 #1**

**样例输入 #1**

```
4 3
1 2 1
2 3 1
3 4 1
```

**样例输出 #1**

```
4
```

**提示**

对于 $100\%$ 的数据，$3 \le N \le 2\times 10^5$，$1 \le U_{i},V_{i} \le N$，$1 \le T_{i} \le 10^{9}$。
```c++
//贪心证明参考 https://www.luogu.com.cn/blog/ILikeDuck/solution-p4408
#include<cstdio>
#include<algorithm>
using namespace std;
typedef long long ll;
struct e{
	int to,next; ll w;
}edge[400005];
ll cnt=0,n,m,head[200005],f[200005],maxn,root[2],dist[200005][2],ans;
void add_edge(int u,int v,ll w){
	edge[++cnt].to=v;
	edge[cnt].next=head[u];
	edge[cnt].w=w;
	head[u]=cnt;
}
void dfs1(int u,int fa,int k,ll sum){
	if(sum>maxn){maxn=sum; root[k]=u;} 
	for(int i=head[u];i;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		dfs1(v,u,k,sum+edge[i].w);
	}
}
void dfs(int u,int fa,int k){
	for(int i=head[u];i;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		dist[v][k]=dist[u][k]+edge[i].w;
		dfs(v,u,k);
	}
}
int main(){
	scanf("%lld%lld",&n,&m);
	for(int i=1;i<=m;i++){
		ll u,v,w;
		scanf("%lld%lld%lld",&u,&v,&w);
		add_edge(u,v,w);
		add_edge(v,u,w);
	}
	dfs1(1,0,0,0);
	maxn=0;
	dfs1(root[0],0,1,0);
	dfs(root[0],0,0);
	dfs(root[1],0,1);
	for(int i=1;i<=n;i++) ans=max(ans,min(dist[i][0],dist[i][1])); //贪心 
	printf("%lld",maxn+ans);
	return 0;
}
```
---
## P4954 [USACO09OPEN]Tower of Hay G(单调队列/动态规划)

**题目背景**

为了调整电灯亮度，贝西要用干草包堆出一座塔，然后爬到牛棚顶去把灯泡换掉。干草包会从传送带上运来，共会出现N包干草，第i包干草的宽度是W i ，高度和长度统一为1。干草塔要从底层开始铺建。贝西会选择最先送来的若干包干草，堆在地上作为第一层，然后再把紧接着送来的几包干草包放在第二层， 再铺建第三层……重复这个过程， 一直到所有的干
草全部用完。每层的干草包必须紧靠在一起，不出现缝隙，而且为了建筑稳定，上层干草的宽度不能超过下层的宽度。 按顺序运来的干草包一定要都用上， 不能将其中几个干草包弃置不用。贝西的目标是建一座最高的塔，请你来帮助她完成这个任务吧。

**题目描述**

**输入格式**

第一行：单个整数：$N$，$1 ≤ N ≤ 100000$
第二行到$N + 1$行：第$i + 1$行有一个整数$W_i$ ，$1 ≤ W_i ≤ 10000$

**输出格式**

第一行：单个整数，表示可以建立的最高高度

**样例 #1**

**样例输入 #1**

```
3
1
2
3
```

**样例输出 #1**

```
2
```

**提示**

将 1 和 2 放在第一层，将 3 放在第二层
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
const int N=100005;
int n,dp[N],g[N],w[N],q[N],sum[N];
//dp[i]表示取前 i 个物品时（当然这里是倒叙取）,所能搭建的最高高度,g[i]则表示此时 i 所在的这一层
//的宽度 
int main(){
	scanf("%d",&n);
	for(int i=n;i>=1;i--) scanf("%d",&w[i]); //倒序存放 
	for(int i=1;i<=n;i++) sum[i]=sum[i-1]+w[i]; 
	int l=0,r=0;
	for(int i=1;i<=n;i++){
		while(l<r&&sum[q[l+1]]+g[q[l+1]]<=sum[i]) l++;
		//注意这里是 l<r ，因为是判断 q[l+1] 是否可行 
		dp[i]=dp[q[l]]+1;
		g[i]=sum[i]-sum[q[l]]; 
		//是减去 sum[q[l]] 而不是 sum[q[l]-1] 是因为 q[l] 是转移点，所以新的一层就是从
		// q[l]+1 开始，再因为是前缀和，减去一，就成了 q[l] 
		while(l<=r&&sum[q[r]]+g[q[r]]>=sum[i]+g[i]) r--;
		q[++r]=i;
	}
	printf("%d",dp[n]);
	return 0;
}
//参考 https://www.luogu.com.cn/blog/emptyset/solution-p4954 
```
---

## P5005 中国象棋 - 摆上马(动态规划/状态压缩DP)

**题目背景**

Imakf 玩腻了国际象棋，决定玩一玩中国象棋。

他发现中国象棋的马和国际象棋的马有所不同，他意识到这又可以出一道简单的问题，于是他又准备摆一摆马了

**题目描述**

Imakf 有一个 $X$ 行 $Y$ 列的棋盘，还有很多**完全相同**的马（你可以认为有无数个）。现在在棋盘上摆上马（或者不摆），求任何马无法攻击另一匹马的方案总数。

中国象棋的马和国际象棋的马不同。

![](https://cdn.luogu.com.cn/upload/pic/40761.png)

注意：实际问题中是没有兵的。

当然由于方案可能过多，请输出对 $(10^9+7)$ 取模的值

**输入格式**

第一行两个正整数 $X,Y$。

**输出格式**

方案对 $(10^9+7)$ 取模的值。

**样例 #1**

**样例输入 #1**

```
1 1
```

**样例输出 #1**

```
2
```

**样例 #2**

**样例输入 #2**

```
3 3
```

**样例输出 #2**

```
145
```

**提示**

对于 100% 的数据，有 $1\le X\leq100$，$1\le Y\leq6$。

对于 20% 的数据，有 $X,Y\leq6$。

对于另外 20% 的数据，有 $X\leq20$。

对于样例 1，可以选择不摆或者摆。

对于样例 2，我有一个绝妙的解释可惜我写不下。
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int mod=1e9+7;
const int N=1<<7;
int n,m,dp[3][N][N],ans=0;
bool chck1(int k1,int k2,int t){
	int s=~((k1&(k1>>1))>>1),k;
	k=s&k2;
	if((k1>>2)&k) return false;
	s=~((k1&(k1<<1))<<1);k=s&k2;
	if((k1<<2)&k) return false;
	if(t) return true;
	else return true&&chck1(k2,k1,t+1);
}
bool chck2(int k1,int k2,int k3,int t){
	int s=~((k1&k2)>>1),k;
	k=s&k3;
	if((k1>>1)&k) return false;
	s=~((k1&k2)<<1),k=s&k3;
	if((k1<<1)&k) return false;
	if(t) return true;
	else return true&&chck2(k3,k2,k1,t+1);
}
int main(){
	scanf("%d%d",&n,&m);
	int maxn=(1<<m)-1;
	for(int i=0;i<=maxn;i++) dp[1][i][0]=1;
	for(int i=0;i<=maxn;i++)
	  for(int j=0;j<=maxn;j++)
	    if(chck1(i,j,0)) dp[2][i][j]=1;
	for(int i=3;i<=n;i++){
	  memset(dp[i%3],0,sizeof dp[i%3]);
	  for(int j=0;j<=maxn;j++)
	    for(int k=0;k<=maxn;k++) if(chck1(j,k,0))
	      for(int s=0;s<=maxn;s++)
	        if(chck1(k,s,0)&&chck2(j,k,s,0))
	          dp[i%3][j][k]=(dp[i%3][j][k]+dp[(i-1)%3][k][s])%mod;
    }
    for(int i=0;i<=maxn;i++)
      for(int j=0;j<=maxn;j++) if(chck1(i,j,0))
        ans=(ans+dp[n%3][i][j])%mod;
    printf("%d",ans);
    return 0;
}
```
---
## P5057 [CQOI2006]简单题(树状数组/线段树)

**题目描述**

有一个 n 个元素的数组，每个元素初始均为 0。有 m 条指令，要么让其中一段连续序列数字反转——0 变 1，1
变 0（操作 1），要么询问某个元素的值（操作 2）。
例如当 n = 20 时，10 条指令如下：

![](https://cdn.luogu.com.cn/upload/pic/44663.png)

**输入格式**

第一行包含两个整数 n, m，表示数组的长度和指令的条数； 以下 m 行，每行的第一个数 t 表示操作的种类：

若 t = 1，则接下来有两个数 L, R，表示区间 [L, R] 的每个数均反转； 若 t = 2，则接下来只有一个数 i，表示询问的下标。

**输出格式**

每个操作 2 输出一行（非 0 即 1），表示每次操作 2 的回答。

**样例 #1**

**样例输入 #1**

```
20 10
1 1 10
2 6
2 12
1 5 12
2 6
2 15
1 6 16
1 11 17
2 12
2 6
```


**样例输出 #1**

```
1
0
0
0
1
1
```

**提示**

对于 50% 的数据，1 ≤ n ≤ $10^3$, 1 ≤ m ≤ $10^4$；
对于 100% 的数据，1 ≤ n ≤ $10^5$, 1 ≤ m ≤ 5 × $10^5$，保证 L ≤ R。
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
typedef long long ll;
struct sgtree{
	ll l,r,d,num;
}tree[400005];
ll n,m,a[100005];
void build(ll x,ll l,ll r){
	tree[x].l=l,tree[x].r=r;
	if(l==r){tree[x].num=0; return;}
	ll mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	tree[x].num=tree[x<<1].num+tree[x<<1|1].num;
}
void pushdown(ll x){
	tree[x<<1].d+=tree[x].d;
	tree[x<<1|1].d+=tree[x].d;
	tree[x<<1].num+=tree[x].d*(tree[x<<1].r-tree[x<<1].l+1);
	tree[x<<1|1].num+=tree[x].d*(tree[x<<1|1].r-tree[x<<1|1].l+1);
	tree[x].d=0;
}
ll query(ll x,ll l){
	if(tree[x].l==tree[x].r) return (tree[x].num&1);
	if(tree[x].d) pushdown(x);
	ll mid=(tree[x].l+tree[x].r)>>1;
	if(l<=mid) return query(x<<1,l);
	else return query(x<<1|1,l);
}
void modify(ll x,ll l,ll r){
	if(l<=tree[x].l&&r>=tree[x].r){
		tree[x].num+=(tree[x].r-tree[x].l+1);
		tree[x].d++;
		return;
	}
	if(tree[x].d) pushdown(x);
	ll mid=(tree[x].l+tree[x].r)>>1;
	if(l<=mid) modify(x<<1,l,r);
	if(r>mid) modify(x<<1|1,l,r);
	tree[x].num=tree[x<<1].num+tree[x<<1|1].num;
}
int main(){
	scanf("%lld%lld",&n,&m);
	build(1,1,n);
	while(m--){
		int op,l,r; scanf("%d%d",&op,&l);
		if(op==1){
			scanf("%d",&r);
			modify(1,l,r);
		}else printf("%lld\n",query(1,l));
	}
	return 0;
}
```
---
## P6186 [NOI Online #1 提高组] 冒泡排序(逆序对/树状数组)

**题目描述**

给定一个 $1 ∼ n$ 的排列 $p_i$，接下来有 $m$ 次操作，操作共两种：
1. 交换操作：给定 $x$，将当前排列中的第 $x$ 个数与第 $x+1$ 个数交换位置。
2. 询问操作：给定 $k$，请你求出当前排列经过 $k$ 轮冒泡排序后的逆序对个数。
对一个长度为 $n$ 的排列 $p_i$ 进行一轮冒泡排序的伪代码如下：
```
for i = 1 to n-1:
  if p[i] > p[i + 1]:
    swap(p[i], p[i + 1])
```

**输入格式**

第一行两个整数 $n$，$m$，表示排列长度与操作个数。

第二行 $n$ 个整数表示排列 $p_i$。

接下来 $m$ 行每行两个整数 $t_i$，$c_i$，描述一次操作：
- 若 $t_i=1$，则本次操作是交换操作，$x=c_i$；
- 若 $t_i=2$，则本次操作是询问操作，$k=c_i$。

**输出格式**

对于每次询问操作输出一行一个整数表示答案。

**样例 #1**

**样例输入 #1**

```
3 6
1 2 3
2 0
1 1
1 2
2 0
2 1
2 2
```

**样例输出 #1**

```
0
2
1
0
```

**提示**

**样例一解释**
第一次操作：排列为 $\{1,2,3\}$，经过 0 轮冒泡排序后为 $\{1,2,3\}$，$0$ 个逆序对。

第二次操作：排列变为 $\{2,1,3\}$。

第三次操作：排列变为 $\{2,3,1\}$。

第四次操作：经过 $0$ 轮冒泡排序后排列变为 $\{2,3,1\}$，$2$ 个逆序对。

第五次操作：经过 $1$ 轮冒泡排序后排列变为 $\{2,1,3\}$，$1$ 个逆序对。

第六次操作：经过 $2$ 轮冒泡排序后排列变为 $\{1,2,3\}$，$0$ 个逆序对。


**数据范围与提示**
对于测试点 1 ∼ 2：$n,m \leq 100$。

对于测试点 3 ∼ 4：$n,m \leq 2000$。

对于测试点 5 ∼ 6：交换操作个数不超过 $100$。

对于所有测试点：$2 \leq n,m \leq 2 \times 10^5$，$t_i \in \{1,2\}$，$1 \leq x < n$，$0 \leq k < 2^{31}$。
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#define N 200005
using namespace std;
typedef long long ll;
ll c[N],tot=0,sum=0,a[N],bef[N],t[N],n,m;
ll lowbit(ll x){return x&(-x);}
void add(ll x,ll k){
	for(;x<=n;x+=lowbit(x)) c[x]+=k;
}
ll ask(ll x){
	ll ans=0;
	for(;x;x-=lowbit(x)) ans+=c[x];
	return ans;
}
int main(){
	scanf("%lld%lld",&n,&m);
	for(int i=1;i<=n;i++) scanf("%lld",&a[i]);
	for(int i=1;i<=n;i++){
		bef[i]=i-ask(a[i])-1;
		sum+=bef[i];
		t[bef[i]]++;
		add(a[i],1);
	}
	memset(c,0,sizeof c);
	add(1,sum);
	for(int i=1;i<=n;i++){
		tot+=t[i-1];
		add(i+1,-(n-tot));
	}
	for(int i=1;i<=m;i++){
		int op; ll x; scanf("%d%lld",&op,&x);
		x=min(x,(ll)(n-1));
		if(op==1){
			if(a[x]<a[x+1]){
				swap(a[x],a[x+1]);
				swap(bef[x],bef[x+1]);
				add(1,1);
				bef[x+1]++;
				add(bef[x+1]+1,-1);
			}else{
				swap(a[x],a[x+1]);
				swap(bef[x],bef[x+1]);
				add(1,-1);
				add(bef[x]+1,1);
				bef[x]--;
			}
		}else printf("%lld\n",(ll)ask(x+1));
	}
	return 0;
}
```
---
## P6218 [USACO06NOV] Round Numbers S(动态规划/进制/数位DP)

**题目描述**

如果一个正整数的二进制表示中，$0$ 的数目不小于 $1$ 的数目，那么它就被称为「圆数」。

例如，$9$ 的二进制表示为 $1001$，其中有 $2$ 个 $0$ 与 $2$ 个 $1$。因此，$9$ 是一个「圆数」。

请你计算，区间 $[l,r]$ 中有多少个「圆数」。

**输入格式**

一行，两个整数 $l,r$。

**输出格式**

一行，一个整数，表示区间 $[l,r]$ 中「圆数」的个数。

**样例 #1**

**样例输入 #1**

```
2 12
```

**样例输出 #1**

```
6
```

**提示**

【数据范围】

对于 $100\%$ 的数据，$1\le l,r\le 2\times 10^9$。

【样例说明】

区间 $[2,12]$ 中共有 $6$ 个「圆数」，分别为 $2,4,8,9,10,12$。
```c++
#include<cstdio>
#include<algorithm>
#include<cstring>
using namespace std;
int a[32],len=0,l,r,dp[32][32][32];
int dfs(int pos,int one,int zero,int limit,int lead){
	if(!pos) return ((zero>=one)||lead);
	if(!lead&&!limit&&dp[pos][one][zero]!=-1) return dp[pos][one][zero];
	int ans=0,ret=limit?a[pos]:1;
	for(int i=0;i<=ret;i++)
	  if(lead&&(!i)) ans+=dfs(pos-1,one,zero,limit&(i==ret),1);
	  else ans+=dfs(pos-1,one+(i==1),zero+(i==0),limit&(i==ret),0);
	if(!lead&&!limit) dp[pos][one][zero]=ans;
	return ans;
}
int solve(int x){
	memset(dp,-1,sizeof dp);
	len=0;
	while(x){
		a[++len]=x&1;
		x>>=1;
	}
	return dfs(len,0,0,1,1);
}
int main(){
	scanf("%d%d",&l,&r);
	printf("%d",solve(r)-solve(l-1));
	return 0;
}
```
---
## P6492 [COCI2010-2011#6] STEP(线段树)

**题目描述**

给定一个长度为 $n$ 的字符序列 $a$，初始时序列中全部都是字符 `L`。

有 $q$ 次修改，每次给定一个 $x$，若 $a_x$ 为 `L`，则将 $a_x$ 修改成 `R`，否则将 $a_x$ 修改成 `L`。

对于一个只含字符 `L`，`R` 的字符串 $s$，若其中不存在连续的 `L` 和 `R`，则称 $s$ 满足要求。

每次修改后，请输出当前序列 $a$ 中最长的满足要求的连续子串的长度。

**输入格式**

第一行有两个整数，分别表示序列的长度 $n$ 和修改操作的次数 $q$。

接下来 $q$ 行，每行一个整数，表示本次修改的位置 $x$。

**输出格式**

对于每次修改操作，输出一行一个整数表示修改 $a$ 中最长的满足要求的子串的长度。

**样例 #1**

**样例输入 #1**

```
6 2
2
4
```

**样例输出 #1**

```
3
5
```

**样例 #2**

**样例输入 #2**

```
6 5
4
1
1
2
6
```

**样例输出 #2**

```
3
3
3
5
6
```

**提示**

**数据规模与约定**

对于全部的测试点，保证 $1 \leq n, q \leq 2 \times 10^5$，$1 \leq x \leq n$。

**说明**

**题目译自 [COCI2010-2011](https://hsin.hr/coci/archive/2010_2011/) [CONTEST #6](https://hsin.hr/coci/archive/2010_2011/contest6_tasks.pdf) *T5 STEP***。
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
struct sgtree{
	int l,r,suml,sum,sumr;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define suml(x) tree[x].suml
	#define sumr(x) tree[x].sumr
	#define sum(x) tree[x].sum
}tree[800005];
int a[200005],n,m;
void pushup(int x){
	int lenl=(r(x<<1)-l(x<<1)+1);
	int lenr=(r(x<<1|1)-l(x<<1|1)+1);
	suml(x)=suml(x<<1),sumr(x)=sumr(x<<1|1),sum(x)=max(sum(x<<1),sum(x<<1|1));
	if(a[r(x<<1)]!=a[l(x<<1|1)]){
		if(sum(x<<1)==lenl) suml(x)=sum(x<<1)+suml(x<<1|1);
		if(sum(x<<1|1)==lenr) sumr(x)=sum(x<<1|1)+sumr(x<<1);
		sum(x)=max(sum(x),sumr(x<<1)+suml(x<<1|1));
	} 
} 
void build(int x,int l,int r){
	l(x)=l,r(x)=r,suml(x)=sumr(x)=sum(x)=1;
	if(l==r) return;
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	pushup(x);
}
void modify(int x,int k){
	if(l(x)==r(x)&&l(x)==k){a[l(x)]^=1; return;}
	int mid=(l(x)+r(x))>>1;
	if(k<=mid) modify(x<<1,k);
	else modify(x<<1|1,k);
	pushup(x);
}
int main(){
	scanf("%d%d",&n,&m);
	build(1,1,n);
	while(m--){
		int x; scanf("%d",&x);
		modify(1,x);
		printf("%d\n",sum(1));
	}
	return 0;
}
```
