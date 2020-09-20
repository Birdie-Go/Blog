# CodeForces1083A

## 题目大意

求树上一条路径使得点权减边权的值最大。

## 题目分析

显而易见的树形$DP$。

```
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int maxn=3e5+10;
LL ans,w[maxn],f[maxn],g[maxn];
vector <pair<int,LL>> e[maxn];

void getM(LL &a,LL &b,LL c) {
    if (c<=0) return;
    if (c>a) {b=a;a=c;}
        else if (c>b) b=c;
}

void dfs(int k,int dad) {
    f[k]=w[k];
    LL m1=0,m2=0;
    for (auto i:e[k]) {
        int to=i.first;LL d=i.second;
        if (to!=dad) {
            dfs(to,k);
            f[k]=max(f[k],f[to]-d+w[k]);
            getM(m1,m2,f[to]-d);
        }
    }
    //printf("%d:%lld %lld %lld\n",k,m1,m2,f[k]);
    ans=max(ans,m1+m2+w[k]);
}

int main() {
    freopen("main.in","r",stdin);
    freopen("main.out","w",stdout);

    int n;scanf("%d",&n);
    for (int i=1;i<=n;i++) scanf("%lld",&w[i]);

    for (int i=1;i<n;i++) {
        int x,y;LL d;scanf("%d%d%lld",&x,&y,&d);
        e[x].push_back({y,d});
        e[y].push_back({x,d});
    }

    dfs(1,1);
    printf("%lld\n",ans);

    return 0;
}
```