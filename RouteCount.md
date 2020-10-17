# 牛客国庆派对2020Day1

## 题目大意

给定若干字符串（总长度的和小于$100$），构造一个长度为$n$的字符串使得这个字符串不包含给定的字符串，求方案数。

## 题目分析

$step 1：$根据给定的字符串，建立$AC$自动机。


$step2：$设$DP$方程为，$f[i][j]$表示字符串的长度为$i$时，第$i$个字符是$AC$自动机上的第$j$号结点的字符。很明显，$f[i][j]$可以从所有$f[k][j]$转移过来，$k$是树上所有能够到达$i$的点。

$step3：$构建矩阵。$a[i][j]$表示从$i$号结点走$1$步能到达$j$号结点的方案数。$a[i][j]$的$n$次幂即为从$i$号结点走$n$步能到达$j$号结点的方案数，$a[0][j]$的和即为答案。

```
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int maxn=1e5+10;
const LL mod=1e9+7ll;
int cnt;
int trie[maxn][30],cntword[maxn],fail[maxn];

class Matrix {
    public:
    LL a[106][106];
    int n;
 
    Matrix (int ss,int val=0) {
        n=ss;
        for (int i=0;i<n;++i)
            for (int j=0;j<n;++j) {
                a[i][j]=val;
            }
    }
 
    Matrix (const Matrix & b) {
        n=b.n;
        for (int i=0;i<n;++i) {
            for (int j=0;j<n;++j) {
                a[i][j]=b.a[i][j];
            }
        }
    }
 
    Matrix operator * (const Matrix & b) {
        Matrix tmp(this->n);
        for (int i=0;i<n;++i)
            for (int j=0;j<n;++j)
                for (int k=0;k<n;++k)
                    tmp.a[i][j]=(a[i][k]*b.a[k][j]%mod+tmp.a[i][j])%mod;
        return tmp;
    }
 
    void print() {
        for (int i=0;i<n;++i) {
            for (int j=0;j<n;++j) {
                printf("%lld%c",a[i][j],j==n-1?'\n':' ');
            }
        }
    }
};

Matrix qpower(Matrix a, LL b) {
    Matrix tmp(a.n);
    for (int i=0;i<a.n;++i) tmp.a[i][i]=1;
    while (b) {
        if (b&1) tmp=tmp*a;
        a=a*a;
        b>>=1;
    }
    return tmp;
}

void insertWords(string s)
{
    int root=0;
    for (int i=0;i<(int)s.length();i++)
    {
        int next=s[i]-'a';
        if (!trie[root][next]) trie[root][next]=++cnt;
        root=trie[root][next];
    }
    cntword[root]++;//不可访问标记
}

void getFail()
{
    queue <int> q;
    for (int i=0;i<26;i++)
    {
        if (trie[0][i])
        {
            fail[trie[0][i]]=0;
            q.push(trie[0][i]);
        }
    }

    while (!q.empty())
    {
        int now=q.front();
        q.pop();

        for (int i=0;i<26;i++)
        {
            if (trie[now][i])
            {
                fail[trie[now][i]]=trie[fail[now]][i];
                q.push(trie[now][i]);
            }
            else trie[now][i]=trie[fail[now]][i];
            cntword[trie[now][i]]|=cntword[fail[trie[now][i]]];//fail路径上的点都不可访问
        }
    }
}

int main() {
	int n,m;scanf("%d%d",&n,&m);
	for (int i=1;i<=m;i++) {
		int L;scanf("%d",&L);
		string ch;cin>>ch;
		insertWords(ch);
	}
    
    getFail();
    Matrix base(cnt+1);
    for (int i=0;i<=cnt;i++) {
        for (int j=0;j<26;j++) {
            if (cntword[trie[i][j]]) continue;
            base.a[i][trie[i][j]]++;
        }
    }
    base=qpower(base,n);
    LL ans=0;
    for (int i=0;i<base.n;i++) ans=(ans+base.a[0][i])%mod;
    printf("%lld\n",ans);

	return 0;
}
```