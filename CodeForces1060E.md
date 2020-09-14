# CodeForces1060E

## 题目大意
给一棵树，若两个点有公共节点，则这两个点连边，求所有点对的距离和。

## 思路分析
考虑一条新边，它的贡献是新边两端的点对数。
考虑旧边，若两个节点的距离为奇数，那必定经过旧边，贡献为奇数深度的点与偶数深度点的点对数。

```
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int maxn=2e5+10;
int dep[maxn];
LL cnt,siz[maxn];
vector <int> e[maxn];

void dfs(int k,int dad) {
    dep[k]=dep[dad]+1;
    siz[k]=1ll;
    for (auto i:e[k])
        if (i!=dad) {
            dfs(i,k);
            siz[k]+=siz[i];
        }
}

int main() {
    freopen("main.in","r",stdin);
    freopen("main.out","w",stdout);

    int n;scanf("%d",&n);
    for (int i=1;i<n;i++) {
        int x,y;scanf("%d%d",&x,&y);
        e[x].push_back(y);
        e[y].push_back(x);
    }

    dfs(1,1);
    LL ans=0;
    for (int i=1;i<=n;i++) {
        ans+=siz[i]*((LL)n-siz[i]);
        if (dep[i]%2==1) cnt++;
    }
    ans+=cnt*(LL)(n-cnt);

    printf("%lld\n",ans/2);

    return 0;
}
```