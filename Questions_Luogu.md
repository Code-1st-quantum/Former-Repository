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

## P1314 [NOIP2011 提高组] 聪明的质监员(前缀和，二分答案)

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
## P1902 刺杀大使

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

## P3718 [AHOI2017初中组]alter(二分答案)

**题目背景**

以下为不影响题意的简化版题目。

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
