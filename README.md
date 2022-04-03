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
```markdown
Trie树 模板/洛谷 P2580
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

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Code-1st-quantum/Code-1st-quantum.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

