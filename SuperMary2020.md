```
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    /**
     * @param s: number string
     * @return: Find the key
     */
    int key(string &s) {
        // write your code here
        int L=s.size(),ans=0;
        int Min[10]={0},cnt[10]={0},can[10]={0};
        for (int a=0;a<10;a++)
        {
            int tot=0;
            for (int j=0;j<10;j++) Min[j]=0,cnt[j]=0,can[j]=0;
            for (int i=0;i<L;i++)
            {
                if (s[i]-48==a)
                {
                    tot++;
                    for (int j=0;j<10;j++)
                        if (tot>0 && j!=a)
                            ans=max(ans,cnt[j]-tot-can[j]);
                    for (int j=0;j<10;j++) can[j]=Min[j];
                    for (int j=0;j<10;j++) Min[j]=min(Min[j],cnt[j]-tot);
                }
                else
                {
                    cnt[s[i]-48]++;
                    if (tot>0) ans=max(ans,cnt[s[i]-48]-tot-can[s[i]-48]);
                }
                
            }
        }
        return ans;
    }
};

int main() {
    freopen("main.in","r",stdin);
    freopen("main.out","w",stdout);

    string s="11100";
    Solution a;
    printf("%d\n",a.key(s));

    return 0;
}
```