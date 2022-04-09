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

#### RMQ问题之ST表 模板
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
For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Code-1st-quantum/Code-1st-quantum.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

