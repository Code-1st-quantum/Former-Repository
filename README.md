## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/Code-1st-quantum/Code-1st-quantum.github.io/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- George Washington
- John Adams
- Thomas Jefferson

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)

```
# C++ Part

## 模板

### 数据结构

#### Trie树 模板
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

#### 01 Trie 模板
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

#### 小根二叉堆 模板
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

#### RMQ 问题之 ST 表 模板
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

#### 线段树 RMQ 方式求 LCA
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
 
#### 线段树之单点修改区间询问
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

### 数论

#### (素数)线性筛
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

#### 扩展欧拉定理(a ^ b % c 问题，b 为极大数)

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

#### 中国剩余定理(CRT)
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

#### 扩展中国剩余定理(EXCRT)
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
 
#### 扩展欧几里得算法及其通解(EXGCD)
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
 
#### 乘法逆元变形：ax≡1(mod b)求极解
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

#### 线性求乘法逆元
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

#### 线性筛求欧拉函数
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
For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Code-1st-quantum/Code-1st-quantum.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

