# CodeForces1083B The Fair Nut and Strings

https://blog.csdn.net/qq_41046771/article/details/88901663
```
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int maxn=5e5+10;
char a[maxn],b[maxn];

int main() {
    freopen("main.in","r",stdin);
    freopen("main.out","w",stdout);

    int L,n;scanf("%d%d",&L,&n);
    scanf("%s%s",a,b);

    LL ans=0,flo=0;
    for (int i=0;i<L;i++) {
        flo=2*flo+(b[i]-'a')-(a[i]-'a');
        flo=min(flo,(LL)n);
        ans+=min(flo+1,(LL)n);
    }
    printf("%lld\n",ans);

    return 0;
}
```