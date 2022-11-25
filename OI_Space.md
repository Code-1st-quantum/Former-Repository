**$\mathbf{\color{Purple}{Welcome\ to\ The\ OI\ -\ Space\ of\ Code}} \mathbf{\color{Purple}{\underline{\ \ }}} \mathbf{\color{Purple}{quantum\ on\ Github}}$**

# C++ Part

## 模板 Templates

### 数据结构 Data Structures

#### 字典树 Trie
##### Trie
 [洛谷 P2580 于是他错误的点名开始了](https://www.luogu.com.cn/problem/P2580)
```c++
#include<iostream>
#include<string>
#include<cstring>
#include<cstdio>
using namespace std;
int n,trie[10000005][30],node=0,f[10000005],m;
void build_tree(){
	for(int i=1;i<=n;i++){
		char s[55];
		int root=0;
		cin>>s;
		int len=strlen(s);
		for(int j=0;j<len;j++){
			int c=s[j]-'a';
			if(!trie[root][c]) trie[root][c]=++node;
			root=trie[root][c];
		}
		f[root]++;
	}
}
void search(){
	for(int i=1;i<=m;i++){
		char s[55];
		cin>>s;
		int root=0,len=strlen(s);
		for(int j=0;j<len;j++){
			int c=s[j]-'a';
			if(!trie[root][c]){
				printf("WRONG\n");
				break;
			}
			root=trie[root][c];
			if(j==len-1&&f[root]==1){
				f[root]++;
				printf("OK\n");
				break;
			}if(j==len-1&&f[root]>1){
				printf("REPEAT\n");
				f[root]++;
				break;
			}
		}
	} 
}
int main(){
	scanf("%d",&n);
	build_tree(); 
	scanf("%d",&m);
	search();
	return 0;
}

```
##### 01 Trie
 [洛谷 P6018 [Ynoi2010] Fusion tree](https://www.luogu.com.cn/problem/P6018)
```c++
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<iostream>
#define p 500005*21   //最多21层 
using namespace std;
namespace trie{
	int tr[p][2];   //01 Trie
	int vxor[p],w[p];   //vxor[i]表示以 i 为根的子树的异或和
/*	

w[i]表示 Trie 上节点 i 和其父节点间的边被几个维护的数字经过了 
例如，维护了这几个数字:
     1001110
     0111000
     1000111
     0111011
则第二位的异或结果为 0^1^0^1 =0 
而 w 数组则直接将经过次数提前统计好了 

*/	               		 
	int tot=0,root[p]; 
	inline int setnode(){    //建立新的节点 
	    tot++;
		tr[tot][0]=tr[tot][1]=0;
		w[tot]=vxor[tot]=0;
		return tot;
	}
	void maintain(int y){ //维护子树异或和 
		w[y]=vxor[y]=0;
		if(tr[y][0]){ 
			w[y]+=w[tr[y][0]];   //累加经过次数 
			vxor[y]^=vxor[tr[y][0]]<<1;  //要左移一位是因为 Trie 是从低位为根储存的，所以要新加一位 
		}
		if(tr[y][1]){
			w[y]+=w[tr[y][1]];
			vxor[y]^=(vxor[tr[y][1]]<<1)|(w[tr[y][1]]&1); // &1 是判断奇偶性 
		}
	}
	void erase(int y,int o,int dp){   //删除某些权值操作
		if(dp>20) return (void)w[y]--;
		erase(tr[y][o&1],o>>1,dp+1); // o&1 是判断该位该往哪边走，o>>1 则是进位 
		maintain(y);
	}
	void insert(int &y,int o,int dp){  //插入新权值
		if(!y) y=setnode();
		if(dp>20) return (void)w[y]++;
		insert(tr[y][o&1],o>>1,dp+1);  //进位建立更深的 Trie 节点 
		maintain(y); //不要忘记重新维护子树异或和 
	}
	void xorsum(int x){   //全局加一操作
		swap(tr[x][1],tr[x][0]);
		if(tr[x][0]) xorsum(tr[x][0]);
		maintain(x); //重新求一下子树和异或 
	}
}
struct edges{
	int next,to;
}edge[p];
int n,m,rt,cnt=0,v[p],head[p],fa[p],change[p];
void add_edge(int u,int v2){
	edge[++cnt].to=v2;
	edge[cnt].next=head[u];
	head[u]=cnt;
}
void solve(){
	for(int i=0;i<=n;i++) head[i]=-1;
}
void findfa(int x,int f){
	fa[x]=f;
	for(int i=head[x];i!=-1;i=edge[i].next){
		int v=edge[i].to;
		if(v==f) continue;
		findfa(v,x);
	}
} 
int newans(int x){
	return (fa[x]==-1?0:change[fa[x]])+v[x];
} 
int main(){
	scanf("%d%d",&n,&m);
	solve();
	for(int i=1;i<n;i++){
		int u,v2;
		scanf("%d%d",&u,&v2);
		add_edge(u,v2);
		add_edge(rt=v2,u);
	}
	findfa(rt,-1);
	for(int i=1;i<=n;i++){
		scanf("%d",&v[i]);
		if(fa[i]!=-1) trie::insert(trie::root[fa[i]],v[i],0);
	}
	for(int i=1;i<=m;i++){
		int op,x;
		scanf("%d%d",&op,&x);
		if(op==1){
			change[x]++;  //在处理它的子节点时更方便，因为这里使其子节点权值改变了 
			if(x!=rt){  //特殊的，该操作需要更改其父节点，因此要特判 
				if(fa[fa[x]]!=-1) trie::erase(trie::root[fa[fa[x]]],newans(fa[x]),0);
				v[fa[x]]++;
				if(fa[fa[x]]!=-1) trie::insert(trie::root[fa[fa[x]]],newans(fa[x]),0);
			}
			trie::xorsum(trie::root[x]);
		}else if(op==2){
			int now;
			scanf("%d",&now);
			if(now!=rt) trie::erase(trie::root[fa[x]],newans(x),0);
			v[x]-=now;
			if(now!=rt) trie::insert(trie::root[fa[x]],newans(x),0);
		}else{
			int ans=0;
			ans=trie::vxor[trie::root[x]];
			ans^=newans(fa[x]);
			printf("%d\n",ans);
		}
	}
	return 0;
}
```

#### 二叉堆 Binary Heap 
##### 小根二叉堆 Binary Heap 
 [洛谷 P3378 【模板】堆](https://www.luogu.com.cn/problem/P3378)
```c++
#include<iostream>
#include<cstdio>
using namespace std;
int n,tot=0,heap[1000005];
void insert(int x){
	heap[++tot]=x;
	int p=tot;
	while(p>1){
		if(heap[p]<heap[p/2]){
			swap(heap[p],heap[p/2]);
			p=p/2;
		}else break;
	}
}
void erase(){
	swap(heap[1],heap[tot--]);
	int p=1;
	while(true){
		int lson=p*2,rson=p*2+1;
		int minn=heap[p],k;
		if(lson<=tot&&heap[lson]<minn) minn=heap[lson],k=lson;
		if(rson<=tot&&heap[rson]<minn) minn=heap[rson],k=rson;
		if(minn==heap[p]) break;
		swap(heap[p],heap[k]);
		p=k;
	}
}
int main(){
	scanf("%d",&n);
	while(n--){
		int op;
		scanf("%d",&op);
		if(op==1){
			int x;
			scanf("%d",&x);
			insert(x);
		}
		else if(op==2) printf("%d\n",heap[1]);
		else erase();
	}
	return 0;
}
```

#### RMQ 问题 Range Minimum/Maximum Queries 
##### ST 表 Sparse Table
 [洛谷 P3865 【模板】ST 表](https://www.luogu.com.cn/problem/P3865)
```c++
#include<iostream>
#include<cstdio>
#include<cmath> 
using namespace std;
int n,m,f[100005][22];
int query(int l,int r){
	int k=log2(r-l+1);
	return max(f[l][k],f[r-(1<<k)+1][k]);
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d",&f[i][0]);
	for(int k=1;k<=21;k++)
	  for(int i=1;i+(1<<k)-1<=n;i++)
	    f[i][k]=max(f[i][k-1],f[i+(1<<(k-1))][k-1]);
	for(int i=1;i<=m;i++){
		int l,r;
		scanf("%d%d",&l,&r);
		printf("%d\n",query(l,r));
	}
	return 0;
}
```

#### 线段树 Segment Tree
##### 线段树之单点修改区间查询 Segment Tree (Modify on Points and Inquiry on Sections) 
```c++
#include<iostream>
#include<cstdio>
using namespace std;
int n,m,a[500005],v[2100005];
void build(int x,int l,int r){
	if(l==r){v[x]=a[l];return;}
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	v[x]=v[x<<1]+v[x<<1|1];
}
void modify(int x,int l,int r,int to,int k){
	if(l==r){v[x]+=k;return;}
	int mid=(l+r)>>1;
	if(to<=mid) modify(x<<1,l,mid,to,k);
	else modify(x<<1|1,mid+1,r,to,k);
	v[x]=v[x<<1]+v[x<<1|1];
}
int query(int x,int l,int r,int ql,int qr){
	if(l==ql&&r==qr) return v[x];
	int mid=(l+r)>>1;
	if(ql<=mid&&qr>mid) return query(x<<1,l,mid,ql,mid)+query(x<<1|1,mid+1,r,mid+1,qr);
	else if(qr<=mid) return query(x<<1,l,mid,ql,qr);
	else return query(x<<1|1,mid+1,r,ql,qr);
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d",&a[i]);
	build(1,1,n);
	while(m--){
		int l,r,op;
		scanf("%d%d%d",&op,&l,&r);
		if(op==1) modify(1,1,n,l,r);
		else printf("%d\n",query(1,1,n,l,r));
	}
	return 0;
}
```
##### 线段树之区间修改单点查询(省空间版) Segment Tree (Modify on Sections and Inquiry on Points(Save Memories))
 [洛谷 P4939 Agent2](https://www.luogu.com.cn/problem/P4939)
```c++
#include<cstdio>
#include<algorithm>
#define N 20000002
using namespace std;
int n,m,a[N],s1[N],s2[N],sum=1;
void modify(int x,int l,int r,int ql,int qr){
	if(ql<=l&&qr>=r){a[x]++; return;}
	int mid=(l+r)>>1;
	if(ql<=mid){
		if(!s1[x]) s1[x]=++sum;
		modify(s1[x],l,mid,ql,qr);
	}
	if(qr>mid){
		if(!s2[x]) s2[x]=++sum;
		modify(s2[x],mid+1,r,ql,qr);
	}
}
int query(int x,int l,int r,int k){
	if(l==r) return a[x];
	int mid=(l+r)>>1;
	if(k<=mid) return a[x]+query(s1[x],l,mid,k);
	else return a[x]+query(s2[x],mid+1,r,k);
}
int main(){
	scanf("%d%d",&n,&m);
	while(m--){
		int op; scanf("%d",&op);
		if(op==0){
			int l,r; scanf("%d%d",&l,&r);
			modify(1,1,n,l,r);
		}else{
			int x; scanf("%d",&x);
			printf("%d\n",query(1,1,n,x)); 
		}
	}
	return 0;
}
```
##### 线段树之区间修改区间查询 Segment Tree (Both Modify and Inquiry on Sections) 
```c++
#include<iostream>
#include<cstdio>
typedef long long ll;
using namespace std;
int n,m;
ll a[100005],d[400005],v[400005];
void build(int x,int l,int r){
	if(l==r){v[x]=a[l];return;}
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	v[x]=v[x<<1]+v[x<<1|1];
}
void pushdown(int x,int l,int r,int mid){
	if(!d[x]) return;
	d[x<<1]+=d[x];
	d[x<<1|1]+=d[x];
	v[x<<1]+=d[x]*(mid-l+1);
	v[x<<1|1]+=d[x]*(r-mid);
	d[x]=0;
}
void modify(int x,int l,int r,int ql,int qr,ll k){
	if(l==ql&&r==qr){d[x]+=k; v[x]+=k*(qr-ql+1);return;}
	int mid=(l+r)>>1;
	pushdown(x,l,r,mid);
	if(ql<=mid&&qr>mid){
		modify(x<<1,l,mid,ql,mid,k);
		modify(x<<1|1,mid+1,r,mid+1,qr,k);
	}else if(qr<=mid) modify(x<<1,l,mid,ql,qr,k);
	else modify(x<<1|1,mid+1,r,ql,qr,k);
	v[x]=v[x<<1]+v[x<<1|1];
}
ll query(int x,int l,int r,int ql,int qr){
	if(l==ql&&r==qr){return v[x];}
	int mid=(l+r)>>1;
	pushdown(x,l,r,mid);
	if(ql<=mid&&qr>mid)
	  return query(x<<1,l,mid,ql,mid)+query(x<<1|1,mid+1,r,mid+1,qr);
	else if(qr<=mid) return query(x<<1,l,mid,ql,qr);
	else return query(x<<1|1,mid+1,r,ql,qr);
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%lld",&a[i]);
	build(1,1,n);
	while(m--){
		int op;
		scanf("%d",&op);
		if(op==1){
			ll x,y,k;
			scanf("%lld%lld%lld",&x,&y,&k);
			modify(1,1,n,x,y,k);
		}else{
			ll x,y;
			scanf("%lld%lld",&x,&y);
			printf("%lld\n",query(1,1,n,x,y));
		}
	}
	return 0;
}
```

#### 平衡树
##### Treap
 [洛谷 P3369 【模板】普通平衡树](https://www.luogu.com.cn/problem/P3369)
```cpp
#include<cstdio>
#include<algorithm>
#include<iostream>
#include<cstring>
#include<cstdlib>
#define N 400005 
#define int long long 
using namespace std;
struct treap{int l,r,size,dat,cnt,val;}a[N];
int n,m,op,x,tot=0,rt;
void update(int x){a[x].size=a[a[x].l].size+a[a[x].r].size+a[x].cnt;}
int make_node(int val){a[++tot].size=1;a[tot].cnt=1;a[tot].val=val;a[tot].dat=rand();return tot;}
void build(){make_node(-1e9);make_node(1e9);rt=1;a[1].r=2;update(rt);}
void zig(int &p){int q=a[p].l; a[p].l=a[q].r; a[q].r=p; p=q; update(a[p].r); update(p);}
void zag(int &p){int q=a[p].r; a[p].r=a[q].l; a[q].l=p; p=q; update(a[p].l); update(p);}
void insert(int &p,int val){
	if(p==0){p=make_node(val);return;}
	if(val==a[p].val){a[p].cnt++; update(p); return;}
	if(val<a[p].val){insert(a[p].l,val); if(a[p].dat<a[a[p].l].dat) zig(p);}
	else{insert(a[p].r,val); if(a[p].dat<a[a[p].r].dat) zag(p);} update(p);
}
int getrank(int p,int val){
	if(!p) return 1; 
	if(val==a[p].val) return a[a[p].l].size+1;
	if(val<a[p].val) return getrank(a[p].l,val);
	return a[a[p].l].size+a[p].cnt+getrank(a[p].r,val);
}
int getval(int p,int rank){
	if(!p) return 1e18; 
	if(rank<=a[a[p].l].size) return getval(a[p].l,rank);
	if(rank<=a[a[p].l].size+a[p].cnt) return a[p].val;
	return getval(a[p].r,rank-a[a[p].l].size-a[p].cnt);
}
void remove(int &p,int val){
	if(!p) return;
	if(val==a[p].val){
		if(a[p].cnt>1){a[p].cnt--;update(p);return;}
		if(a[p].l||a[p].r){
			if((!a[p].r)||a[a[p].l].dat>a[a[p].r].dat) zig(p),remove(a[p].r,val);
			else zag(p),remove(a[p].l,val); 
			update(p);
		}else p=0; return;
	}
	if(val<a[p].val) remove(a[p].l,val); else remove(a[p].r,val);
	update(p);
}
int getnext(int val){
	int ans=2,p=rt;
	while(p){
		if(a[p].val==val){if(a[p].r>0){p=a[p].r; while(a[p].l>0)p=a[p].l; ans=p;} break;}
		if(a[p].val>val&&a[p].val<a[ans].val) ans=p;
		if(val<a[p].val) p=a[p].l; else p=a[p].r;
	}return a[ans].val;
}
int getpre(int val){
	int ans=1,p=rt;
	while(p){
		if(a[p].val==val){if(a[p].l>0){p=a[p].l; while(a[p].r>0)p=a[p].r; ans=p;} break;}
		if(a[p].val<val&&a[p].val>a[ans].val) ans=p;
		if(val<a[p].val) p=a[p].l; else p=a[p].r;
	}return a[ans].val;
}
signed main(){
	scanf("%lld",&n); build();
	for(int i=1;i<=n;i++){
		scanf("%lld%lld",&op,&x);
		switch (op){
			case 1:{insert(rt,x);break;}
			case 2:{remove(rt,x);break;}
			case 3:{printf("%lld\n",getrank(rt,x)-1);break;}
			case 4:{printf("%lld\n",getval(rt,x+1));break;}
			case 5:{printf("%lld\n",getpre(x));break;}
			case 6:{printf("%lld\n",getnext(x));break;}
		}
	}
	return 0;
}
```
##### FHQ-Treap
[洛谷 P3369 【模板】普通平衡树](https://www.luogu.com.cn/problem/P3369)
```cpp
#include<cstdio>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<cstdlib>
#define N 200005
using namespace std;
struct fhq_treap{int l,r,val,size,dat;}a[N];
int n,op,x,tot=0,rt=0;
void make_node(int val){a[++tot].val=val;a[tot].size=1;a[tot].dat=rand();a[tot].l=a[tot].r=0;}
void update(int p){a[p].size=a[a[p].l].size+a[a[p].r].size+1;}
void split(int p,int val,int &l,int &r){
	if(!p){l=r=0;return;}
	if(a[p].val<=val){l=p;split(a[p].r,val,a[p].r,r);}
	else{r=p;split(a[p].l,val,l,a[p].l);} update(p);
}
int merge(int l,int r){
	if((!l)||(!r)) return l+r;
	if(a[l].dat<a[r].dat){a[l].r=merge(a[l].r,r); update(l); return l;}
	else{a[r].l=merge(l,a[r].l); update(r); return r;}
}
int getval(int p,int rank){
	if(!p) return 1;
	if(a[a[p].l].size>=rank) return getval(a[p].l,rank);
	if(a[a[p].l].size+1>=rank) return a[p].val;
	return getval(a[p].r,rank-a[a[p].l].size-1);
}
int main(){
	scanf("%d",&n); int l,r,now;
	for(int i=1;i<=n;i++){
		scanf("%d%d",&op,&x);
		if(op==1){split(rt,x,l,r); make_node(x); rt=merge(merge(l,tot),r);}
		else if(op==2){split(rt,x,l,r); split(l,x-1,l,now); now=merge(a[now].l,a[now].r); rt=merge(merge(l,now),r);}
		else if(op==3){split(rt,x-1,l,r); printf("%d\n",a[l].size+1); rt=merge(l,r);}
		else if(op==4){printf("%d\n",getval(rt,x));}
		else if(op==5){split(rt,x-1,l,r); printf("%d\n",getval(l,a[l].size)); rt=merge(l,r);}
		else{split(rt,x,l,r); printf("%d\n",getval(r,1)); rt=merge(l,r);}
	}
	return 0;
}
```

### 数论 Number Theory

#### 素数筛法/素数判断 Prime Sieve/Prime Judgement
##### (素数)线性筛 Linear Sieve Method
 [洛谷 P3383 【模板】线性筛素数](https://www.luogu.com.cn/problem/P3383)
 ```cpp
 #include<cstdio>
using namespace std;
bool vis[100000005];
int n,q,k,prime[100000005],cnt=0;
void linear_prime(){
	for(int i=2;i<=n;i++){
		if(!vis[i]) prime[++cnt]=i;
		for(int j=1;j<=cnt&&i*prime[j]<=n;j++){
			vis[i*prime[j]]=true;
			if(i%prime[j]==0) break;
		}
	}
}
int main(){
	scanf("%d%d",&n,&q);
	linear_prime();
	while(q--){
		scanf("%d",&k);
		printf("%d\n",prime[k]);
	}
	return 0;
}
```
#### 同余 Congruence
##### 扩展欧拉定理(a ^ b % c 问题，b 为极大数) Extended Euler Theorem
 [洛谷 P5091 【模板】扩展欧拉定理](https://www.luogu.com.cn/problem/P5091)
```cpp
#include<cstdio>
#include<string>
#include<cmath>
typedef long long ll;
using namespace std;
ll n,m,ans,m2;
ll read(ll mod){
	ll f=0,x=0; char c=getchar();
	while(!(c>='0'&&c<='9')) c=getchar();
	while(c>='0'&&c<='9'){
		x=x*10+c-'0';
		if(x>=mod) f=1;
		x%=mod; 
		c=getchar();
	}
	return x+(f?mod:0);
}
ll quickpow(ll a,ll b,ll c){
	int res=1;
	while(b){
		if(b&1) res=res*a%c;
		a=a*a%c;
		b>>=1;
	}
	return res;
}
int main(){
	scanf("%lld%lld",&n,&m);
	ans=m,m2=m;
	for(int i=2;i<=sqrt(m);i++)
	  if(m%i==0){
		ans-=ans/i;
		while(m%i==0) m/=i;
	  }
	if(m>1) ans-=ans/m;
	ll b=read(ans); 
	printf("%lld",quickpow(n,b,m2));
	return 0;
}
```
##### 中国剩余定理 Chinese Remainder Rheorem CRT
 [洛谷 P1495 【模板】中国剩余定理(CRT)/曹冲养猪](https://www.luogu.com.cn/problem/P1495)
```cpp
#include<cstdio>
#include<cmath>
typedef long long ll;
using namespace std;
int n;
ll a[100005],m[100005],Ms=1,M[100005],t[100005],ans;
ll exgcd(ll a,ll b,ll &x,ll &y){
	if(b==0){x=1,y=0; return a;}
	ll d=exgcd(b,a%b,x,y);
	ll z=x; x=y; y=z-(a/b)*y;
	return d;
}
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++) scanf("%lld%lld",&m[i],&a[i]),Ms*=m[i];
	for(int i=1;i<=n;i++) M[i]=Ms/m[i];
	for(int i=1;i<=n;i++){
		ll x,y;
		ll d=exgcd(M[i],m[i],x,y);
		t[i]=((x%m[i]+m[i])%m[i]);
		ans=(ans+t[i]*M[i]*a[i])%Ms;
	}
	printf("%lld",ans%Ms);
	return 0;
}
```
##### 扩展中国剩余定理 Extended Chinese Remainder Theorem
 [洛谷 P4777 【模板】扩展中国剩余定理（EXCRT）](https://www.luogu.com.cn/problem/P4777)
 ```cpp
 #include<cstdio>
#include<iostream> 
#include<cmath>
typedef __int128 ll;
using namespace std;
int n;
long long r[100005],m[100005];
ll exgcd(ll a,ll b,ll &x,ll &y){
	if(b==0){x=1,y=0; return a;}
	ll d=exgcd(b,a%b,x,y);
	ll z=x; x=y; y=z-(a/b)*y;
	return d;
}
ll exCRT(){
	ll m1=m[1],r1=r[1];
	for(int i=2;i<=n;i++){
		ll m2=m[i],r2=r[i];
		ll delta=r2-r1;
	    ll x,y,gcd;
	    gcd=exgcd(m1,m2,x,y);
	    ll mod=m2/gcd;
	    x=((x*delta/gcd)%mod+mod)%mod;
	    r1=r1+x*m1;
	    m1=(m1*m2)/gcd;
	}
	return r1;
}
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++) cin>>m[i]>>r[i];
	cout<<(long long)(exCRT());
	return 0;
}
 ```
 ##### 扩展欧几里得算法及其通解 Extended Euclidean Algorithm
 [洛谷 P5656 【模板】二元一次不定方程 (exgcd)）](https://www.luogu.com.cn/problem/P5656)
 ```cpp
 #include<cstdio>
#include<iostream> 
#include<cmath>
typedef long long ll;
using namespace std;
int t;
ll exgcd(ll a,ll b,ll &x,ll &y){
	if(b==0){x=1,y=0; return a;}
	ll n=exgcd(b,a%b,x,y);
	ll z=x; x=y; y=z-(a/b)*y;
	return n; 
}
int main(){
	scanf("%d",&t);
	while(t--){
		ll a,b,c,d,x,y;
		scanf("%lld%lld%lld",&a,&b,&c);
		d=exgcd(a,b,x,y);
		if(c%d!=0){ printf("-1\n"); continue;}
		ll x1=(x*c)/d; ll y1=(y*c)/d;
		ll dx=b/d; ll dy=a/d;
		ll s1=ceil((1.0-x1)/dx);
		x1+=s1*dx,y1-=s1*dy;
		if(y1<=0){
			ll y2=y1+dy*1ll*ceil((1.0-y1)/dy);
			printf("%lld %lld\n",x1,y2);
			continue;
		} 
		printf("%lld ",(y1-1)/dy+1);
		printf("%lld ",x1);
		printf("%lld ",(y1-1)%dy+1);
		printf("%lld ",(y1-1)/dy*dx+x1);
		printf("%lld\n",y1);
	}
	return 0;
}
 ```
 ##### 线性同余方程 Linear Congruent Equation
[洛谷 P1082 [NOIP2012 提高组] 同余方程](https://www.luogu.com.cn/problem/P1082)
```cpp
#include<cstdio>
#include<cmath>
typedef long long ll;
using namespace std;
ll exgcd(ll a,ll b,ll &x,ll &y){
	if(b==0){x=1,y=0; return a;}
	ll m=exgcd(b,a%b,x,y);
	ll z=x; x=y; y=z-(a/b)*y;
	return m;
}
int main(){
	ll a,b,x,y,d;
	scanf("%lld%lld",&a,&b);
	d=exgcd(a,b,x,y);
	printf("%lld",((x%(b/d)+(b/d))%(b/d)));
	return 0;
}
```
##### 线性求乘法逆元 Linear Multiplicative Inverse
[洛谷 P3811 【模板】乘法逆元](https://www.luogu.com.cn/problem/P3811)
```cpp
#include<cstdio>
typedef long long ll;
using namespace std;
ll n,p,inv[3000005];
int main(){
	scanf("%lld%lld",&n,&p);
	inv[1]=1;
	printf("1\n");
	for(int i=2;i<=n;i++){
	    inv[i]=(p-(p/i))*(inv[p%i])%p;
	    printf("%lld\n",inv[i]);
    }
    return 0;
}
```

### 约数 Divisor
##### 线性筛求欧拉函数 Getting Euler Function By Sieve Method
[洛谷 P4139 上帝与集合的正确用法](https://www.luogu.com.cn/problem/P4139)
```cpp
#include<cstdio>
#include<iostream>
typedef long long ll;
using namespace std;
const ll MAXN=1e7+5;
ll phi[MAXN],prime[MAXN],cnt=0;
bool vis[MAXN]={false};
ll t,p;
void prephi(){  //线性筛求欧拉函数
	phi[1]=1;
	for(int i=2;i<=MAXN;i++){
		if(!vis[i]){
			phi[i]=i-1;
			prime[++cnt]=i;
		}
		for(int j=1;j<=cnt&&i*prime[j]<=MAXN;j++){
			vis[i*prime[j]]=true;
			if(i%prime[j]) phi[i*prime[j]]=(prime[j]-1)*phi[i];
			else{
				phi[i*prime[j]]=phi[i]*prime[j];
				break;
			}
		}
	}
}
ll quickpow(ll a,ll k,ll mod){
	ll res=1;
	while(k){
		if(k&1) res=(res*a)%mod;
		a=(a*a)%mod;
		k>>=1;
	}
	return res;
}
ll exEuler(ll mod){
	if(mod==1) return 0;
	return quickpow(2,exEuler(phi[mod])+phi[mod],mod);
}
int main(){
	prephi();
	scanf("%lld",&t);
	while(t--){
		scanf("%lld",&p);
		printf("%lld\n",exEuler(p));
	}
	return 0;
}
```

### 图论 Graph Theory

#### 欧拉路问题 Euler Path
##### 未拼接的欧拉回路具体方案 Get Path on Unspliced Euler Loop
注 : 不常用
```c++
#include<cstdio>
using namespace std;
struct e{
	int to,next;
}edge[20005];
bool vis[20005];
int n,m,cnt=1,head[10005],top=0,stack[10005];
void add_edge(int u,int v){
	edge[++cnt].to=v;
	edge[cnt].next=head[u];
	vis[cnt]=false;
	head[u]=cnt;
}
void dfs(int x){
	for(int i=head[x];i;i=edge[i].next)
		if(!vis[i]){
			int v=edge[i].to;
			vis[i]=vis[i^1]=true;
			dfs(v);
			stack[++top]=v;
		}
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++){
		int u,v; scanf("%d%d",&u,&v);
		add_edge(u,v);
		add_edge(v,u);
	}
	dfs(1);
	for(int i=top;i>0;i--) printf("%d ",stack[i]);
	return 0;
}
```
##### 欧拉回路求具体方案 Get Path on Euler Loop
```c++
#include<cstdio>
using namespace std;
struct e{
	int to,next;
}edge[200005];
bool vis[200005];
int n,m,cnt,ans[200005],tot=0,head[200005],top=0,stack[200005];
void add_edge(int u,int v){
	edge[++cnt].to=v;
	edge[cnt].next=head[u];
	vis[cnt]=false;
	head[u]=cnt;
}
void euler(){
	stack[++top]=1;
	while(top>0){
		int x=stack[top],i=head[x];
		while(i&&vis[i]) i=edge[i].next;
		if(i){
			stack[++top]=edge[i].to;
			vis[i]=vis[i^1]=true;
			head[x]=edge[i].next;
		}else{
			top--;
			ans[++tot]=x;
		}
	}
}
int main(){
	scanf("%d%d",&n,&m); cnt=1;
	for(int i=1;i<=m;i++){
		int u,v; scanf("%d%d",&u,&v);
		add_edge(u,v);
		add_edge(v,u);
	}
	euler();
	for(int i=tot;i;i--) printf("%d ",ans[i]);
	return 0;
}
```

#### LCA 最近公共祖先 Lowest Common Ancestors
##### 倍增求LCA  Getting LCA by Multiplication
```c++

```
##### 线段树 RMQ 方式求 LCA  Getting Lowest Common Ancestor by Using Segment Tree on RMQ
 [洛谷 P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)
 ```c++
 #include<iostream>
#include<cstdio>
using namespace std;
struct e{
	int next,to;
}edge[1000005];
int n,m,s,cnt=0,amax=0,a[1000005],b[500005],dp[500005],v[5000005],head[500005];
void add_edge(int u,int v){
	edge[++cnt].to=v;
	edge[cnt].next=head[u];
	head[u]=cnt;
}
inline void solve(){for(int i=1;i<=n;i++) head[i]=-1;}
void dfs(int u,int fa){
	a[++amax]=u;
	b[u]=amax;
	for(int i=head[u];i!=-1;i=edge[i].next){
		int v=edge[i].to;
		if(v==fa) continue;
		dp[v]=dp[u]+1;
		dfs(v,u);
		a[++amax]=u;
	}
}
int cmp(int x,int y){
	if(dp[x]<dp[y]) return x;
	else return y;
}
void build(int x,int l,int r){
	if(l==r){v[x]=a[l]; return;}
	int mid=(l+r)>>1;
	build(x<<1,l,mid);
	build(x<<1|1,mid+1,r);
	v[x]=cmp(v[x<<1],v[x<<1|1]);
}
int query(int x,int l,int r,int ql,int qr){
	if(l==ql&&r==qr) return v[x];
	int mid=(l+r)>>1;
	if(ql<=mid&&qr>mid){
	  return cmp(query(x<<1,l,mid,ql,mid),query(x<<1|1,mid+1,r,mid+1,qr));
	}
	else if(qr<=mid){return query(x<<1,l,mid,ql,qr);}
	else{return query(x<<1|1,mid+1,r,ql,qr);}
}
int main(){
	scanf("%d%d%d",&n,&m,&s);
	solve();
	for(int i=1;i<n;i++){
		int x,y;
		scanf("%d%d",&x,&y);
		add_edge(x,y); add_edge(y,x);
	}
	dfs(s,0);
	build(1,1,amax);
	while(m--){
		int l,r;
		scanf("%d%d",&l,&r);
		if(b[l]>b[r]) swap(l,r);
		printf("%d\n",query(1,1,amax,b[l],b[r]));
	}
	return 0;
}
 ```
##### Getting LCA by *Tarjan*
```c++

```

#### K 短路 K-th Shortest Path
##### A* 求 K 短路 Getting K-th Shortest Path by A-Star Algorithm
 [洛谷 P2901 [USACO08MAR]Cow Jogging G](https://www.luogu.com.cn/problem/P2901)
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
using namespace std;
struct e{
	int to,next,w;
}edge[2][20005];
struct node{
	int x,dis;
	bool operator <(const node &_node)const{return _node.dis<dis;}
};
bool vis[10005]={false};
int dis[10005],n,m,k,cnt[2],head[2][10005],sum=0,ans[105];
struct star{
	int u,val;
	bool operator <(const star &_star)const{return _star.val+dis[_star.u]<val+dis[u];}
};
void add_edge(int op,int u,int v,int w){
	edge[op][++cnt[op]].to=v;
	edge[op][cnt[op]].next=head[op][u];
	edge[op][cnt[op]].w=w;
	head[op][u]=cnt[op];
}
void dijkstra(){
	priority_queue<node>q;
	memset(dis,0x3f,sizeof dis);
	dis[1]=0;
	q.push((node){1,0});
	while(!q.empty()){
		int u=q.top().x; q.pop();
		if(vis[u]) continue;
		vis[u]=true;
		for(int i=head[1][u];i;i=edge[1][i].next){
			int v=edge[1][i].to,w=edge[1][i].w;
			if(dis[u]+w<dis[v]){
				dis[v]=dis[u]+w;
				q.push((node){v,dis[v]});
			}
		}
	}
}
void A_star(){
	priority_queue<star>q;
	q.push((star){n,0});
	while(!q.empty()){
		int u=q.top().u,val=q.top().val; q.pop();
		if(u==1){
			ans[++sum]=val;
			if(sum==k) return;
			continue;
		}
		for(int i=head[0][u];i;i=edge[0][i].next){
			int v=edge[0][i].to,w=edge[0][i].w;
			q.push((star){v,val+w});
		}
	}
}
int main(){
	scanf("%d%d%d",&n,&m,&k);
	for(int i=1;i<=m;i++){
		int u,v,w;
		scanf("%d%d%d",&u,&v,&w);
		add_edge(0,u,v,w);
		add_edge(1,v,u,w);
	}
	dijkstra();
	A_star();
	for(int i=1;i<=sum;i++) printf("%d\n",ans[i]);
	for(int i=sum+1;i<=k;i++) puts("-1");
	return 0;
}
````

### 其他 Other Algorithms

#### 龟速乘 Turtle Speeded Exponentiation
若模数>1e9时，快速幂会爆掉，要使用龟速乘
```c++
typedef long long ll;
ll quick_mul(ll a,ll b,ll mod){
	ll ans=0;
	while(b){
		if(b&1) ans=ans+a,ans%=mod;
		a=a+a,a%=mod;
		b>>=1;
	}
	return ans;
}
ll quick_pow(ll a,ll b,ll mod){
	ll res=q;
	while(b){
		if(b&1) res=quick_mul(res,a,mod),res%=mod;  //这里相当于原来快速幂的 res*a%mod ，只不过把 res*a 换成了 res+res+res...(a 个 res)
		a=(a,a,mod),a%=mod;
		b=b>>1;
	}
	return res;
}
```
For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

